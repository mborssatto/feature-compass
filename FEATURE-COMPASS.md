# Feature Compass — shipped registry

> Append-only ledger of shipped reality. Each row represents a commitment to
> users. To move an entry here, ship it (use the Communicate mode of the
> `feature-compass` skill).
>
> In-flight work (`clarifying`, `building`) lives in `FEATURE-COMPASS.draft.md`.
>
> Built by [Mariana Borssatto](https://github.com/mborssatto) · [Contribute](https://github.com/mborssatto/feature-compass)

## Summary

| # | Theme | Feature | One-line description | Shipped |
|---|---|---|---|---|

<!-- Shipped features go below, in the same order as the table above. -->
<!-- One section per feature: name, one-liner, criteria table, out-of-scope. -->

---

<!-- DELETE this example section once you have real shipped features. -->

## 1. Never Blank Screens

**Theme:** Reliability
**One-liner:** When an API call fails transiently, the user sees a retry state instead of an empty page.
**Shipped:** 2026-04-01

### Acceptance criteria

| #   | Criterion |
|-----|-----------|
| 1.1 | Failed API calls retry up to 3 times on 5xx and timeout errors |
| 1.2 | No retry on 4xx (permanent) errors |
| 1.3 | User sees a "retrying" state, not a blank screen |
| 1.4 | Retry history is visible in the admin dashboard |

### Out of scope
- Alternative payment methods
- Offline mode
