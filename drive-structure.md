# Google Drive structure & naming

All client work lives in the **Active Clients** shared drive. The Drive MCP can create folders and
create a Google Doc from text. Note: copying a folder does **not** copy its contents recursively, so
new-client folders are built by creating each subfolder (most are empty in the template anyway).

> **Upload limitation:** `create_file` only accepts **inline** content (text or base64). It **cannot
> upload large binaries** (e.g. the client-facing proposal PDF, a big deck, video). Create the folder
> via the MCP, then have the operator **drag the file in manually**. Google-native files (Sheets/Docs)
> can be linked instead of uploaded.

## Key IDs (captured 2026-06-22)

| Thing                                   | ID                                              |
| --------------------------------------- | ----------------------------------------------- |
| Active Clients (shared drive root)      | `0AE7-flqLz3G9Uk9PVA`                            |
| Template Master client folder           | `191F61GNKKnMCcDaXaQo-Zo4_uwQSwJQt`             |
| General brief template (Google Doc)     | `16d8IHmHbqBA-GSeKUz264Y_IxWG242Dde189mWwZcFE`  |
| Video Production brief template (Doc)    | `1vMOYN4qTQoKs40V8LK0AbMy3JTvqCpGX2Wn01dBSHPo`  |

> The brief is generated fresh and **pre-filled** via `create_file` (text → Google Doc) rather than
> copied blank — see `brief-template.md`. The template IDs above are the formatting reference.

## Standard client-folder layout (mirrors Template Master)

```
[CODE] - [Client Name]/
├── _Library/
├── _admin/
├── _Digital/
├── _Client Services/                   <- client-facing paperwork (proposal PDF + budget sheet)
├── [CODE]-[number] - [Project Name]/   <- the project subfolder (one per project)
└── zz_Archive/
```

## Naming conventions

- **Client folder**: `[CODE] - [Client Name]` (e.g. `WR - Wolf Roofing`, `CLE - CLE Productions`).
- **Project subfolder**: `[CODE]-[number] - [Project Name]`, where `number` is the Bonsai project
  number from `create_project`. This makes the folder name match the Bonsai Project ID.
  Real examples: `CLE-2607 - Social Media June`, `CLE-2629 | Hubspot Implementation`.

## _Client Services folder

Holds the client-facing paperwork for the engagement so it's never mixed with internal working files:
- the **exact proposal PDF** the client accepted, and
- the **budget source spreadsheet** the project numbers came from.

Create the folder via `create_file` (folder mimeType). Because the proposal PDF is a binary the MCP
can't upload, tell the operator to **drag it into `_Client Services` manually**. Link the budget sheet
(or drop a copy in) and capture both links for the project Overview (SKILL.md Step 9).

## Procedure

### Existing client
1. `search_files` with `parentId = '0AE7-flqLz3G9Uk9PVA' and title contains '[CODE]'` (or the client
   name) to find the client folder. Confirm the match with the operator.
2. `create_file` a folder (`mimeType: application/vnd.google-apps.folder`) named
   `[CODE]-[number] - [Project Name]` with `parentId` = the client folder id.
3. If the client folder has no `_Client Services` subfolder yet, create one (see above).

### New client
1. `create_file` a folder `[CODE] - [Client Name]` with `parentId = '0AE7-flqLz3G9Uk9PVA'`.
2. Inside it, `create_file` folders: `_Library`, `_admin`, `_Digital`, `_Client Services`,
   `[CODE]-[number] - [Project Name]`, `zz_Archive`.
3. If the team wants the `_Library` starter assets, copy them from the Template Master `_Library`
   (`1M-bHg4kI4xX9hwoQaDJQD_POv6FU-jz5`) as a follow-up (per-file copy; flag as a manual step).

### Brief
Create the pre-filled brief Google Doc inside the project subfolder (SKILL.md Step 8) and capture
its `viewUrl` to link in the Bonsai project Overview.
