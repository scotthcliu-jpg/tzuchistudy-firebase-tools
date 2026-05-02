# Codex Firebase Setup Notes

## Scope

This repository records the working setup for connecting Codex Desktop to Firebase Cloud Firestore through the Firebase MCP server.

The verified Firebase project is:

- Project ID: `tzuchistudy`
- Firestore database: `projects/tzuchistudy/databases/(default)`
- Public collection for the word cloud demo: `wordcloud_words`

Do not use the older project ID `teacherstudy-109ef` for this workspace.

## Files In This Repo

- `.firebaserc`: sets the default Firebase project to `tzuchistudy`
- `firebase.json`: points Firebase CLI to the Firestore rules file
- `firestore.rules`: allows public read/write only for `wordcloud_words`
- `wordcloud.html`: browser-based word cloud demo using Firebase Web SDK and Firestore realtime updates

## Firestore Rules

Current rule behavior:

- `wordcloud_words`: public read/write
- all other document paths: read/write denied

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /wordcloud_words/{document} {
      allow read, write: if true;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

This is intentionally permissive for the demo collection. Do not reuse this rule for sensitive data.

## Deployment Command

Run from this repo root:

```powershell
npx.cmd -y firebase-tools@latest deploy --only firestore:rules --project tzuchistudy
```

Expected success signal:

```text
Deploy complete!
```

## Codex MCP Configuration

Codex Desktop reads MCP servers from:

```text
C:\Users\user\.codex\config.toml
```

The Firebase MCP block is:

```toml
[mcp_servers.firebase]
command = "npx.cmd"
args = ["-y", "firebase-tools@latest", "mcp"]
startup_timeout_sec = 60
tool_timeout_sec = 120
```

After changing this file, restart Codex Desktop. A config file entry only proves that the server is configured; it does not prove that the current Codex conversation has loaded the tools.

## Verification Evidence

Verified on 2026-05-03:

- Firebase CLI version: `15.16.0`
- Firebase CLI could see project: `tzuchistudy`
- Firestore database existed: `projects/tzuchistudy/databases/(default)`
- Rules deployed successfully: `Deploy complete!`
- REST verification created, read, deleted, and confirmed deletion of `wordcloud_words/codex_deploy_test_20260503`
- Codex MCP tools loaded after restart
- MCP verification created, read, listed, deleted, and confirmed deletion of `wordcloud_words/codex_mcp_test_20260503`

Existing real documents were visible in `wordcloud_words`, including entries with words such as:

- `慈濟 感恩 包容 尊重 愛 大愛`
- `慈濟 慈濟 慈濟`
- `大愛 大愛`

## Pitfalls And Fixes

| Pitfall | Symptom | Fix |
|---|---|---|
| Running deploy from the wrong folder | `Not in a Firebase app directory (could not locate firebase.json)` | `cd "G:\我的雲端硬碟\2026Databsse"` before deploying |
| Using the old project ID | Deploy or permission checks target `teacherstudy-109ef` | Use `tzuchistudy`; confirm `.firebaserc` before deploying |
| Firebase account has no project access | `No projects found` or HTTP 403 | Confirm the logged-in Google account can see the project in Firebase Console and has IAM permissions |
| Missing service usage permission | HTTP 403 while enabling or using `firestore.googleapis.com` | Add a role such as `Service Usage Consumer`, plus Firebase/Firestore roles as needed |
| PowerShell blocks `npx.ps1` | script execution policy error | Use `npx.cmd` on Windows |
| Firebase CLI login inside Codex is non-interactive | `Cannot run login in non-interactive mode` | Run `npx.cmd -y firebase-tools@latest login` in a normal PowerShell window |
| CI token workaround is tempting | `login:ci` can generate a reusable token | Avoid unless explicitly approved; it is a sensitive long-lived credential |
| MCP configured but tools not visible | Firebase block exists but no `mcp__firebase__` tools | Restart Codex Desktop and verify tool exposure separately |
| JSON document sent as a string to MCP | `Invalid value at 'document'` | Pass the Firestore `document` object directly, not a JSON-encoded string |
| Query tool can be flaky with read time | `read_time cannot be in the future` | Prefer `firestore_get_document` and `firestore_list_documents` for verification |

## Safe Test Pattern

Use a clearly named document and delete it immediately after verification:

```text
wordcloud_words/codex_mcp_test_YYYYMMDD
```

Verification should include:

1. Create the test document.
2. Read the same document.
3. List the collection if useful.
4. Delete the test document.
5. Read again and confirm `Document not found` or HTTP `404`.

## Security Notes

- Do not commit Firebase Admin SDK private keys.
- Do not commit service account JSON files.
- Firebase Web SDK config is not a private secret, but avoid spreading API keys into documentation unless needed.
- Public read/write rules are acceptable only for this demo collection and should not protect sensitive data.
