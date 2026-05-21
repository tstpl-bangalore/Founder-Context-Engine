Use case document
---
# Entrepreneur Meeting Notes — Claude Code Skill
## Complete Use Case Document
---
### The Business Context
A business coach or investor relations professional manages ongoing relationships with 10–15 entrepreneurs per month. Each entrepreneur has regular touchpoints — calls every 2 weeks or monthly. The professional needs to:

- Capture raw notes during or after each call
- Maintain a clean, growing history per entrepreneur
- Walk into every call with instant context — what happened in the last 3 meetings

Today this is done manually. Files are scattered, naming is inconsistent across team members, and reviewing history before a call means scrolling through months of notes. The goal is to automate the consolidation and review using Claude Code skills.

---
### The People Involved

**The professional (you)** — reviews summaries before calls, runs commands, approves what gets written permanently.

**Team members** — may take rough notes on your behalf during a call and drop the file into the right folder. They don't need to know Claude Code. They just drop a file.

---

### The Folder and File Structure

Two top-level folders live at the workspace root:

```
docs/
    BRD-Context.md

.claude/
    CLAUDE.md
    skills/
        prep/
            SKILL.md
        update/
            SKILL.md

contacts/
    vineet_khattar/
        vineet_khattar_log.md
        vineet_khattar_summary.md
        [drafts dropped here by anyone]
    priya_patel/
        priya_patel_log.md
        priya_patel_summary.md
```

**Why two separate folders** — `.claude/skills/` holds instructions for Claude. `contacts/` holds live data. Keeping them separate means the skill definition never gets mixed with growing contact history.

**Naming convention** — all lowercase, underscores, no spaces. Folder name matches the person's name. The two permanent files always follow the pattern `[foldername]_log.md` and `[foldername]_summary.md`. This is how Claude knows which files are protected.

**`vineet_khattar_log.md`** — The permanent, append-only history. Every meeting ever. Never edited, only added to. This is the source of truth.

**`vineet_khattar_summary.md`** — The living summary. Always contains last 3 meetings condensed + a one-line current status at the top. Gets completely rewritten after every update. This is what you open before a call.

When a meeting happens, one or more draft files appear in the folder. Team members name them however they like:

```
rajesh_15-may-25.md
rajesh_15-05.md
rajesh_notes_may.md
meeting_rajesh.md
```

**The skill identifies drafts by elimination** — it knows the 2 permanent files by their exact names (`[foldername]_log.md` and `[foldername]_summary.md`). Anything else in the folder = a draft to be processed.

**Multiple drafts** — if more than one draft exists (e.g. two team members both took notes on the same day), Claude reads them all. Each draft contains a timestamp line at the top (e.g. `15 June, 12pm`). Claude reads them in chronological order by that timestamp and treats them as a single merged meeting record before generating the log entry and summary.

---

### The Two Moments That Matter

**Moment 1 — Before the call.** You want to walk in prepared. You need last 3 meetings in 60 seconds. Zero friction.

**Moment 2 — After the call.** You or a team member has rough notes. Could be messy bullets, could be a pasted transcript, could be half-sentences written during the call. These need to become a clean permanent record.

---

### The Two Commands

---

#### Command 1: `/prep vineet_khattar`

**When:** 10 minutes before a call with Vineet.

**What you type:**
```
/prep vineet_khattar
```

**What Claude does:**
- Navigates to `/contacts/vineet_khattar/`
- Reads `vineet_khattar_summary.md`
- Displays it cleanly in the terminal

**What you see:**
A focused pre-call brief. Current status line at top. Last 3 meetings below. Nothing else.

**Design principle:** This command does nothing permanent. Read only. No files are changed. Pure retrieval.

---

#### Command 2: `/update vineet_khattar`

**When:** Within 1–2 days after a call, once draft notes are in the folder.

**What you type:**
```
/update vineet_khattar
```

**What Claude does — step by step:**

**Step 1 — Find the draft**
Scans `/contacts/vineet_khattar/`. Identifies the two permanent files by name. Any other file present = the draft. Picks it up regardless of filename or naming convention.

**Step 2 — Read everything**
Reads the draft file in full. Reads `vineet_khattar_log.md` for historical context. Understands what has been discussed before, what was pending, what the relationship looks like.

**Step 3 — Generate proposed log entry (X)**
Produces a clean, structured meeting entry from the messy draft. Includes date, key topics discussed, decisions made, action items, anything notable. Compressed but complete. This is what will be appended to `vineet_khattar_log.md`.

**Step 4 — Generate proposed summary rewrite (Y)**
Rewrites `vineet_khattar_summary.md` entirely. Top line: current status of Vineet updated to reflect this meeting. Below: last 3 meetings condensed, most recent first. Previous summary is used as reference but fully replaced.

