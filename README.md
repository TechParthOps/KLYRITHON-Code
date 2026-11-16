# KLYRITHON CODE

An independent, production-grade **AI coding agent** desktop application for
Windows 10/11 (64-bit). You open a local project, describe work in natural
language, and KLYRITHON plans, reads your code, edits real files, runs approved
commands, verifies the result, and reports honestly — with full persistence
across restarts.

It is a **coding agent, not a chatbot**: every change it claims is backed by
recorded tool calls, file diffs, and exit codes.

**Current release: v0.1.2** —
`apps\desktop\release\KLYRITHON-CODE-Setup-0.1.2.exe`

## License and Distribution

KLYRITHON CODE is proprietary software distributed under a custom license.

The official GitHub repository provides compiled executable releases for public download. The application source code is not publicly distributed.

Users are permitted to download, install, and use KLYRITHON CODE for personal, educational, research, and commercial purposes, subject to the terms of the included LICENSE.

Redistribution, modification, resale, sublicensing, and reverse engineering are restricted, except where applicable law provides otherwise.

All rights to the original software, branding, and intellectual property are reserved by the copyright holder.

See [LICENSE] for complete terms.

---

## Install

1. Run `KLYRITHON-CODE-Setup-0.1.2.exe` (NSIS installer, per-user — no admin
   rights needed, no code-signing certificate configured).
2. Installing over an older version (0.1.0 / 0.1.1) upgrades the app in place
   and **keeps all your data**: projects, conversations, provider profiles, API
   keys and settings are never touched by install or uninstall.
3. Launch **KLYRITHON CODE** from the Start menu or desktop shortcut.

