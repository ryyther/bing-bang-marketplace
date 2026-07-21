# Bing Bang Project Spin-Up

A shareable Claude plugin that turns an accepted Bonsai proposal into a fully set-up project — Bonsai
project + tasks with the sold hours distributed across them, a Google Drive folder, and a pre-filled
creative brief — and guides the team through every manual step in between.

## What's inside

**Skills**
- **launch-project** — the on-demand spin-up. Run it when a proposal is accepted. Creates the Bonsai
  project, asks which task template to apply (or builds a new one), distributes the sold hours across
  the tasks at the blended rate, builds the Drive folder (including `_Client Services`), generates a
  pre-filled creative brief Google Doc, links it in the project Overview, and adds the kickoff meeting
  as the first task with a time-log subtask. Approval gate on every side-effectful step.
- **pipeline-runbook** — the full lead→kickoff walkthrough so anyone can run the whole system,
  including the manual sales / CRM / proposal / invoice stages.

## How to use it

Say any of: "spin up the project for [client]", "the [client] proposal was accepted",
"kick off [client]", "launch the [client] [type] project". For the big-picture process, ask
"walk me through our workflow."

## Standing rules baked in

- **Never contacts the client during spin-up** — no emails, no invoice sends, no client notifications;
  every client-notify option is left unchecked.
- **Asks before assigning people** — tasks default to unassigned (assigning can notify).
- **Always asks which task template** to apply (or whether to build a new one).
- **Distributes hours to match the sale** — `(total sale price − contractor charges) ÷ $175` (the
  standard blended rate, overridable), with contractor/client-handled tasks zeroed.

## Connectors required

| Tool         | Role                                   | How to connect                          |
| ------------ | -------------------------------------- | --------------------------------------- |
| Bonsai       | Companies, projects, tasks, team       | Bundled (this plugin) — OAuth on install |
| Google Drive | Active Clients drive, folders, briefs  | Enable the Google Drive connector        |
| HubSpot      | CRM (read-only; entry stays manual)    | Optional — enable if you want lookups    |

## Design notes / known limits (Bonsai beta MCP)

- **No pipeline-stage API** and **no `update_project`** — new projects land in the default stage; the
  skill flags "drag to Awaiting Kick Off", "set the budget number", "log the contractor expense", and
  "paste the brief + budget-sheet links in Overview" as manual (or browser-assisted) steps.
- **No task-template read/apply API** — templates are applied in Bonsai's UI (which preserves
  estimates and monthly task-list grouping). The plugin carries a catalog mapping project types to
  your real templates. The spin-up overwrites template estimates so the total matches the sold hours.
- **`create_task` estimate/section limits** — recurring social's monthly grouping is done in the
  Bonsai UI; per-task estimates are written via `time_estimate_in_minutes`.
- **`list_projects` is intermittently failing (HTTP 500)** in the beta — the skill avoids depending on
  it and uses `create_*` return values instead.
- **Drive `create_file` is inline-only** — it can't upload large binaries (e.g. the proposal PDF), so
  the operator drags those into `_Client Services` manually. Folder copy isn't recursive, so
  new-client folders are rebuilt from the standard subfolder list.

## Customizing

Team roster, Drive IDs, the template catalog, and the budget/hours rules live in
`skills/launch-project/references/`. Update those files as the team, folders, templates, or the
blended rate change.
