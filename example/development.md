# development.md — login flake work package
_Human-owned · a running history, not a diary. `---` splits each hand-off to the
agent; dates only call out events worth remembering._

login e2e test keeps going red in CI, ~1 in 5 runs. passes fine locally so I can't
even see it, which is half the problem. blocking the release tag, really annoying.
grabbed the failing CI log off the runner and dropped it in resources/. handing it to
the agent — told it I don't care about elegance, I just want CI reliably green.

---

agent reckons it's a timing race in the *test*, not login: a hard-coded 1s wait for
the redirect that's too short when the CI box is under load. makes sense, the runners
are slow and it never shows up on my machine.

> **2026-08-15 — release cut-off is Friday**, so this needs to land today.

told it to go ahead and swap the sleep for a proper wait. (side note to self: the
payments suite smells like it has the same fixed-sleep problem — sweep it some other
time.)

---

fix merged in PR #212, reran the branch CI 20x and it's all green.

> **2026-08-15 — release v1.4 tagged.** flake gone, we're unblocked.

agent flagged 3 more e2e tests with the same sleep pattern — not touching them now,
opened FLAKE-91 to track separately. asked it to close the package out: tidy its own
notes and write the post-mortem.
