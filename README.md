# tzuchistudy Firebase Tools

Realtime classroom word cloud demo backed by Firebase Cloud Firestore.

## Status

- Firebase project: `tzuchistudy`
- Firestore database: `projects/tzuchistudy/databases/(default)`
- Demo collection: `wordcloud_words`
- Codex Firebase MCP: verified
- GitHub repo: `scotthcliu-jpg/tzuchistudy-firebase-tools`

## Files

- `wordcloud.html`: browser-based realtime word cloud.
- `firestore.rules`: Firestore security rules.
- `firebase.json`: Firebase CLI rules config.
- `.firebaserc`: default Firebase project.
- `FIREBASE_CODEX_SETUP.md`: setup notes and troubleshooting.
- `AGENTS.md`: Codex project working rules.

## Firestore Rules

The current rules allow public read/write only for `wordcloud_words`.

```text
wordcloud_words: public read/write
all other paths: denied
```

This is suitable for a classroom demo collection only. Do not store sensitive data in it.

## Deploy Rules

Run from the repository root:

```powershell
npx.cmd -y firebase-tools@latest deploy --only firestore:rules --project tzuchistudy
```

Expected result:

```text
Deploy complete!
```

## Codex Workflow

Local Codex skills used by this project:

- `startup-sync`
- `shutdown-sync`
- `project-init-sync`

Use `開工` to begin a context-aware session, and `收工` to record results in Obsidian and GitHub.

## Safety

Do not commit:

- `.env` files
- service account JSON
- Firebase Admin SDK private keys
- API tokens
- local `.codex/` or `.claude/` folders
