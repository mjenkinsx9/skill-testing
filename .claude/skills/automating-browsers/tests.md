# Tests for automating-browsers

Verification scenarios. All exercised live; the patterns this skill
teaches come directly from these runs. Environment identifiers in the
enterprise scenarios (hostnames, usernames, record numbers) are
anonymized for publication — the runs themselves were real.

Last verified: 2026-05-15

## Golden path: extract section structure from a docs page

**Input:** "Go to the Anthropic skill authoring best-practices page and
list every H2/H3 heading."

**Expected behavior:** `browser_navigate` to the docs URL, then either
(a) `browser_snapshot(depth:3, target:<main>)` and read the heading
entries, or (b) `browser_evaluate` with a `document.querySelectorAll('h2,h3')`
query. Returns the structured list of headings.

**Verified output:** 44 headings (10 H2, 34 H3) extracted from
https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
via `browser_evaluate`. Matched the page's table of contents.

## Form interaction: search + click result

**Input:** "Search the Anthropic docs for 'progressive disclosure' and
open the top result."

**Expected behavior:** Navigate to docs, click the "Search documentation"
button to open the dialog, `browser_type` into the search combobox, wait
for results, snapshot the dialog subtree, click the top result option.
URL changes to the result page.

**Verified output:** Search dialog opened, 19 doc results returned, top
result "Agent Skills" clicked, landed on
`/docs/en/agents-and-tools/agent-skills/overview#how-skills-work`.

## Login flow: fill credentials and verify success

**Input:** "Log into the-internet.herokuapp.com using the test credentials
shown on the page (tomsmith / SuperSecretPassword!) and confirm you
reached the secure area."

**Expected behavior:** Navigate to `/login`, snapshot to find Username
and Password textbox refs, `browser_fill_form` with both fields,
`browser_click` the Login button, re-snapshot, verify URL is `/secure`
and a "You logged into a secure area!" banner appears.

**Verified output:** URL changed to
`https://the-internet.herokuapp.com/secure`, success banner present,
Secure Area heading visible. Login confirmed.

## Edge case: link click that doesn't redirect

**Input:** While logged in to the-internet.herokuapp.com/secure, click
the "Logout" link (an `<a href="/logout">`) and verify return to the
login page.

**Expected behavior:** The click fires, but the redirect may not complete
within `browser_wait_for`'s window. Skill should detect that the URL is
still `/secure` after the wait and fall back to
`browser_navigate('/logout')` explicitly.

**Verified output:** First attempt — `browser_click` on the Logout link
ran but the URL stayed at `/secure` and `browser_wait_for(text:"Login Page")`
timed out at 30s. Recovery — explicit `browser_navigate('/logout')`
landed on `/login` with the form fields back in view. Pattern documented
in SKILL.md "Pitfalls".

## Enterprise app: iframes, unlabeled icons, custom tab strips

**Input:** "Log into our Acumatica ERP at acumatica.example.com, navigate
Favorites → Cases → the My Tickets filter tab, open a specific case,
and extract its Activities log."

**Expected behavior:** Login form on the outer page; after login the
content lives in a nested iframe. Snapshot reveals an `f3eN` / `f9eN`
ref namespace inside the iframe — clickable directly. Left-rail nav
icons have no a11y labels, so screenshot + visual-position mapping
identifies them. Custom tab strips (filter tabs, the case-detail
sub-tabs) need `browser_click(target=ref)` — `evaluate(.click())` fired
but did not change the active tab.

**Verified output:** Login succeeded → `/Main` landed on Support
Accounts. Favorites pane opened (1st icon by position, `e32`/`e97`),
Cases link clicked (`e84`), tab dropdown chevron expanded the overflow
list, My Tickets selected. 9 cases assigned to the test user listed.
Opened one case, switched to Activities tab via MCP click (`f9e429`)
after `evaluate(.click())` was a no-op, extracted 4 activity entries
(1 work note with hours logged + 3 email entries).
Direct-URL navigation (`/storage/volumes`, `?ScreenId=...`) was also
used to skip ref-drift through nav clicks.

## Virtualized table: partial-data unavoidable

**Input:** "Log into a Pure Storage SAN6 array and figure out where most
of the space is being used."

**Expected behavior:** Login → dashboard shows 95% utilization. Storage
> Volumes lists 46 volumes but the table virtualizes and only renders
~10 rows in the DOM. The scroll container is not reachable via
`getComputedStyle(d).overflowY` searches. REST API at `/api/2.x/`
returns 403 to the web session cookie. Skill pivots: enumerate the
authoritative summary at a less-granular view (Pods page = 8 items
complete; Hosts page = host-group summary rows that sum to the array
total) and tell the user about the data gap explicitly.

**Verified output:** Pods page returned all 8 pods cleanly (the largest
local-active pod held 21.79 T unique; the biggest replication-target
pod 21.13 T). Host-group summaries totaled 198.92 T Unique against the
array dashboard's 199.31 T — fully accounted for the macro picture.
Volume-level detail for the ~36 unrendered rows was reported as a known
gap with the likely volume-name prefixes suggested for follow-up.
Pattern documented in SKILL.md "Virtualized tables only render visible
rows".
