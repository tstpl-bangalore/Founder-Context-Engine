# Portfolio Memory 

I managed a portfolio of 10+ founders, every month: calls and notes across Excel, CRM.

Now, CRMs and Excel store information. They don't synthesize it. 
Before a call I still have to check — what did we discuss last time? What's unresolved?

So I built this.

**Before my 1st July call, I should just know: 15th June, 1st June, 15th May — what happened, what's pending.**

**Similarly, for next meeting 15th July — get a summary of meetings: 1st July, 15th June, 1st June.**

That's what this Claude Code skill does.
Every meeting logged, last 3 meeting summaries come always on top — when needed.


---

![Update notes skill](docs/images/1.%20update-notes%20Skill.png)
![Update notes skill 2](docs/images/2.%20update-notes%20Skill%20%20.png)
![Prep before meeting](docs/images/3.prep-before-meeting%20skill.png)
![Prep before meeting 2](docs/images/4.%20prep-before-meeting%20skill.png)

---

## For teams

After a call, whoever was on it just drops their notes in the founder's folder. No format, no template. Run `/update-notes-founder` and it pulls everything together — sorted, merged, done.


---

## How it works

Before your call with Vineet:
```
/prep-for-founder vineet_khattar
```
Pulls up where things stand. What was discussed, what's open. Takes a second.

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

## How it's built

Built as a Claude Code skill for VS Code — structured prompts that define agent behavior for `/prep-for-founder` and `/update-notes-founder`.

No external backend. State lives in a local file-based database: each founder gets a folder with a running summary and dated log entries, all plain markdown.

Local markdown means portable, inspectable, version controllable, privacy-friendly, and zero backend maintenance.

Claude handles:
- **Context synthesis** — reads the summary + recent logs, surfaces what matters before a call
- **Note processing** — turns raw, unstructured call notes into structured log entries
- **Memory management** — rewrites the summary after each call so the last 3 meetings are always current

---

## Limitations

- No cross-founder linking
- Summary quality depends on note quality
- No calendar or email integration