**Step 5 — Confirm before writing**
Claude shows you both and asks:

```
Here is the log entry I propose to append (X):

[proposed log entry]

---

Here is the updated summary I propose (Y):

[proposed summary]

---

Does X look good to append to the log?
Does Y look good to replace the summary?

Type YES to confirm both, or tell me what to change.
```

**Step 6 — You review and respond**
You read both. If something is wrong — Claude missed a key point, misread a name, got the action items wrong — you tell it. Claude revises. This loop continues until you're satisfied.

**Step 7 — Write and clean up**
Only after your confirmation, in this exact order:
1. X gets appended to `vineet_khattar_log.md` — then verified non-empty
2. Y replaces `vineet_khattar_summary.md` entirely — then verified non-empty
3. Draft file(s) are deleted — only after both verifications pass

**Design principle:** Nothing permanent happens without human confirmation. The draft is never deleted until you say yes, and never deleted before both files are written. If something fails mid-write, the drafts survive so nothing is lost.

---

### What the Summary File Looks Like

```
CURRENT STATUS — May 2025
Vineet is closing his seed round. Target close date end of June.
Investor intro requested — needs warm intro to Sequoia.

---

LAST 3 MEETINGS

15 May 2025
Discussed cap table concerns. Co-founder dilution issue surfaced.
Vineet to consult his lawyer before next call.
Action: Share cap table template by 20 May.

1 May 2025
First call after product launch. Early traction looks promising.
Revenue not yet meaningful but user growth strong.
Agreed to review metrics together next call.

15 April 2025
Introductory deep dive. Background, vision, founding story.
Identified key challenge: no CFO, handling finance himself.
Suggested connecting with a fractional CFO in our network.
```

This file gets completely replaced every time `/update` runs. It always reflects the current moment.

---

### What the Log File Looks Like

```
---
MEETING — 15 May 2025

Topics: Cap table structure, co-founder dilution, fundraising timeline
Key discussion: Co-founder concerned about dilution in upcoming round.
Vineet unsure how to handle. Advised consulting a startup lawyer.
Fundraising target confirmed at $1.2M seed.
Action items: Share cap table template (us by 20 May). Vineet to get legal clarity before next call.
Next meeting: 1 June 2025

---
MEETING — 1 May 2025

Topics: Post-launch review, early metrics, team morale
Key discussion: Product launched 3 weeks ago. 400 signups, 12 paying customers.
Revenue ~$600/month, not meaningful yet but directionally good.
Team morale high. Vineet energized.
Action items: Vineet to prepare metrics dashboard before next call.
Next meeting: 15 May 2025

---
MEETING — 15 April 2025
...
```

This file only grows. Nothing is ever edited or removed.

---

### The Team Scenario

Your colleague Priya takes notes during a call with Vineet on 15 June at 12pm. She names her file `vineet_june15_priya.md` and drops it in `contacts/vineet_khattar/`. You also took brief notes during the same call and dropped `vineet_june15_ashesh.md` in the same folder.

You come back to your desk. You run `/update vineet_khattar`. Claude finds the folder, sees 4 files, identifies the 2 permanent ones by name, picks up both draft files automatically. It reads the timestamp inside each (`15 June, 12pm` and `15 June, 12pm`) and merges them into one meeting record. You never had to coordinate file names with Priya. The folder structure did the work.

---

### What Makes This Interesting for a Claude Code Class

**1. File system as logic.** The skill uses the presence or absence of a third file as its input signal. No database, no config, no metadata. The folder itself encodes state.

**2. Named file immunity.** Two files are protected by name. Everything else is fair game. Simple rule, powerful behavior.

**3. Confirmation loop.** The skill doesn't just execute — it proposes and waits. Students learn that good skills keep humans in the loop for irreversible actions.

**4. Two intelligence tasks in one command.** Compression (log entry) and synthesis (summary rewrite) are different cognitive tasks. Claude has to do both from the same raw input.

**5. Real business workflow.** Not a toy example. Students can immediately see how this maps to their own work — sales calls, investor updates, client relationships, mentorship.

**6. Team-aware design.** The skill works whether you took the notes or someone else did. Robust to human variation.

---

### What Students Will Build

1. The folder and file structure
2. `CLAUDE.md` — global workspace rules only (e.g. where contacts live)
3. `prep/SKILL.md` — the `/prep` command definition
4. `update/SKILL.md` — the `/update` command definition including the confirmation loop
5. Test it with a real or simulated entrepreneur folder and rough notes

---

That's the complete use case. Ready to hand to a class as-is.