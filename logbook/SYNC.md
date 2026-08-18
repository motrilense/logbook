# logbook — how to use the template

`logbook` is a plain-text template framework for a human and an AI agent to keep a
shared, session-crossing record of a piece of work, so the agent can be handed the
work and pick it up cold.

This guide explains where each type of note goes and its function in the framework.
If you have never seen logbook before, you should be able to copy the `logbook/`
directory, define your work package, prepare your workspace, and place your notes
correctly after reading this once.

## Ownership and Purpose

A work package is one bounded piece of work (it may run for months, with
gaps). Inside it, notes split two ways at once:

- **by owner** — some files are the agent's, some are the human's;
- **by kind** — a durable *current picture*, a time-ordered *trail*, and a shared
  pool of *resources*.

Cross those two axes and you get the full set of files:

| File / dir       | Owner | Kind            | Written           |
|------------------|-------|-----------------|-------------------|
| `prompt.md`      | Agent | current picture | curated, kept short |
| `diary.md`       | Agent | trail           | newest on top     |
| `development.md` | Human | trail           | top-down, as you go |
| `index.md`       | Human | links (the map) | annotated links   |
| `resources/`     | Human | ground truth    | dropped in as needed |

The two trails (`diary.md` and `development.md`) describe similar events from
two points of view: the agent narrates for a future agent; the human jots for
themselves. They are correlated but need not match in tone or level of detail.

## Layout

```
<work-package>/
├── prompt.md          # AGENT: the current picture — read first, every session
├── diary.md           # AGENT: the trail — what happened, newest on top
├── development.md     # HUMAN: the trail — your running execution notes
├── index.md           # HUMAN: annotated links to everything relevant
└── resources/         # SHARED: specs, tickets, logs, artifacts (human-owned)
    └── ABOUT.md       # placeholder
```

Copy the `logbook/` directory and start filling it in.

## Where and What to Write

Ask two questions: **who owns it** and **how long is it true**.

- A durable fact the agent needs to start cold (architecture, where things live,
  a command that works, a recurring gotcha) → **`prompt.md`**, written by the agent.
- Something that just happened (a step taken, a result, a dead-end) that is worth
  keeping track of → **`diary.md`** for the agent, **`development.md`** for you.
- A link to a repo, ticket, PR, or doc — always with one line of what/why, never a
  bare URL → **`index.md`**, maintained by you.
- A spec, ticket text, diagram, log, or artifact → **`resources/`**, provided by you
  as the person driving the work package forward.

Rule of thumb: if a note only makes sense inside one investigation (timestamps,
"then I tried X"), it belongs in a trail. If it stays true after the task closes,
it belongs in `prompt.md` (agent) or `resources/` (shared material).

## How to Start

First, define your work package and gather the resources needed to start the first
task.

These can be a ticket, a specification, or notes distilled from one or more meetings;
place them in the work package's `resources/` directory. Be specific about what you
want to achieve, and provide both what you already have and what you still need to
complete the task. Remember that code is a means to fulfil a project, not the product
itself — it only captures one point of view of the reality. It is up to the person
driving the work package to build that context from their interactions with the
stakeholders.

Once that is in place, let the AI agent sync with the workspace using this same
`GUIDE.md` — it is written for human and agent alike. Then point the agent at the
work package's resources. With those, it should have enough to start filling in its
own files (`prompt.md`, `diary.md`) from its understanding.

## Vocabulary

`logbook` uses a small, fixed set of status words. There are **two axes** — do not
mix them.

**Item state** — the progress of a to-do or checklist item:

| Word      | Meaning                          |
|-----------|----------------------------------|
| `TODO`    | not started                      |
| `ONGOING` | in progress                      |
| `DONE`    | the item is finished             |

**Outcome** — how an investigation or trail entry ended:

| Word      | Meaning                                        |
|-----------|------------------------------------------------|
| `OPEN`    | still in progress / unresolved                 |
| `DONE`    | resolved, with something that verified it      |
| `BLOCKED` | cannot proceed; waiting on an external thing   |
| `WONTDO`  | deliberately not doing it (record why)         |

> `DONE` appears in both axes — that is intentional but easy to confuse. Read it
> by context: on a checklist item it means *the item is finished*; on a diary or
> development entry it means *this line of work reached a verified result*. When
> in doubt, prefer the entry-level `Status:` line to state the outcome.
