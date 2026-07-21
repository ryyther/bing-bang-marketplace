# Budget & hours distribution

How to turn a signed proposal into per-task time estimates in Bonsai. This is the step that makes
every project's estimated hours add up to what was actually sold — so utilization and margin reports
stay honest. Run it after the tasks exist (template applied + first task created), before hand-off.

> **Hard rule:** always confirm the numbers with the operator before writing estimates. Never
> silently assume a target. Show the math every time.

## The distributable-hours formula

```
distributable hours = (total sale price − billed expense amounts) ÷ blended rate
```

- **Total sale price** — the grand total on the accepted proposal (what the client pays).
- **Billed expense amounts** — for each pass-through expense/contractor line, the amount we **bill the
  client** (not our cost). Expenses are a pass-through, not internal labor, so the client-billed amount
  comes out before we distribute hours. Ask the operator for two numbers per line — what we **pay the
  contractor** (cost) and what we **charge the client** (billable) — and subtract the **billable**
  amount here. (When cost = charge, i.e. no markup, the two are the same.) The cost-vs-billable split
  is what makes Bonsai show the client the right number — see "Expenses & markup" below.
- **Blended rate** — Bing Bang's standard internal rate. **Default: `$175/hr`.** This is baked in as
  the standard; still show it in the math and let the operator override for a one-off if they say so.

Convert to minutes for Bonsai: `distributable minutes = round(distributable hours × 60)`. Bonsai's
`update_task` / `create_task` take `time_estimate_in_minutes` (positive integer; `null` clears it).

### Worked example (MAN-2678, the reference project)

- Total sale price: **$20,780**
- Expense (one outside contractor, billed to client at cost — no markup): cost **$3,000**, billable **$3,000**
- Blended rate: **$175/hr**
- Distributable hours = (20,780 − 3,000) ÷ 175 = **101.6 h = 6,096 minutes**

Every internal task's `time_estimate_in_minutes` must sum to **6,096**.

## Contractor-handled (and client-handled) tasks get ZEROED

Any task delivered by an outside contractor — or handled directly by the client — carries **no
internal hours**. Set `time_estimate_in_minutes = null`. The billed expense already left the pool in
the formula above; leaving hours on those tasks would double-count and inflate the estimate.

In the reference project two tasks were zeroed:
- the "confirm contractor capture" coordination task (client handles it), and
- the "contractor capture" shoot task (the $3,000 contractor delivers it).

Ask the operator which tasks, if any, are contractor- or client-delivered, and zero those first.

## Distributing the pool

1. **Anchor the known tasks.** Production and post tasks usually have real, estimable durations
   (shoot day, long-form edit, motion graphics, reels edit, etc.). Set those to the honest estimate.
2. **Fund the kickoff + discovery tasks.** The kickoff meeting is the first task — give it a real
   block (4h in the reference project; see SKILL.md for the time-log subtask). Discovery items like
   "collect org charts / historical assets" get a modest real estimate (≈1.5h in the reference).
3. **Distribute the remainder** across the pre-production and granular tasks that shipped with no
   hours — especially the run-of-show / planning tasks — until the pool is exhausted. Small
   delivery/coordination tasks can take short placeholder estimates (a handful of minutes each).
4. **Never exceed the pool.** If anchored tasks already sum above the distributable total, stop and
   flag it to the operator — the sale price or expense figure is probably off.

## Expenses & markup (so Bonsai bills the client correctly)

Bonsai tracks each expense with a **cost** (what we pay) and a **billable amount** (what the client
pays). Getting both right is what makes the client see the correct charge and keeps margin honest.

For each expense/contractor line, capture **both** numbers from the operator:
- **Cost** = what we pay the contractor.
- **Billable amount** = what we charge the client.
- **Markup %** = (billable − cost) ÷ cost × 100 — record it for reference.

In the Bonsai project's **Expenses tab**, enter the **cost** in the cost field and the **billable
amount** in the billable field. Bonsai shows the client the billable amount. There is no expense or
`update_project` API, so enter expenses and the project budget number **via the browser** (SKILL.md
Hard Rule 5). Remember: the hours formula above subtracts the **billable** amount, not the cost.

## Verify before hand-off

Re-list the project's tasks with `list_tasks` and sum `time_estimate_in_minutes` across all of them.
It must equal the distributable minutes exactly (6,096 in the reference). If it doesn't, adjust the
flex tasks until it does. Report the final total in hours to the operator as part of the recap.

> API notes: `create_task` / `update_task` have no field for a task-list/section or for estimate
> rollups — the sum is something you compute yourself by listing tasks. There is no `update_project`,
> so the project-level budget number and the expense lines (cost + billable/markup) are set in the
> Bonsai UI via the browser, not the API.
