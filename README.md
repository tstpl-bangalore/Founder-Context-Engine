# Portfolio Memory 

In a VC context across a portfolio of 20+ founders, 
the difference between showing up with context and showing up cold isn't just preparation defines the quality of the relationship.

**Before your 1st July call you should just know: 15th June, 1st June, 15th May — what happened, what's pending.**

**Similarly, Next meeting 15th July — get a summary of 1st July, 15th June, 1st June.**

That's what this Claude Code skill does.
Every meeting logged, last 3 meeting summaries always on top — when needed.
You walk in knowing. Every time.

---

![Update notes skill](docs/images/1.%20update-notes%20Skill.png)
![Update notes skill 2](docs/images/2.%20update-notes%20Skill%20%20.png)
![Prep before meeting](docs/images/3.prep-before-meeting%20skill.png)
![Prep before meeting 2](docs/images/4.%20prep-before-meeting%20skill.png)

---

## For teams

After a call, whoever was on it just drops their notes in the founder's folder. No format, no template. Run `/update` and it pulls everything together — sorted, merged, done.


---

## How it works

Before your call with Vineet:
```
/prep vineet_khattar
```
Pulls up where things stand. What was discussed, what's open. Takes a second.

After the call, drop your notes in his folder and run:
```
/update vineet_khattar
```
Claude turns them into a proper log entry, rewrites the summary, shows you both before saving anything. You confirm, it's done.

---

## Setup

Clone this repo, open in Claude Code. Make a folder inside `contacts/` for each person — that's where everything lives.

```
contacts/vineet_khattar/
```

Drop notes there after calls. That's the whole setup.

---

## How it's built

Built as a Claude Code skill for VS Code — structured prompts that define agent behavior for `/prep` and `/update`.

No external backend. State lives in a local file-based database: each founder gets a folder with a running summary and dated log entries, all plain markdown.

Claude handles:
- **Context synthesis** — reads the summary + recent logs, surfaces what matters before a call
- **Note processing** — turns raw, unstructured call notes into structured log entries
- **Memory management** — rewrites the summary after each call so the last 3 meetings are always current
