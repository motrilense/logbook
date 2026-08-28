# diary.md — login flake work package

<!--
WHAT THIS FILE IS
- Owner:     the AGENT. Written by the agent, for the agent.
- Purpose:   the "trail" — a time-ordered log of what actually happened.
- Direction: NEWEST ENTRY ON TOP (prepend, never append).
- Partner:   development.md is the human's trail of the same work. Think of two
             people on the same project: each keeps their own account of the same
             events, from a different point of view, out of a different experience,
             in their own writing style. The two are companions, not copies.
- Promotion: when an entry hardens into a durable, task-agnostic fact, distil a
             one-line summary up into prompt.md and leave the full trail here.
- Record progress AND dead-ends — a closed-off path is progress worth logging.
- Status values: see the "Vocabulary" section of the logbook SYNC guide (`SYNC.md`).
-->

<!-- Newest entry goes directly below this line. -->

## 2026-08-15 — Close-out: pruned my files, wrote the post-mortem
Status: DONE
Context: Human called the package done after the release tag and asked me to close it
out.
Findings:
- Trimmed this diary: folded a couple of noisy step-by-step lines from the 08-14
  entry into the summary below, keeping the trail intact.
- Reduced `prompt.md` to the durable picture — the async-redirect gotcha is the only
  thing a future task here really needs.
Outcome / next: wrote `post-mortem.md` from both trails + `index.md` for the human.
Package closed. Follow-up lives in `FLAKE-91`.

## 2026-08-15 — Replaced the fixed sleep with an explicit wait; CI green
Status: DONE
Context: The human wanted the login e2e test to stop failing ~1 in 5 CI runs —
reliable CI, not elegance.
Findings:
- The test does `time.sleep(1)` and then asserts `current_url` ends with
  `/dashboard`. The redirect is driven by an async session write; under CI load it
  lands *after* the 1s, so the assertion fires too early. Fast local machines hide
  it. It is a timing race in the test, not a login bug.
- Confirmed it is purely timing: looping the test with the CPU pinned failed 4/20;
  bumping the sleep to 3s made it pass 20/20.
- Fix: swapped the sleep for
  `WebDriverWait(driver, 10).until(lambda d: d.current_url.endswith("/dashboard"))`,
  which polls for the redirect instead of guessing a duration.
Outcome / next: PR #212 merged; branch CI 20/20 green. → promoted the durable gotcha
to `prompt.md`. Grep turned up the same sleep-then-assert shape in three other e2e
tests; flagged to the human as a follow-up, out of scope here.

## 2026-08-14 — Synced and read the failing test
Status: DONE
Context: Picked up the work package; goal is to stop the flaky login test.
Findings:
- Read `development.md` and `resources/ci-failure-4213.log`: the failure is an
  `AssertionError` on `current_url` (still `/login?next=/dashboard`), not an app
  exception — that points at the test, not the login code.
- Located the test at `webapp/tests/e2e/test_login.py:41`.
Outcome / next: working hypothesis is a redirect timing race; next step is to
reproduce it under load.
