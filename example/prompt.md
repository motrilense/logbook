# prompt.md — login flake work package

<!--
WHAT THIS FILE IS
- Owner:  the AGENT. Written by the agent, for the agent: you write and curate
          this file; the human rarely touches it.
- Purpose: the "current picture" — durable, curated, and TASK-AGNOSTIC.
- Read:   FIRST, at the start of every session, to get back in sync with the
          work package before doing anything else.
- Filter: before adding anything ask "would this still be true and useful on an
          unrelated future task in this same work package?" If no, it is a diary
          entry, not a line here. Keep this file short — an overgrown prompt.md
          is worse than a short one.
- Name:   "prompt" is historical — this began as a reusable prompt for the agent.
          Treat it as the onboarding briefing, not an LLM instruction file.
-->

> Make the intermittently-failing login end-to-end test reliably green in CI.

## What this is
A small, bounded work package in the `webapp` repo. The login e2e test failed on
roughly 1 in 5 CI runs but always passed locally, and it was blocking the release
tag. The goal was reliable CI, not a redesign of login. **Closed** — fixed and
shipped; see `post-mortem.md`.

## Map — where things live
- `webapp/tests/e2e/test_login.py` — the test in question
  (`test_login_redirects_to_dashboard`, ~line 41).
- `webapp/app/auth/` — the login and session code the test exercises.
- `.github/workflows/e2e.yml` — runs the e2e suite on every PR.
- `resources/ci-failure-4213.log` — the captured failing run used to diagnose it.

## Conventions & commands
- Run just this test: `pytest webapp/tests/e2e/test_login.py -k redirects`.
- Reproduce a timing flake: run the test in a loop with the CPU busy — it passes in
  isolation, so load is what surfaces the race.

## Current focus
Nothing active — the package is closed. A follow-up (`FLAKE-91`) was opened for the
same fixed-`sleep` pattern found in three other e2e tests; that is a separate work
package, out of scope here.

## Gotchas
- E2e redirects are async: never assert on `current_url` right after a fixed
  `sleep`. Poll with `WebDriverWait(driver, 10).until(...)` for the expected URL. A
  fixed sleep passes on fast local machines and flakes under CI load. (This is the
  one durable lesson worth carrying to any future test work in this repo.)