App data lives in `%APPDATA%\KLYRITHON CODE\` (SQLite database + encrypted
credential vault). It is never deleted by the uninstaller.

---

## Quick start

1. **Add a provider** — Settings → Providers → *Add provider*. Pick a preset
   (OpenAI, OpenRouter, Groq, Cerebras, SambaNova, **NVIDIA NIM**, Z.ai,
   Cloudflare, LM Studio, Ollama, custom) and paste your API key. Keys are
   stored encrypted via Electron safeStorage (Windows DPAPI) — they never
   appear in the UI or logs after saving.
2. **Open a project** — Projects → open a local folder. This folder is the
   workspace boundary: KLYRITHON cannot read or write outside it (canonical
   path guard, enforced on every filesystem operation).
3. **Describe the work** — in Chat, type a task, e.g.
   *"Create a product website with an index page, stylesheet and a start.bat
   so I can test it"*. Pick a workflow (Auto is right for most work) and Send.
4. **Approve permissions** — the first time the agent needs a file edit, a
   command, or an operation you have not granted yet, a permission prompt
   appears. Approve once, for the session, or always for this project — your
   choice, and critical/destructive operations always ask again.
5. **Watch it work** — the right-hand Task panel shows the live plan, every
   tool call, and file changes. The chat shows the agent's narration between
   steps; raw tool JSON stays in the task timeline, not the chat.
6. **Check the result** — tasks end with an **evidence-based summary card**:
   outcome (verified / unverified / partially completed / failed), files
   changed, commands run, and honest issues if anything blocked. When files
   changed and verification is enabled, KLYRITHON runs the project's
   tests/build itself and feeds failures back for bounded repair.

---

## How it behaves (v0.1.2)

### Task execution engine
- **Plan → act → verify loop.** For multi-step work the agent first presents a
  numbered plan, then executes through 20 validated tools (read/write/edit/
  create/delete files, search, git, run commands, run tests, …) — each call
  passing schema validation → checkpoint → permission → bounded execution →
  recorded evidence.
- **Honest outcomes.** `completed_verified` only when checks actually passed;
  `completed_unverified` when work finished but no checks could run;
  `partially_completed` when work remains; `failed` when blocked. Summaries
  are generated from real tool records — never from the model's self-report.
- **Crash safety.** Everything is persisted (SQLite WAL). If the app or task
  is interrupted, completed work is checkpointed and shown as *interrupted* —
  nothing auto-repeats. "Continue the session" resumes from the checkpoint.

### Limits that protect you (and how v0.1.2 improved them)
- **Task time limit** — default **60 minutes** per task (v0.1.1 raised this
  from 10 minutes; existing installs are migrated automatically). If the limit
  is reached the task stops *with an explicit reason in the summary* (not a
  silent "cancelled"), keeps all checkpointed work, and can be continued.
  Adjustable in Settings → Agent behavior (1–60 min).
- **Request token budget** — default **6,000 tokens** per model request
  (v0.1.2). Every request the agent sends is compacted to fit: old tool output
  is elided, the oldest history is dropped, tool-call pairing is never broken,
  and the newest request always stays intact. This matters on metered/free
  API tiers — e.g. NVIDIA NIM's free tier rejects any request over
  7,000 input tokens/minute with HTTP 413. Previously tasks failed with
  *"Request too large (ITPM)"*; now they are compacted automatically, and if a
  provider still reports "too large", the agent shrinks the budget and retries
  before ever giving up. Raise the budget in Settings → Agent behavior if your
  provider allows larger requests.
- **Iteration / tool-call caps** — max 30 iterations and 60 tool calls per task
  attempt by default (configurable). Reaching them yields a truthful
  *partially_completed* summary with the remaining work, not a silent stop.
- **Rate limits** — HTTP 429 (per-minute provider limits) are retried
  automatically with backoff; the task slows down briefly instead of failing.
- **Failover** — if a provider/model fails, the router moves to the next
  configured profile; every attempt is labelled in the task panel.

### Security model (always on)
- The renderer never touches the filesystem, processes, or your API keys.
- Workspace path guard on every file operation: canonical paths,
  case-insensitive Windows comparison, traversal/symlink/junction blocked.
- Model output is treated as untrusted: no shell commands are ever parsed out
  of free text for execution — actions go through the tool broker only.
- Repository instruction files (AGENTS.md / CLAUDE.md in *your* projects) can
  influence coding style only — they can never weaken security or permissions.
- Destructive operations (delete, anything outside the project) require fresh
  explicit approval; grants are never re-used for critical operations.
- Secrets are redacted in all logs and events.

---

## Settings reference (Settings → Agent behavior)

| Setting | Default | What it does |
|---|---|---|
| Verification after tasks | on | Runs the project's tests/build after file changes and feeds failures back for repair |
| Planning for complex tasks | on | Agent presents a numbered plan before multi-step work |
| Max iterations per task | 30 | Model↔tool loop rounds per attempt |
| Max tool calls per task | 60 | Tool executions per task attempt |
| Task time limit (minutes) | 60 | Wall-clock cap per task; stop is reported honestly and work stays checkpointed |
| Request token budget | 6,000 | Approximate input-token cap per model request; history is compacted to fit. Keep below your provider's per-request/per-minute input limit |
| Routing mode | Auto | Auto / Fast / Powerful / Free only / Local only / Manual |

---

## Troubleshooting

- **"HTTP 413 / Request too large" in the task panel** — your provider's input
  token cap is lower than the *Request token budget* setting. Lower the budget
  (e.g. 5,500 for NVIDIA NIM free tier) and retry. v0.1.2 already compacts
  automatically; this message should now be rare and self-healing.
- **Task marked "cancelled" but you did not cancel** — on v0.1.2 this only
  happens at the task time limit, and the summary says so explicitly with the
  elapsed limit. Raise the limit in Settings or continue the session.
- **Task dies with `permission_denied` after ~10 minutes** — a permission
  prompt was left unanswered; it auto-denies after 10 minutes for safety.
  Answer prompts when they appear (v0.1.0 bug fixed in 0.1.1).
- **Provider keeps failing** — check the API key and model id in
  Settings → Providers; the task panel shows each failover attempt. Per-profile
  circuit breakers cool down failing providers for 60 s.
- **Offline / no keys?** — enable *Fixture provider* in Settings → Agent
  behavior. It is a labelled diagnostic/demo adapter (clearly marked, never
  shown as a real cloud provider) that can execute real file edits end-to-end
  without any API.

---

## What v0.1.2 fixed (vs 0.1.1 / 0.1.0)

- **v0.1.2 — "every task fails with HTTP 413 Request too large"**: added
  request token budgeting, history/tool-output compaction (pairing-safe),
  system-prompt shrinking, and adaptive shrink-and-retry on 413/context errors;
  new *Request token budget* setting. Verified: 81/81 automated tests
  (8 new budget tests), packaged self-check 10/10.
- **v0.1.1 — "tasks stop mid-execution, shown as cancelled after 600s"**:
  task wall-clock default raised 10 min → 60 min with automatic migration of
  stored old defaults; limit exposed in Settings; wall-clock stops are reported
  truthfully instead of faking a user cancel.
- **v0.1.0** — first release: full agent loop, 20 tools, multi-provider
  gateway, SQLite persistence, permission system, workspace, installer.

See `DEVELOPMENT_STATE.md` for the honest verified/unverified breakdown,
`SECURITY.md` for the security model, `FEATURES.md` for the full feature
inventory, and `KNOWN_LIMITATIONS.md` for accepted v0.x limitations (pipe-based
terminal, PDF/DOCX/XLSX attachments not parsed yet, heuristic project memory).

---

## Build from source

Requirements: Windows 10/11 x64, Node.js ≥ 20, pnpm.

```powershell
pnpm install          # workspaces
pnpm typecheck        # 11/11 projects
pnpm test             # 81/81 tests (no external AI APIs required)
pnpm build            # production build
pnpm dist             # NSIS installer → apps/desktop/release/
pnpm app:selfcheck    # packaged self-check, exits 0/1
pnpm dev              # dev Electron app
```

License: proprietary — © KLYRITHON. All rights reserved.
