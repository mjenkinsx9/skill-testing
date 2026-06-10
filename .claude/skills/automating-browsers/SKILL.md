---
name: automating-browsers
description: Drives a live browser via the Playwright MCP plugin to navigate, log in, fill forms, click through UI flows, and extract page content. Use when the user wants to interactively operate a real browser against any web app. Do NOT use for writing Playwright scripts (that's code-writing) or for cheap knowledge fetches that WebFetch can answer (e.g., "what version is X on npm").
---

# Automating Browsers

## When to use

Real-time interactive browser tasks: "log into my dashboard and grab Z",
"click through the checkout flow", "verify the new build deployed",
"scrape this page."

Skip when:

- Cheap text fetch from a known URL → `WebFetch`.
- The site has a documented API → call it directly.
- Headless scraping at scale → write a Playwright Python script; the
  MCP is for interactive driving.

## Core workflow

The loop is **snapshot → act → verify**, repeated until done.

1. **Navigate.** `browser_navigate(url)`. Inspect the returned URL, title,
   and console-error count for surprises.
2. **Snapshot.** `browser_snapshot()` returns an accessibility tree as
   YAML with `[ref=eN]` references on every interactive element. The ref
   is what you pass to clicks/types.
3. **Act.** Use the most specific tool for the action — `browser_click`,
   `browser_type`, `browser_fill_form` (multi-field), `browser_select_option`,
   `browser_press_key`, `browser_hover`. Always pass `target=<ref>` plus a
   human-readable `element` description so the user can audit.
4. **Verify.** After any state-changing action, re-snapshot or call
   `browser_wait_for(text=...)` to confirm the new state landed (URL
   changed, expected text appears, error banner gone). Don't assume the
   click worked.
5. **Repeat** until the user's goal is met, then `browser_close`.

**Shortcut:** if the app has predictable URLs, `browser_navigate`
directly to the target page instead of clicking through. Faster, and
immune to ref drift when the intermediate snapshots change shape.

Direct-URL recipes from observed apps:

- **Acumatica:** `?CompanyID=<co>&ScreenId=<id>` jumps to any screen.
  `CR3060PL` = Cases list, `CR306000&CaseCD=<id>` = single case,
  `AK303002&AcctCD=<n>` = Support Account.
- **Pure FA:** `/storage/<tab>` (`array`, `volumes`, `hosts`, `pods`,
  `policies`), `/analysis/capacity/<view>`, `/health/<tab>`.
- **GitHub:** for repos/PRs/issues prefer `gh api repos/<owner>/<repo>/...`
  via Bash over browsing — faster and works for private repos.

## Tool selection

| Want to… | Use |
|---|---|
| Load a page | `browser_navigate` |
| See structure / find a ref | `browser_snapshot` (prefer this over screenshot) |
| See pixels | `browser_take_screenshot` (visual verification only — can't act on it; save under `.playwright-mcp/screenshots/<session-stamp>/<name>.png`) |
| Click one thing | `browser_click` |
| Type into one field | `browser_type` (use `submit:true` to press Enter after) |
| Fill several fields at once | `browser_fill_form` |
| Pick a dropdown option | `browser_select_option` |
| Press a key (Tab, Esc, Enter) | `browser_press_key` |
| Wait for text to appear/disappear | `browser_wait_for` |
| Read raw DOM (text, attrs) the tree omits | `browser_evaluate` |
| Multi-tab work | `browser_tabs` |
| Upload a file | `browser_file_upload` |
| Handle a JS alert / confirm / prompt | `browser_handle_dialog` |
| Debug a failure | `browser_console_messages`, `browser_network_requests` |
| End the session | `browser_close` |

For full tool parameters, selector strategy (`ref=eN` vs CSS vs roles),
snapshot tuning, end-to-end patterns (login, multi-step form, content
extraction, multi-tab), and debugging, see
[references/reference.md](references/reference.md).

### Worked example: search → click result

1. `browser_navigate(siteUrl)`
2. `browser_click(target=<search-btn-ref>)` — opens search dialog
3. `browser_type(target=<combobox-ref>, text="<query>")`
4. `browser_wait_for(text="<query>")` — wait for results to render
5. Re-snapshot the dialog (`target=<dialog-ref>, depth=6`) → pick the
   top result's option ref
6. `browser_click(target=<option-ref>)` → URL changes to result page

### Extracting structured content

`browser_snapshot` already returns headings, links, list items as a
shaped a11y tree. On heavy pages, clip with `depth:N` and/or scope with
`target:<ref>` — typical pattern: snapshot at `depth:5` to find the main
region ref, then re-snapshot with `target:<that-ref>` at higher depth.

For text/attrs the tree omits, drop into `browser_evaluate` with a small
JSON-serializable function:

```js
() => Array.from(document.querySelectorAll('article h2, article h3'))
        .map(h => `${h.tagName}: ${h.textContent.trim()}`)
```

### Screenshot session lifecycle

Self-contained cleanup that travels with the skill — no scheduled task,
no per-machine setup. Run once at the **first** `browser_take_screenshot`
of a session, then reuse the printed path for every subsequent screenshot.

```bash
SCREENSHOT_DIR=".playwright-mcp/screenshots"
STAMP=$(date +%Y-%m-%d-%H%M)
find "$SCREENSHOT_DIR" -mindepth 1 -maxdepth 1 -type d -mtime +7 \
    -exec rm -r {} + 2>/dev/null || true
mkdir -p "$SCREENSHOT_DIR/$STAMP"
echo "$SCREENSHOT_DIR/$STAMP"
```

Use the printed path as the screenshot filename prefix: `browser_take_screenshot(filename="$PRINTED_PATH/<name>.png")`.
Works on any shell with `find` and `date` (Git Bash on Windows, native
Bash on macOS/Linux). The 7-day retention is a default — adjust the
`-mtime +7` if you need a longer window.

## Credentials

Never hardcode, commit, or echo passwords/tokens/keys. Treat any
user-supplied credentials as session-only. **Unexpected login wall
mid-task → stop** — don't infer credentials from anywhere or try to
bypass the wall.

## Pitfalls observed in real runs

- **Link clicks may not redirect.** After clicking an `<a href>`, if
  `browser_wait_for(text=...)` times out OR the URL hasn't changed:
  read the `/url:` value from the link's snapshot entry, then
  `browser_navigate(href)` explicitly to complete the redirect.
- **`browser_wait_for(text=,time=N)` ignores `time` when `text` is set** —
  use `time:N` alone for plain delays.
- **Refs are per-DOM-state.** Re-snapshot after navigation; refs shift
  on overlay/dialog open. Same-origin iframes get `f3eN`/`f9eN`
  namespace (descend via target). Cross-origin iframes and shadow DOM
  need `browser_evaluate`.
- **Virtualized tables only render visible rows.** "1-10 of 46" means
  36 rows aren't in the DOM. Workaround ladder: (1) scroll the virt
  container in JS, (2) use a less-granular view (Pods/Hosts page that
  summarizes), (3) hit the product's REST API. If the API 403s on web
  session cookies, accept partial data and tell the user.
- **`browser_evaluate(().click())` may not fire framework handlers.**
  React/custom widgets often need the real Playwright event — retry
  with `browser_click(target=<ref>)`.
- **Canvas / unlabeled-icon UIs have no a11y leverage** — screenshot,
  map element by visual position, then click the ref.
- **Console-error count jumps signal trouble** — glance after each act.
- **`.playwright-mcp/` is created in cwd** — gitignore it. Screenshots
  belong under `.playwright-mcp/screenshots/<session-stamp>/<name>.png`. See
  the **screenshot-lifecycle** example below for the one-time setup that
  prunes folders older than 7 days and creates the new session subdir.

## Output

After completing the user's task, summarize:

```
## What I did
<1-3 bullets of actions taken, with URLs>

## What I found / verified
<concrete result — text extracted, form submitted, login succeeded, etc.>

## State
<browser closed | left open at <URL> for follow-up>
```

If the task failed (page didn't load, element missing, login rejected),
stop early, report the failure cleanly with the console + network
evidence, and ask the user how to proceed. Don't fabricate "results"
from partial state.

