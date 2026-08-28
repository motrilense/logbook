# index.md — login flake work package

<!--
WHAT THIS FILE IS
- Owner: the HUMAN. Written by the human, for the human.
- Purpose: the single entry point — annotated links to everything relevant.
- Rule:  never a bare URL. Every link = URL + one line of what/why.
-->

## Repositories
- [acme/webapp](https://github.com/acme/webapp) — the app that owns the flaky login
  test.

## Tickets & pull requests
- [FLAKE-88](https://acme.example.com/browse/FLAKE-88) — the ticket: login e2e flaky,
  blocks release tagging.
- [PR #212](https://github.com/acme/webapp/pull/212) — the fix: fixed sleep →
  explicit wait.
- [FLAKE-91](https://acme.example.com/browse/FLAKE-91) — follow-up: same sleep pattern
  in three other e2e tests.

## Documentation & references
- [Selenium — waits](https://www.selenium.dev/documentation/webdriver/waits/) — why
  explicit waits beat fixed sleeps.
