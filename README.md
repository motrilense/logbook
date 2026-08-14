# logbook

A plain-text framework for keeping a human and an AI agent in sync across the life of a
work package.

## What it's for

Some work is built from scratch; a lot of work is *jumping into something that already exists* and figuring it out as you go, often across several repositories, with blockers and decisions that only surface once you reach them.
The useful knowledge on that kind of work is discovered mid-task: logs, commands and their output, links, meetings, and the dead-ends you want to avoid repeating.
Most of it is noise once a thread closes; a small part is perdurable and worth keeping.

`logbook` is a lightweight convention for capturing that as plain text, so the same notes serve two purposes at once: they keep *you* oriented across sessions (including long gaps of inactivity), and they give an AI agent the context to work alongside you instead of starting cold every time.

It does not build things for you or automate a workflow. It is a note-taking structure that happens to be readable by both a human and an agent.

## Background

The approach was distilled from real work packages during which many notes were taking over their months of development.
Across all of them the same shape kept appearing on its own: an index that maps the work, heavily annotated links, a time-ordered execution log, and a handful of distilled notes holding the knowledge worth keeping. That recurring shape, informed by the Zettelkasten idea of promoting atomic notes out of raw material, is what logbook formalises.

## Pillars

1. **Work Package**: the unit everything hangs off. Bounded, and may span months with gaps.
2. **Index**: one note per work package acts as its map and entry point.
3. **Three content kinds, split by lifespan:**
   - *Current picture* - durable, curated, kept fresh; read this first.
   - *Trail* - the time-ordered execution log; raw and archivable.
   - *Distilled notes* - atomic, perdurable knowledge promoted out of the trail.
4. **Plain text, always annotated**: every link and artifact carries a one-line "what / why".
5. **Small status vocabulary**: `TODO` · `ONGOING` · `DONE`, and outcomes
   `OPEN` · `DONE` · `BLOCKED` · `WONTFIX`.
6. **Promotion & archival**: trail → distilled → current picture; spent noise is archived,
   never lost.

The pillars above define the conceptual baseline of this framework.
