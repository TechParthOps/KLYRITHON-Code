# KLYRITHON CODE — Changelog

All notable changes to KLYRITHON CODE are documented in this file.

## [0.1.2] - 2026-11-16

**Windows installer:** `KLYRITHON-CODE-Setup-0.1.2.exe`  
**Architecture / installer:** x64 NSIS, per-user installation.

The supplied release notes state that installing over an earlier version preserves existing projects, conversations, provider profiles, API keys, and settings. Verify this behavior against the release build and back up important data before upgrading.

### Fixed

- **Requests failing with HTTP 413 / “Request too large”** on providers with input-token limits.
  - Added a configurable request-token budget, with a reported default of 6,000 tokens.
  - Added conversation compaction that groups assistant tool-call turns with their corresponding tool results so a tool interaction is not split apart.
  - Older tool outputs and large tool arguments can be shortened with explanatory notes, and older conversation units can be dropped when necessary.
  - The newest user request and newest conversation units are prioritized during compaction.
  - When a request still exceeds the provider's limit, the orchestrator reduces the budget and retries the same pass before treating the issue as a provider-failover condition.
  - This addresses the previous behavior in which HTTP 413 was treated as a quota/provider failure and failover could repeat the same oversized request against another profile.

### Added

- **Request token budget** setting under Settings → Agent behavior.
  - Reported range: 1,000–200,000 tokens.
  - Reported default: 6,000 tokens.
  - The appropriate value depends on the selected provider's request and input-token limits.
- Automated tests for request-budget compaction, including:
  - Preserving a request unchanged when it fits the budget.
  - Eliding older content while retaining recent results.
  - Dropping older conversation units with an explanatory note under tight budgets.
  - Keeping tool calls paired with their results.
  - Protecting the newest conversation units.
  - Keeping elided tool arguments valid JSON.
  - Shrinking the system prompt while retaining required rules.

### Verification reported for this release

The supplied release changelog records these results:

| Check | Reported result |
|---|---|
| `pnpm -r typecheck` | 11/11 workspace projects clean |
| `pnpm test` | 81/81 tests passed across 8 suites |
| `pnpm app:build` | Production build completed cleanly |
| Packaged self-check | 10/10 passed; exit code 0 |

The packaged self-check is reported to cover SQLite and migrations, services, path guard, permission matrix, fixture provider, agent-loop file editing, restart recovery, and permission-denial recording.

These are results reported by the project; they have not been independently reproduced as part of this changelog revision.

## [Unreleased]

### Planned

- Future improvements will be documented here as they are implemented and verified.

---

For downloads and release assets, see:

https://github.com/TechParthOps/KLYRITHON-Code/releases
