# post-mortem.md — login flake work package
_Agent-written for the human · the close-out · read this to see how it went._

## Summary
The login e2e test was going red about 1 in 5 CI runs and blocking the release tag.
It turned out not to be a login bug at all — the test waited a fixed 1 second for the
redirect and then checked the URL, which isn't long enough when the CI box is under
load. We swapped the sleep for a proper wait that polls for the redirect. CI has been
green since (20/20 on the branch), the release is tagged, and we found the same smell
in three other tests that we punted to a follow-up.

## What this was
A quick, bounded cleanup in the `webapp` repo: stop `test_login_redirects_to_dashboard`
from failing intermittently in CI so the release could be tagged. Scope was reliability
only — no changes to how login actually works.

## Timeline
- **2026-08-14** — flagged the flake, handed it over, dropped the failing CI log in
  `resources/`.
- **2026-08-15** — diagnosed as a redirect timing race in the test; agreed to replace
  the sleep.
- **2026-08-15** — fix merged (PR #212), CI reran 20x green, release tagged, package
  closed.

## Outcome
- **DONE** — flake fixed and shipped in PR #212; the release is tagged.
- **WONTDO (here)** — the same fixed-sleep pattern in three other e2e tests was left
  alone on purpose and moved to `FLAKE-91`, so this package stayed small.

## What went well
- The captured CI log made the diagnosis fast — the URL was still `/login?next=…`, so
  it was obviously the test asserting too early, not the app breaking.
- Reproducing under CPU load turned "it's flaky" into a confirmed timing race, so the
  fix wasn't a guess.

## What was hard
- It never reproduced locally, which is exactly why it sat around annoying us — the
  bug only shows up when the machine is slow.

## Lessons & what to do differently
- Don't trust `current_url` right after a fixed `sleep` in an e2e test. Wait for the
  redirect explicitly (`WebDriverWait(...).until(...)`). Fixed sleeps look fine locally
  and rot in CI.
- We keep hitting this pattern (payments suite too, per the notes) — worth a broader
  sweep than one test at a time.

## Open threads & follow-ups
- `FLAKE-91` — the same sleep-then-assert pattern in three other e2e tests. Not
  started; tracked in `index.md`.

## References
- `resources/ci-failure-4213.log` — the failing run that pinned the cause.
- PR #212 and tickets FLAKE-88 / FLAKE-91 — see `index.md` for links.
