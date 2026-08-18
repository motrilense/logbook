# logbook — sync guide

> **Framework version:** `a1.0.0` (alpha)

`logbook` is a plain-text template framework for a human and an AI agent to keep a
shared, session-crossing record of a piece of work, so the agent can be handed the
work and pick it up cold.

This is the file both parties read to **get in sync**: the human reads it to set up
the work package, then points the agent at it so the agent learns the same layout.
It explains where each type of note goes and its function in the framework. If you
have never seen logbook before, you should be able to copy the `logbook/` directory,
define your work package, prepare your workspace, and place your notes correctly
after reading this once.

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
├── SYNC.md            # HUMAN/AGENT: the sync guide — read this to get in sync
├── development.md     # HUMAN: the trail — your running execution notes
├── index.md           # HUMAN: annotated links to everything relevant
├── prompt.md          # AGENT: the current picture — read first, every session
├── diary.md           # AGENT: the trail — what happened, newest on top
├── post-mortem.md     # AGENT→HUMAN: the close-out summary, written when work ends
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

This is the sync method — the ritual that hands a work package from human to agent.
Do the steps in order.

**Human — set up the work package:**

1. **Read this guide.** It is written for human and agent alike.
2. **Define the work package and gather resources.** These can be a ticket, a
   specification, or notes distilled from one or more meetings; place them in the
   work package's `resources/` directory. Be specific about what you want to
   achieve, and provide both what you already have and what you still need.
   Remember that code is a means to fulfil a project, not the product itself — it
   only captures one point of view of reality. It is up to the person driving the
   work package to build that context from their interactions with stakeholders.
3. **Prime the map and the trail.** Add annotated links to `index.md`, and write
   the first lines of `development.md` so there is a starting point to build on.

**Handoff:**

4. **Point the agent at this file (`SYNC.md`) to sync**, then at the work package's
   `resources/`.

**Agent — pick up the work:**

5. **Read the guide and resources, learn the layout, and start working.** With that
   context the agent has enough to begin filling in its own files (`prompt.md`,
   `diary.md`). Each file's header block explains how that file is used, so the
   agent can move around the `logbook/` directory without further instruction.

## The Working Loop

The first sync happens once. After it, the work package runs as a loop of short
human–agent handovers, each side keeping its own trail and passing the baton across
`development.md`.

**Human — keep the trail moving:**

- Record actions and status in `development.md` as you go — decisions taken in
  meetings, blockers hit, scope changes.
- Drop new material into `resources/` — machine logs to investigate, design drafts,
  trade-off notes.
- Add any new relevant links to `index.md`.
- Treat every document on a need-to-know basis for the agent, and redact confidential
  information before it lands. Keeping the trail takes discipline, but the information
  passes through you either way — the work cannot progress without it.

**Handoff — request the agent:**

- Write the request at the bottom of `development.md` and point the agent at that last
  section.

**Agent — work the request:**

- Read that last section of `development.md`, then explore the `logbook/` to build the
  picture: context knowledge plus task definition.
- Execute the task, analyse the outcome, and update `diary.md` and/or `prompt.md`
  accordingly.

Then control returns to the human, and the loop repeats.

## Closing a Work Package

When the work is finished — or parked for good — the human triggers a **close-out**.
The point is to leave the package clean and to hand the human a readable retrospective.
The human asks the agent to close it out; the agent then does two things, in order.

**1. Prune the agent's own files.** With everything learned across the work package,
the agent revisits its notes for a future reader that is *itself*:

- `prompt.md` — leave only perdurable, task-agnostic facts about this workspace;
  strip out noise and detail specific to individual tasks.
- `diary.md` — compact or simplify entries that ballooned into small steps, while
  keeping the trail complete. Nothing worth revisiting is lost, just tightened.

**2. Write the close-out summary.** The agent then fills in `post-mortem.md` — the one
file it writes *for the human*. It reads the human-owned notes (`index.md`,
`development.md`) and, where needed, `resources/`, to reconstruct how the work went;
it does **not** modify those human files. The agent supplies the pattern-finding and
abstraction, but writes in the human's tone (mirror `development.md`) so the human can
skim it with a low entry barrier. `post-mortem.md`'s own header explains what each
section should hold.

The result is a helicopter view the human can read end to end to see what happened and
why — a standard software post-mortem, kept useful rather than ceremonial.

## Vocabulary

`logbook` uses one small set of status words — a single lifecycle you can put on
anything you track: a checklist item, a sub-task, or a diary / `development.md` entry.

| Word      | Meaning                                                  |
|-----------|----------------------------------------------------------|
| `TODO`    | identified, not started                                  |
| `ONGOING` | in progress / still open                                 |
| `BLOCKED` | cannot proceed; waiting on something external (say what) |
| `DONE`    | finished, with something that verified it                |
| `WONTDO`  | deliberately dropped (record why)                        |

The happy path is `TODO → ONGOING → DONE`; `BLOCKED` and `WONTDO` are the two ways
out. Where it shows up:

- On a **checklist or plan**, tag each item with its state.
- On a **diary or `development.md` entry**, the `Status:` line records where that
  thread stands right now — usually `DONE` (you finished and something verified it)
  or `ONGOING` (still open), sometimes `BLOCKED` or `WONTDO`. You rarely file an
  entry for a `TODO`, because writing one means you have already started.

`DONE` means exactly one thing everywhere — *finished and verified* — so there is no
second axis to keep straight.

## Worked Example

See [`example/`](../example) for a small, realistic work package
(`20260814_fix-flaky-login-test`): a flaky login e2e test, taken from open to closed.
The human seeds `development.md` and `index.md`, the agent syncs and drops a failing
CI log into `resources/`, reproduces and fixes the flake, logs both trails, promotes
the durable gotcha up into `prompt.md`, and finally closes the package out in
`post-mortem.md`. Notice the deliberate contrast in voice: the human's notes are terse
scratch, the agent's entries are structured, and the post-mortem is the agent writing
back in the human's tone.
