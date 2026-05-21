# Founder Meeting Notes

Most people use Claude like a chatbot - ask something, get an answer, move on. That's fine for one-off stuff.

But you're talking to the same founders every two weeks. Stuff compounds. Context matters.

Before your 1st July call you should just know calls of 15th June, 1st June, 15th May, what happened, what's pending. Not because you dug through notes. Because it's just there.

That's what this does. Every meeting logged, last 3 always referenced. You walk in knowing. Every time.

In a VC context, this compounds fast. Across a portfolio of 20+ founders, the difference between showing up with context and showing up cold isn't just preparation — it's the quality of the relationship, the pattern recognition across companies, and ultimately the signal you catch early. This keeps all of that live without adding any workflow overhead.

---

## For teams

Anyone can drop notes into a founder's folder after a call, no format needed. `/update` finds everything, sorts by timeline, merges into one record.

In a firm where multiple partners touch the same portfolio company — one does the initial check-in, another sits on the board, a third catches a serendipitous coffee — the context stays shared. No one walks into a call blind because they weren't on the last one. The folder is the single source of truth for that relationship, and anyone on the team can pick it up mid-thread.

---

## How it works

Before your call with Vineet:
```
/prep vineet_khattar
```
Current status, last few conversations. Ready in seconds.

After the call, drop your notes in his folder and run:
```
/update vineet_khattar
```
Claude writes up a log entry, updates the summary, shows you both. You confirm, it saves.

---

## Setup

Clone this repo, open in Claude Code. Each founder gets a folder — that's where their history lives and where you drop notes after calls. Create one manually inside `contacts/` for each person you track, named `firstname_lastname`.

```
contacts/vineet_khattar/
```

---

## How it's built

Built as a Claude Code skill for VS Code — structured prompts that define
agent behavior for `/prep` and `/update`.

No external backend. State lives in a local file-based database: each founder
gets a folder with a running summary and dated log entries, all plain markdown.

Claude handles:
- **Context synthesis** — reads the summary + recent logs, surfaces what matters before a call
- **Note processing** — turns raw, unstructured call notes into structured log entries
- **Memory management** — rewrites the summary after each call so the last 3 meetings are always current
