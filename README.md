# Founder-Context-Engine

Every month, I managed a portfolio of 10+ founders. Calls and notes across Excel, CRM.

CRMs and Excel store information. They don't synthesize it.

Before a call, I still have to check — what did we discuss last time? What's unresolved across the last 3 calls?

So I built this.

- Structured log updated after each meeting
- Rolling memory across every founder
- Pre-call briefing in one command (30s)

**Before my 1st July call, I should know: 15th June, 1st June, 15th May. What happened, what's pending.**

**For my 15th July call, I should know: 1st July, 15th June, 1st June. Same thing, rolled forward.**

That's what this does. Every meeting logged. Last 3 summaries always on top, when needed.

---

![Update notes skill](docs/images/1.%20update-notes%20Skill.png)
![Update notes skill 2](docs/images/2.%20update-notes%20Skill%20%20.png)
![Prep before meeting](docs/images/3.prep-before-meeting%20skill.png)
![Prep before meeting 2](docs/images/4.%20prep-before-meeting%20skill.png)

---

## How it works

```
Raw notes
   ↓
update-notes-founder
   ↓
summary.md + log.md
   ↓
prep-for-founder
   ↓
Pre-call briefing
```

Before your call with Vineet:
```
/prep-for-founder vineet_khattar
```
Pulls up where things stand: what was discussed, what's open. Takes a second.

After the call, drop your notes in his folder and run:
```
/update-notes-founder vineet_khattar
```
Claude turns them into a proper log entry, rewrites the summary, shows you both before saving anything. You confirm, it's done.

---

## Example prep output

```
CURRENT STATUS — Apr 2025

Vineet has strong PMF signal on 3PL routing.
Two term sheets in hand.
Co-founder cap table issue unresolved.

Key context before next call:
- waiting on lawyer discussion
- customer discovery validated coordination-layer hypothesis
- founder disagreement on hiring pace
```

---

## For teams

After a call, whoever was on it just drops their notes in the founder's folder. No format, no template. Run `/update-notes-founder` and it pulls everything together. Sorted, merged, done.

---

## Setup

Clone this repo, open in Claude Code. Make a folder inside `contacts/` for each person — that's where everything lives.

```
contacts/vineet_khattar/
    vineet_khattar_summary.md
    vineet_khattar_log.md
    newnotes-vineet.md
```

After your first `/update-notes-founder`, Claude creates a summary file and a running log. Drop new notes in the same folder each time.

---

## Architecture

```
.claude/skills/
├── prep-for-founder/         # Read-only pre-call briefing
│   └── SKILL.md              # Reads summary file, outputs as-is — no writes
└── update-notes-founder/     # Post-call note processing
    └── SKILL.md              # 8-step pipeline: find drafts → generate → confirm → write

contacts/[firstname_lastname]/
├── [name]_summary.md         # Always current: status + last 3 meetings
├── [name]_log.md             # Append only: full dated meeting history
└── [draft files]             # Any other file = raw notes, consumed on /update
```

---

## Limitations

- No cross-founder linking
- Summary quality depends on note quality
- No calendar or email integration
