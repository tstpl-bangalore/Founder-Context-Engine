---
name: prep-for-founder
description: Pre-call briefing for a contact. Reads and displays the contact's summary file. Use when the user runs /prep [contact_folder_name] before a meeting. Read-only — no files are written or deleted.
---

## Usage
```
/prep vineet_khattar
```

The argument must exactly match the contact's folder name inside `contacts/`.

## Steps

1. Take the argument as-is (e.g. `vineet_khattar`).
2. Check that `contacts/[argument]/` exists. If not, stop and tell the user: "No contact folder found for '[argument]'. Folder name must match exactly."
3. Read `contacts/[argument]/[argument]_summary.md`.
4. Display the full contents cleanly. Do not add commentary, headers, or any extra text — just the summary as written.

## What not to do
- Do not read the log file.
- Do not scan for drafts.
- Do not write or delete any file.
