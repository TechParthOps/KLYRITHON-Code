# KLYRITHON CODE — Third-Party Notices

**Application version:** 0.1.2  
**Copyright:** © 2026 KLYRITHON. All rights reserved in the original proprietary components of KLYRITHON CODE.

This document identifies third-party software reported as bundled with or used to build KLYRITHON CODE v0.1.2. Third-party components remain subject to their own licenses. The proprietary license for KLYRITHON CODE does not replace or restrict rights granted by those licenses.

> **Verification note:** Component names, versions, and license identifiers below are taken from the project's supplied v0.1.2 notice file. They have not been independently reconciled against the package-manager lockfile or final installer payload in this review. Confirm them against the exact release build before publishing.

## 1. Runtime dependencies reported as distributed with the application

| Component | Reported version | Reported license |
|---|---:|---|
| Electron | 38.8.6 | MIT |
| React | 18.3.1 | MIT |
| React DOM | 18.3.1 | MIT |
| Zustand | 5.1.0 | MIT |
| Zod | 3.25.76 | MIT |
| better-sqlite3 | 12.11.1 | MIT |
| better-sqlite3-electron (alias) | 12.10.0 | MIT |
| Monaco Editor | 0.52.2 | MIT |
| @monaco-editor/react | 4.7.0 | MIT |
| @xterm/xterm | 5.5.0 | MIT |
| @xterm/addon-fit | 0.10.0 | MIT |
| marked | 15.0.12 | MIT |
| DOMPurify | 3.4.18 | MPL-2.0 OR Apache-2.0 |

The two better-sqlite3 entries are listed separately because that is how they appear in the supplied inventory. Confirm whether the alias is a separately packaged component or only an alternate package name; avoid double-counting it if it is only an alias.

## 2. Embedded components

The supplied inventory also identifies these components as distributed indirectly through runtime dependencies or installer packaging:

| Component | How it is included | License / notice |
|---|---|---|
| Chromium | Bundled with Electron | Chromium license and third-party notices supplied with the Electron distribution |
| Node.js | Bundled with Electron | Node.js license and applicable notices supplied with the Electron distribution |
| SQLite | Embedded through better-sqlite3 | Public domain; https://sqlite.org/copyright.html |
| NSIS | Used by the Windows installer build | zlib/libpng license, according to the supplied inventory |

Preserve the upstream license and notice files included with Electron and other bundled distributions. In particular, do not remove Electron's Chromium third-party notices or Node.js license materials from the packaged distribution.

## 3. Build-time and development dependencies

The following are reported as build-time/development tools and are not intended to be shipped as runtime application dependencies in the installer.

| Component | Reported version | Reported license |
|---|---:|---|
| TypeScript | 5.9.3 | Apache-2.0 |
| Vite | 6.4.6 | MIT |
| @vitejs/plugin-react | 4.7.0 | MIT |
| esbuild | 0.25.12 | MIT |
| electron-builder | 26.18.0 | MIT |
| Vitest | 3.2.7 | MIT |

Build-time dependencies can still have distribution or notice implications if their code or artifacts are copied into the published application. Reassess this section whenever the build configuration changes.

## 4. License texts and upstream references

Include the applicable license texts and notices for the exact versions distributed. The references below are starting points; verify each package's own license file and preserve required copyright and attribution notices.

- MIT License: https://opensource.org/license/mit/
- Apache License 2.0: https://www.apache.org/licenses/LICENSE-2.0
- Mozilla Public License 2.0: https://www.mozilla.org/MPL/2.0/
- NSIS: https://nsis.sourceforge.io/License
- SQLite: https://sqlite.org/copyright.html
- Electron notices and licenses: https://www.electronjs.org/

DOMPurify is reported as dual-licensed under MPL-2.0 or Apache-2.0. Confirm the exact license option and obligations applicable to the distributed copy.

A URL or short excerpt may not satisfy every redistribution obligation. Include complete license texts and required notices in the distribution where the relevant license requires them.

## 5. Third-party services and APIs

KLYRITHON CODE may connect to external AI providers, APIs, and developer services. An external service is not necessarily bundled with the application. Its use is governed by the provider's own terms, privacy policy, and API conditions.

Do not interpret the mention of a provider in product documentation as proof that the provider is integrated in this release, or as an endorsement or affiliation.

## 6. Trademarks

Third-party names and trademarks belong to their respective owners. Their appearance in this document is for identification and attribution only and does not imply sponsorship, endorsement, or partnership.

## 7. Reporting an attribution or licensing issue

Please report a suspected missing notice, incorrect license identifier, or other third-party licensing concern through:

https://github.com/TechParthOps/KLYRITHON-Code/issues

## 8. Release maintenance

This notice is specific to the reported KLYRITHON CODE v0.1.2 dependency inventory. Review and update it whenever dependencies, versions, packaging, or distribution contents change.

---

**KLYRITHON CODE is proprietary software.** Its original components are © 2026 KLYRITHON, all rights reserved, subject to the separate rights and licenses applicable to third-party components.
