# index.md — login flake work package
_Human-owned · the map · annotated links only, never a bare URL._

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
