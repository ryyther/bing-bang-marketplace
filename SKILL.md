---
name: launch-project
description: >
  Spin up a new Bing Bang project the moment a proposal is accepted in Bonsai. Use when
  someone says "a proposal was accepted", "spin up a project", "kick off [client]", "launch
  the project for [client]", "the [client] proposal got accepted", "set up the new project",
  or "start the [client] [project type] project". Creates the Bonsai project, applies the
  right task template, distributes the sold hours across the tasks at the blended rate, builds
  the Google Drive folder, and drops a pre-filled creative brief linked back to the project —
  guiding the team through every manual step along the way.
---

# Launch Project (Bing Bang spin-up)

This skill runs the moment a proposal is accepted in Bonsai. It performs the automated steps
(project, hours distribution, folder, brief) and walks the operator through the manual ones with
approval gates. It is **on-demand** — a team member runs it; nothing polls in the background.

> Write all chat to the operator in plain language. Confirm before every side-effectful action
> (creating a project, creating folders, creating files, applying tasks, writing time estimates).
> Never assume defaults — ask. ALWAYS ask which task template to use or whether to build a new one
> (a hard rule from Josh).

## HARD RULES (never break these)

1. **Never contact the client during spin-up.** No emails, no invoice sends, no client-facing
   messages, no calendar invites to the client. Spin-up is internal setup only.
2. **Keep every client-notify option UNCHECKED.** In Bonsai and Drive, whenever an action offers to
   notify/share with the client or send anything, leave it off. If a step would notify the client and
   can't be turned off, stop and ask the operator.
3. **Ask before assigning people to tasks.** Default is to leave tasks **unassigned**. Assigning a
   task in Bonsai can trigger a notification to that person, so explicitly ask the operator: "Do you
   want me to assign people now, or leave everything unassigned for you to assign in the UI?" Only
   assign if they say yes, and only to the people they name.
4. **ALWAYS ask which task template to apply** (or whether to build a new one). Never silently default.

## Preconditions (manual — confirm, do not perform)

These happen by hand before this skill runs. Confirm each with the operator; if any is missing,
point them to the `pipeline-runbook` skill and stop:

1. CRM contact entered in HubSpot (manual, by Sales).
2. Company exists in Bonsai (manual). If it does not, you may create it in Step 2.
3. **Proposal accepted in Bonsai** (manual) — this is the trigger.
4. Invoice created and sent in Bonsai (manual).

## Step 1 — Gather inputs

Ask the operator for what's known at acceptance. Use the AskUserQuestion tool for the discrete
choices (project type, billing type, new vs. existing client, which template, assign-people yes/no).
Collect:

- **Client name** and **client CODE** (3-letter prefix used in folders, e.g. WOL, CLE, BUR, MAN).
  If existing client, confirm the CODE from their folder name; if new, agree on one.
- **Project name** (becomes the project title and the folder name).
- **Project type**: Social Media · Video · Campaign · Website · Branding · Other.
- **Billing**: `fixed_fee` (needs fee) · `retainer` (needs fee + cycle) · `time` · `not_billable`.
- **Budget inputs for the hours distribution** (see `references/budget-and-hours.md`):
  - **Total sale price** (the proposal grand total).
  - **Contractor charge(s)** — each outside contractor and the flat dollar amount paid to them.
  - **Blended rate** — default **$175/hr** (Bing Bang standard). Confirm or let them override.
- **Start date** (default today), **target due / launch date** if known.
- **Project lead / owner** and any other known team assignments (see `references/team-roster.md`).
  Remember: assigning is gated on the operator's yes (Hard Rule 3).
- **Source documents**: the exact client-facing **proposal PDF** and the **budget source spreadsheet**
  (the sheet the numbers came from). These go into `_Client Services` (Step 6) and get linked in the
  Overview (Step 8).
- **What's known for the brief**: objective, scope, audience, deliverables, key contacts, links.

