---
name: pipeline-runbook
description: >
  The full Bing Bang new-business-to-kickoff runbook. Use when someone asks "how does our process
  work", "walk me through the workflow", "how do I run the system", "what are the steps from lead to
  kickoff", "onboard me to our process", or "where does the automation fit". Explains every stage —
  the manual sales/CRM/proposal/invoice steps and the automated project spin-up — so anyone on the
  team can run the whole thing end to end.
---

# Bing Bang pipeline runbook (lead → kickoff)

This is the operator's guide to the whole workflow. Most early stages are **manual by design**; the
automated core is the project spin-up, handled by the `launch-project` skill. Walk the person through
the stage they're on; don't perform manual steps for them unless they ask.

## The pipeline at a glance

```
1. Sales captures lead        → HubSpot (MANUAL)
2. Lead qualified, ready      → create Company in Bonsai (MANUAL)
3. Build & send Proposal      → Bonsai (MANUAL)
4. Proposal ACCEPTED          → ** trigger **
5. Create & send Invoice      → Bonsai (MANUAL)
6. Spin up the project        → run `launch-project` skill (AUTOMATED + guided)
      • Bonsai project created
      • task template applied (you choose)
      • sold hours distributed across tasks at the blended rate ($175 default)
      • Drive folder built (incl. _Client Services)
      • pre-filled creative brief created + linked
      • first task = Kickoff meeting — review & finalize creative brief
7. Finalize brief + kickoff   → owner of the first task (MANUAL, scheduled at spin-up)
```

> **Standing rule across the whole spin-up:** nothing goes to the client automatically. No emails, no
> invoice sends, no client notifications. The `launch-project` skill keeps every client-notify option
> unchecked and **asks** before assigning any task to a person (assigning can trigger a Bonsai notice).

## Stage detail

### 1. Lead capture — HubSpot (manual)
Sales enters the contact/company in HubSpot. The team does this by hand; the system does not write to
HubSpot. Nothing to automate here.

### 2. Company in Bonsai (manual)
When a lead is ready to quote, create the company in Bonsai. (The `launch-project` skill can create it
later if it's still missing at spin-up time.)

### 3. Proposal (manual)
Build and send the proposal in Bonsai. This stays manual — the team controls scope and pricing.

### 4. Proposal accepted — the trigger
This is the single event that starts the automation. The spin-up is **on-demand**: a team member runs
`launch-project` when a proposal is accepted. Nothing polls in the background.

### 5. Invoice (manual)
Create and send the invoice in Bonsai. Stays manual.

### 6. Spin up the project — run the `launch-project` skill
Say something like "spin up the project for [client]" or "the [client] proposal was accepted." The
skill will: gather inputs (including the total sale price and any contractor charges), create the
Bonsai project, ask **which task template** to apply (or build a new one), **distribute the sold hours
across the tasks** (total sale price − contractor charges ÷ $175, with contractor/client-handled tasks
zeroed), build the Google Drive folder (including a `_Client Services` folder for the proposal + budget
sheet), create a pre-filled creative brief as a Google Doc, link it in the project Overview, and add
the first task — "Kickoff meeting — review & finalize creative brief" with a time-log subtask for
attendees. You approve each step, and it asks before assigning people.

Known manual touches the skill will flag (Bonsai MCP limits):
- Drag the new project to the **"Awaiting Kick Off"** stage (no API for pipeline stage).
- Apply the chosen task template in Bonsai's UI to keep estimates + monthly task-list grouping.
- Set the project budget number and log the contractor flat fee under Expenses (no `update_project` API).
- Paste the brief + budget-sheet links into the project Overview (no `update_project` API) — or let the
  skill do these via the browser if you grant access.
- Drag the exact client-facing proposal PDF into `_Client Services` (Drive MCP can't upload large binaries).

### 7. Finalize brief + kickoff (manual, already scheduled)
The first task's owner fills in any remaining brief fields and schedules the kickoff meeting with the
pertinent team members. Attendees log their meeting/prep time against the kickoff time-log subtask.

## What's connected
- **Bonsai** (bundled MCP) — companies, projects, tasks, team, invoices.
- **Google Drive** (connector) — Active Clients shared drive, folders, brief docs.
- **HubSpot** (optional connector) — read-only; CRM entry stays manual.

If a stage is blocked because a tool isn't connected, tell the operator which connector to enable.
