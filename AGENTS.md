# tzuchistudy Firebase Tools - AGENTS.md

## Project Context

Project name: `tzuchistudy-firebase-tools`
Purpose: classroom tool workspace for a realtime Firebase Firestore word cloud demo.
Local folder: `G:\我的雲端硬碟\2026Databsse`
GitHub repo: `https://github.com/scotthcliu-jpg/tzuchistudy-firebase-tools`
Default branch: `master`
Firebase project: `tzuchistudy`
Firestore database: `projects/tzuchistudy/databases/(default)`

## Working Style

Use Traditional Chinese for user-facing communication. Keep responses concise, practical, and evidence-based.

Before answering substantial requests, rewrite the request into:

- role task
- background
- concrete instructions
- constraints

Only state facts that have been verified or are clearly sourced from project files. If confidence is below 90%, say the information is insufficient.

## Project Files

- `wordcloud.html`: realtime word cloud demo.
- `firestore.rules`: Firestore security rules.
- `firebase.json`: Firebase CLI project config.
- `.firebaserc`: Firebase project alias, currently `tzuchistudy`.
- `FIREBASE_CODEX_SETUP.md`: Codex/Firebase setup and troubleshooting record.
- `README.md`: user-facing project summary and run instructions.

## Obsidian Record

Use Obsidian for durable setup records and project handoff notes.

Current project note:

```text
Projects/Codex/2026-05-03-codex-firebase-tzuchistudy.md
```

When updating setup knowledge, include:

- what changed
- verification evidence
- commands or tools used
- pitfalls and fixes
- unresolved risks

## Firebase Rules

Current Firestore rules intentionally allow public read/write for:

```text
wordcloud_words
```

All other Firestore document paths are denied.

Do not store sensitive data in `wordcloud_words`. Do not broaden public rules without explicit approval.

Deploy rules from the repo root:

```powershell
npx.cmd -y firebase-tools@latest deploy --only firestore:rules --project tzuchistudy
```

Use `npx.cmd`, not `npx`, on Windows.

## Codex Skills

This project expects these local Codex skills:

- `startup-sync`: read context, Obsidian notes, Git state, Firebase state before work.
- `shutdown-sync`: summarize work, update Obsidian, commit and push intended changes.
- `project-init-sync`: initialize or repair classroom tool workspace files and integrations.

Trigger phrases:

- `開工` or `啟動同步`
- `收工` or `結束同步`
- `初始化班級工具工作模式`

## Git And GitHub

Before committing:

1. Run `git status -sb`.
2. Review diffs.
3. Stage only intended files.
4. Commit with a concrete message.
5. Push to the configured GitHub remote when requested.

Do not run destructive Git commands such as reset or checkout unless the user explicitly asks.

## GitHub Pages

The repo is public. GitHub Pages may be enabled for the static `wordcloud.html` demo, but confirm the exact Pages source before changing remote settings.

Suggested source if enabled:

```text
branch: master
path: /
```

## Safety Rules

- Do not commit `.codex/`, `.claude/`, `.env`, service account JSON, Admin SDK keys, tokens, or credentials.
- Do not use Firebase `login:ci` unless the user explicitly approves creating a long-lived credential.
- Delete temporary Firebase test documents after verification.
- Treat MCP config and current-session MCP tool availability as separate checks.
- Prefer ASCII filenames for setup records when crossing PowerShell, Node, Git, and MCP tools.