## Step 2 — Resolve the Bonsai company

Call `list_companies` (filter by client name) to get the `company_id`.
- Existing client → use the returned id.
- New client → confirm with the operator, then `create_company`, then use the new id.

> Bonsai note: `list_projects` is unreliable in the beta MCP (intermittent HTTP 500). Do NOT depend
> on it. Rely on `create_*` return values and `list_companies` / `list_team_members` instead.

## Step 3 — Create the Bonsai project (gate: confirm first)

Confirm the details, then `create_project` with: `title` = project name, `company_id`,
`billing_type` (+ `billing_fee` / `billing_cycle` as required), `start_date`.
Capture the returned `number` — the project folder name uses `[CODE]-[number]`.

> The API can't set the pipeline stage on create and there is no `update_project` tool. The project
> will land in Bonsai's default stage. Flag to the operator: **drag the project to "Awaiting Kick Off"
> manually** (or do it via the browser — see Step 9).

## Step 4 — Choose the task template (ALWAYS ask)

Show the templates that match the project type from `references/task-template-catalog.md` and ask the
operator to pick one **or** to build a new one. Never silently default. Recommended starting points:

- Social Media (recurring) → `A_MASTER Social Media - Recurring Monthly`
- Social Media (first month) → `A_MASTER - Social Media First Month`
- Video → `A_Full Day Video Shoot` or `A_1/2 Day Video Shoot`
- Website → `A_Website Build`
- Branding → `A_Brand Workshop` (+ `Brand Launch` / `Brand Refresh`)
- Campaign → `Digital Campaign`
- Anything else / custom → `A_Generic Project`, or build a new template

If they choose "build new," help them define the task list, then offer to save it as a new Bonsai
template (manual in Bonsai UI) for reuse.

## Step 5 — Apply the tasks + add the kickoff first task (gate: confirm first)

**Preferred path — apply the template in Bonsai's own UI** (this preserves estimates and the
month-by-month task-list grouping, which the API cannot reproduce). Two ways:

- **Guided manual**: tell the operator exactly which template to apply — Bonsai → the new project →
  Tasks → Apply Template → pick `[chosen template]`.
- **Browser-assisted** (optional, with permission): drive it with the Claude-in-Chrome tools —
  open the project in `app.hellobonsai.com`, apply the named template, set assignees/dates.

Then create the **kickoff meeting as the first task** (top of the list) via `create_task`:
- `title`: "Kickoff meeting — review & finalize creative brief"
- `project_id`: the new project id · `priority`: "high" · `due_date`: 2–3 business days out.
- `assignee_member_id`: **only if the operator said to assign** (Hard Rule 3); otherwise leave unset.

Then create a **time-log subtask** under it so meeting attendees can log their time, via `create_task`:
- `title`: "Kickoff meeting — log time (attendees)"
- `parent_task_uuid`: the kickoff task's uuid (inherits the project; don't also pass `project_id`).
- No time estimate (the parent task carries the kickoff hours). Leave unassigned — attendees assign
  themselves in the UI when they log. Add a short description explaining it's a time-log container.

> API limits to flag: `create_task` has no field for estimates-at-create beyond
> `time_estimate_in_minutes`, and none for placing a task in a named task list/section. Subtasks go max
> 2 levels deep. For recurring social, the monthly task-list grouping is done in the Bonsai UI.

## Step 6 — Distribute the sold hours across the tasks (gate: confirm first)

Follow `references/budget-and-hours.md`. In short:

1. Compute **distributable minutes** = round( (total sale price − contractor charges) ÷ blended rate × 60 ).
   Show the math to the operator (e.g. (20,780 − 3,000) ÷ 175 = 101.6h = 6,096 min).
2. **Zero the contractor/client-handled tasks**: `update_task` with `time_estimate_in_minutes = null`
   on any task an outside contractor or the client delivers. Ask which ones first.
