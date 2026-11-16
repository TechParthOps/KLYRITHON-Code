# KLYRITHON CODE — Changelog

All notable changes to KLYRITHON CODE are documented in this file.

## [0.1.2] - 2026-11-16

**Windows installer:** `KLYRITHON-CODE-Setup-0.1.2.exe`  
**Architecture / installer:** x64 NSIS, per-user installation.

The supplied release notes state that installing over an earlier version preserves existing projects, conversations, provider profiles, API keys, and settings. Verify this behavior against the release build and back up important data before upgrading.


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

## [released]

### Planned

- Future improvements will be documented here as they are implemented and verified.

---

For downloads and release assets, see:

https://github.com/TechParthOps/KLYRITHON-Code/releases
