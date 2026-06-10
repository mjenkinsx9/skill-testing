# Reference: Playwright MCP tools

Full parameter reference and selection strategy. Loaded only when this
skill is actively driving a browser.

## Contents

- Selector strategy: `ref=eN` vs CSS selectors vs roles
- Snapshot tuning for big pages
- Tool catalog with parameters
- Patterns: login, multi-step form, content extraction, multi-tab
- Debugging failures

## Selector strategy

Every `target:` parameter accepts either:

- **`eN` reference** from the most recent snapshot. Preferred — the MCP
  resolves it to a stable Playwright locator (`getByRole`,
  `getByPlaceholder`, etc.) automatically. Refs are valid for the current
  page state; they reset on navigation and may shift when overlays open.
- **CSS selector** (e.g. `button[type=submit]`, `#login-form input[name=user]`).
  Use when no snapshot ref is available or when a ref keeps drifting.
  Must uniquely identify one element.

When in doubt, snapshot first. The accessibility tree is the source of
truth — picking a `ref` from it is more robust than guessing CSS.

## Snapshot tuning

`browser_snapshot` parameters:

| Param | When to use |
|---|---|
| `depth:N` | Cap tree depth. `depth:3` shows page skeleton; `depth:8+` shows leaf text. |
| `target:<ref>` | Scope to a subtree. Combine with depth to drill in. |
| `filename:<path>` | Write the snapshot to a file instead of returning it. Use for huge pages you'll grep through. |
| `boxes:true` | Include bounding boxes — only needed for layout/visual debugging. |

Recipe for a heavy page:

1. `browser_snapshot(depth:3)` → find the `<main>` or content region ref.
2. `browser_snapshot(target:<main-ref>, depth:6)` → see headings + structure.
3. `browser_snapshot(target:<specific-ref>, depth:10)` → drill in to act on a form/widget.

## Tool catalog

### Navigation

- `browser_navigate(url)` — initial load or explicit redirect.
- `browser_navigate_back()` — browser back button.
- `browser_tabs(action: list|new|close|select, index?, url?)` — tab management.
- `browser_close()` — end session; closes the page.

### Inspection

- `browser_snapshot(depth?, target?, filename?, boxes?)` — accessibility tree.
- `browser_take_screenshot(type: png|jpeg, fullPage?, target?, filename?)` —
  pixels. Cannot drive actions; for visual verification or sending the
  user a picture.
- `browser_console_messages(level: error|warning|info|debug, all?, filename?)`
  — console output. Default returns since last navigation; `all:true`
  for the full session.
- `browser_network_requests(static:false, filter?, filename?)` — list
  requests. `filter` is a regex on the URL (e.g. `/api/.*user`).
- `browser_evaluate(function, target?, element?, filename?)` — run JS.
  Use sparingly; prefer structured tools. Required when you need
  attributes, computed styles, or text the accessibility tree doesn't expose.

### Acting

- `browser_click(target, element, button?, doubleClick?, modifiers?)` —
  click. `modifiers:["Control"]` for Ctrl-click, etc.
- `browser_type(target, element, text, slowly?, submit?)` — type. `submit:true`
  presses Enter after. `slowly:true` types one char at a time (useful
  for autocomplete handlers that debounce).
- `browser_fill_form(fields: [{target, name, type, value, element}])` —
  fill multiple fields in one call. `type` is `textbox`, `checkbox`,
  `radio`, `combobox`, or `slider`.
- `browser_select_option(target, values, element)` — `<select>` dropdown.
  Pass an array of values to allow multi-select.
- `browser_press_key(key)` — `Enter`, `Tab`, `Escape`, `ArrowDown`, etc.
- `browser_hover(target, element)` — reveal hover-only menus.
- `browser_file_upload(paths: [absolute-paths])` — upload to a file input.
  Omit `paths` to cancel the chooser.
- `browser_handle_dialog(accept, promptText?)` — respond to native
  alert/confirm/prompt. Must be set up before the dialog fires.

### Synchronization

- `browser_wait_for(text?, textGone?, time?)` — wait for state change.
  - `text` waits up to 30s for that string to appear; times out hard if absent.
  - `textGone` waits for it to disappear.
  - `time` is a plain delay in seconds.
  - **Do not combine `text` with `time`** — text takes precedence.

### Misc

- `browser_resize(width, height)` — viewport. For responsive testing.

## Patterns

### Login

```
1. browser_navigate(login_url)
2. browser_snapshot()                  → find username/password/submit refs
3. browser_fill_form([
     {target: <u-ref>, name: "Username", type: "textbox", value: "<user>", element: "Username field"},
     {target: <p-ref>, name: "Password", type: "textbox", value: "<pass>", element: "Password field"},
   ])
4. browser_click(<submit-ref>, "Login button")
5. browser_snapshot()                  → verify URL change + success indicator
```

If a "Remember me" checkbox or 2FA prompt appears between steps 4 and
5, handle each before re-snapshotting.

### Multi-step form / wizard

```
1. browser_navigate(start_url)
2. For each step:
   a. browser_snapshot() / browser_snapshot(target:<form-ref>)
   b. browser_fill_form([...]) or sequential type/select
   c. browser_click(<next-ref>, "Next")
   d. browser_wait_for(text: "<next-step heading>")
3. Final step: click submit, verify success page.
```

### Content extraction

For structured content (headings, list items, links), `browser_snapshot`
gives a clean tree.

For raw text, attributes, or anything the accessibility tree omits, use
`browser_evaluate`:

```js
() => Array.from(document.querySelectorAll('article h2, article h3'))
        .map(h => `${h.tagName}: ${h.textContent.trim()}`)
```

Keep evaluate functions small and pure — return JSON-serializable data,
not DOM nodes.

### Multi-tab work

```
1. browser_tabs(action: "new", url: <second-url>)   → opens tab 1
2. browser_tabs(action: "list")                     → see indices
3. browser_tabs(action: "select", index: 0)         → switch to first tab
4. browser_tabs(action: "close", index: 1)          → close second tab
```

Each tab maintains its own snapshot refs.

## Debugging failures

When an action seems to do nothing or returns surprising state:

1. `browser_snapshot()` — has the page actually changed?
2. `browser_console_messages(level: "error")` — JS errors?
3. `browser_network_requests(static: false, filter: "/api/")` — failed
   XHRs / 4xx / 5xx?
4. `browser_take_screenshot()` — sanity-check the visual state.

Common causes:

- **Element not yet in DOM:** add `browser_wait_for(text: "<expected>")`
  before the action.
- **Element in an iframe:** the MCP doesn't auto-descend into iframes;
  you may need `browser_evaluate` to interact with iframe content, or
  navigate directly to the iframe's `src`.
- **Bot detection / Cloudflare challenge:** the page may render a
  challenge interstitial. Snapshot will reveal it. Most public sites are
  fine; some banking/SaaS portals block automation outright.
- **Stale ref after overlay opened:** re-snapshot. Refs after a dialog
  open may renumber unaffected elements.