3. **Anchor** the real production/post tasks to honest estimates; give the **kickoff** task a real
   block (e.g. 4h) and discovery tasks (e.g. "collect org charts / historical assets") a modest one.
4. **Distribute the remainder** across the no-hour pre-pro/planning tasks (especially the run-of-show)
   until the pool is exhausted. Small delivery/coordination tasks get short placeholder estimates.
5. **Verify**: `list_tasks` and sum `time_estimate_in_minutes` — it must equal distributable minutes.
6. Log the **contractor flat fee** under the Bonsai project's Expenses tab and set the project budget
   number in the UI (no `update_project` API) so margin reads correctly. Flag both as manual touches.

## Step 7 — Build the Google Drive folder + _Client Services (gate: confirm first)

Follow `references/drive-structure.md`. In short:

- **Existing client** → `search_files` for their client folder under Active Clients
  (`parentId = '0AE7-flqLz3G9Uk9PVA'`), then `create_file` a project subfolder inside it named
  `[CODE]-[number] - [Project Name]` (mimeType `application/vnd.google-apps.folder`).
- **New client** → `create_file` a client folder `[CODE] - [Client Name]` under Active Clients, then
  create the standard subfolders (`_Library`, `_admin`, `_Digital`, `_Client Services`, the project
  subfolder `[CODE]-[number] - [Project Name]`, `zz_Archive`).
- **`_Client Services`** holds the client-facing paperwork: the exact **proposal PDF** and the
  **budget source spreadsheet**. The Drive MCP can only create files from inline text/base64 — it
  **cannot upload large binaries**. So: create the folder, then tell the operator to **drag the exact
  proposal PDF into `_Client Services` manually** (and the spreadsheet, unless it's a Google Sheet you
  can link). Capture the folder + file links for Step 8.

## Step 8 — Create the pre-filled creative brief (gate: confirm first)

Build the brief text from `references/brief-template.md` (general brief for most work; the video
brief for Video projects), filling every field known from Step 1 and marking the rest `TBD`.
`create_file` it as a Google Doc (`textContent` + `contentMimeType: text/plain`, which auto-converts)
inside the project subfolder, titled `[CODE]-[number] _ Kickoff & Creative Brief`. Capture its `viewUrl`.

## Step 9 — Link documents in the project Overview + finalize stage

Bonsai's Overview/description is a UI field and the MCP has no `update_project`, so this is manual or
browser-assisted. Add a **"Documents & Links"** block to the Overview with: the brief doc, the Drive
project folder, and the **budget source spreadsheet**. Keep any client-notify/share option UNCHECKED.
- **Guided manual**: tell the operator to paste those links into the project's Overview, and to drag
  the project to **Awaiting Kick Off**.
- **Browser-assisted** (optional, with permission): do both with the Claude-in-Chrome tools.

## Step 10 — Recap, cleanup, and hand off

Post a short recap with live links: Bonsai project, Drive project folder, `_Client Services`, brief
doc, the kickoff task + time-log subtask, and the final **distributed hours total** (in hours).
Remind the operator of the remaining manual touches:
- assign template tasks to people (if not done — Hard Rule 3), set due dates;
- confirm the project is in "Awaiting Kick Off";
- set the project budget number + log the contractor expense;
- drag the proposal PDF into `_Client Services`;
- **clean up any stray budget/working docs** created during the process (trash scratch files).

The kickoff task's owner finalizes the brief and schedules the kickoff meeting with the pertinent team
members; attendees log time against the time-log subtask.

## References

- `references/budget-and-hours.md` — the distributable-hours formula ($175 default), contractor
  zeroing, and the verification step.
- `references/team-roster.md` — names → `company_member_id` for assigning tasks.
- `references/task-template-catalog.md` — the Bonsai template library and project-type mapping.
- `references/drive-structure.md` — Active Clients root, Template Master, `_Client Services`, naming, IDs.
- `references/brief-template.md` — the fill-in brief (general + video) to generate the Google Doc.
