---
name: update-notes-founder
description: Process draft meeting notes for a contact into a permanent record. Finds draft files in the contact folder, generates a log entry and summary rewrite, confirms with the user before writing anything. Use when the user runs /update [contact_folder_name] after a meeting. Nothing is written or deleted until the user confirms.
---

## Usage
```
/update vineet_khattar
```

The argument must exactly match the contact's folder name inside `contacts/`.

## Steps

### Step 1 — Resolve the folder
Check that `contacts/[argument]/` exists. If not, stop: "No contact folder found for '[argument]'. Folder name must match exactly."

### Step 2 — Identify files
Inside the folder, the two permanent files are:
- `[argument]_log.md`
- `[argument]_summary.md`

Every other file in the folder is a draft. Collect all drafts regardless of their filename.

If no drafts are found, stop: "No draft files found in contacts/[argument]/. Drop your notes file there and run /update again."

### Step 3 — Read the drafts
Read all draft files. Each draft has a timestamp line at the top (e.g. `15 June, 12pm`). If there are multiple drafts, sort them by that timestamp in chronological order and treat them as a single merged meeting record.

### Step 4 — Read the log
Read `[argument]_log.md` in full. Use it to understand the history: what has been discussed, what was pending, how the relationship has evolved.

If the log file is empty (this is the first-ever meeting), note that there is no prior history. For the summary Y, show only this meeting under LAST 3 MEETINGS and leave the earlier slots out entirely — do not write placeholder text.

### Step 5 — Generate proposals
Produce two outputs:

**X — Log entry** to be appended to `[argument]_log.md`:
```
---
MEETING — [date: use DD Mon YYYY if day is known, Mon YYYY only if day is not in the draft]

Topics: [comma-separated topic list]
Key discussion: [2–4 sentences, compressed but complete]
Action items: [who does what by when; omit this line if none]
Next meeting: [date if mentioned; omit this line if not mentioned]
```

If the draft does not contain a specific day, note the imprecision in the date field (e.g. `Aug 2025 — exact date not in notes`) rather than silently dropping it.

**Y — Summary rewrite** to fully replace `[argument]_summary.md`:
```
CURRENT STATUS — [month year]
[1–2 sentences on where this person stands right now]
[One line for the most important open ask or pending action, if any — e.g. "Intro requested: warm intro to Sequoia needed."]

---

LAST 3 MEETINGS

[Most recent date]
[3–4 lines: key topics, decisions, actions]

[Second most recent date]
[3–4 lines]

[Third most recent date]
[3–4 lines]
```
The summary always shows the 3 most recent meetings including this new one. Pull earlier meetings from the log if needed. If fewer than 3 meetings exist, show only those that exist — do not add placeholder entries.

### Step 6 — Confirm before writing
Show the user both proposals exactly like this:

```
Here is the log entry I propose to append (X):

[X]

---

Here is the updated summary I propose (Y):

[Y]

---

Does X look good to append to the log?
Does Y look good to replace the summary?

Type YES to confirm both, or tell me what to change.
```

### Step 7 — Revision loop
If the user requests changes, apply them and show the updated proposals again. Repeat until the user confirms.

Handle partial approval explicitly:
- If the user approves only X ("X is fine, fix Y") — treat X as locked, revise only Y, show both again.
- If the user approves only Y ("Y is fine, fix X") — treat Y as locked, revise only X, show both again.
- Write nothing until the user has confirmed both X and Y, whether in one YES or through partial approvals.

Repeat until both are confirmed.

### Step 8 — Write and clean up
Only after both X and Y are confirmed, in this exact order:
1. Append X to `[argument]_log.md`
2. Re-read `[argument]_log.md` and verify it is non-empty and ends with the new entry. If verification fails, stop immediately and do not proceed.
3. Fully replace `[argument]_summary.md` with Y
4. Re-read `[argument]_summary.md` and verify it is non-empty and contains the new CURRENT STATUS line. If verification fails, stop immediately and do not proceed.
5. Delete all draft files — only after both verifications pass.

If any step fails, stop immediately. Do not delete any drafts. Tell the user which step failed.
