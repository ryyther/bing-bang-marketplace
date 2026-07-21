# Bonsai task-template catalog

Bonsai's MCP can't read or apply task templates, so these are applied in **Bonsai's UI** (which
preserves estimates and the month-by-month task-list grouping the API can't reproduce). This catalog
maps Bing Bang project types to the canonical templates. Live source of truth:
`app.hellobonsai.com/task_templates`. Snapshot captured 2026-06-22.

> **Hard rule:** always ask the operator which template to use, or whether to build a new one. Never
> silently default.

## Canonical library (the `A_` prefix = active/standard)

| Template name                                   | Tasks | Use for                                  |
| ----------------------------------------------- | ----- | ---------------------------------------- |
| A_MASTER - Social Media First Month             | 16    | Social — first month of a new engagement |
| A_MASTER Social Media - Recurring Monthly       | 13    | Social — every recurring month after     |
| A_CLE Monthly Social Media                      | 18    | Client-specific social (CLE) reference    |
| A_McKay Social Media Monthly                    | 14    | Client-specific social (McKay) reference  |
| A_Burkhead_SocialMedia                          | 11    | Client-specific social (Burkhead) ref     |
| A_Full Day Video Shoot                          | 15    | Video — full day shoot                    |
| A_1/2 Day Video Shoot                           | 15    | Video — half day shoot                    |
| FPV Drone Tour                                  | 7     | Video — FPV drone tour add-on             |
| A_Website Build                                 | 26    | Website — full build                      |
| A_Web Hosting & Maintenance                     | 3     | Website — ongoing hosting/maintenance     |
| A_Brand Workshop                                | 13    | Branding — discovery workshop             |
| Brand Launch                                    | 4     | Branding — new brand launch               |
| Brand Refresh                                   | 4     | Branding — refresh of existing brand      |
| Digital Campaign                                | 2     | Campaign — digital campaign scaffold      |
| A_Digital Media Management_No In-House Creative  | 4     | Digital media mgmt (client has no creative)|
| Email - Monthly Email Send (2 emails per month) | 3     | Email — recurring monthly sends           |
| A_Burkhead_Newsletter                           | 8     | Newsletter (Burkhead) reference           |
| A_McKay_Newsletter                              | 10    | Newsletter (McKay) reference              |
| Creative Strategy Meeting                       | 2     | Add-on — strategy meeting                 |
| A_Generic Project                               | 17    | Catch-all default for anything custom     |

## Project type → recommended template

- **Social Media** → first engagement: `A_MASTER - Social Media First Month`; ongoing months:
  `A_MASTER Social Media - Recurring Monthly`. (Client-specific `A_CLE` / `A_McKay` / `A_Burkhead`
  variants exist as references.)
- **Video** → `A_Full Day Video Shoot` or `A_1/2 Day Video Shoot`; add `FPV Drone Tour` if relevant.
- **Website** → `A_Website Build`; ongoing → `A_Web Hosting & Maintenance`.
- **Branding** → `A_Brand Workshop`, then `Brand Launch` or `Brand Refresh`.
- **Campaign** → `Digital Campaign`.
- **Email / Newsletter** → `Email - Monthly Email Send`.
- **Anything else / complex / custom** → `A_Generic Project`, or build a new template and (manually)
  save it in Bonsai for reuse.

## Notes on structure

- Templates store: task name, priority, estimate, start/due date, tags, service, billing, assignee,
  status. Many fields are intentionally blank in the template (a scaffold) and get filled per project.
  In particular, template estimates are a starting point — the spin-up **overwrites them** during the
  hours distribution (SKILL.md Step 6 / `budget-and-hours.md`) so the total matches the sold hours.
- Recurring social projects use **multiple task lists, one per month** (e.g. "June", "July"). That
  grouping is a Bonsai UI feature; apply the template in the UI to keep it.
- On top of the applied template, the spin-up adds the **kickoff meeting as the first task** —
  "Kickoff meeting — review & finalize creative brief" — with a **time-log subtask** for attendees.
  These are created via the API (SKILL.md Step 5).
