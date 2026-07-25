# Firebase Setup

The dashboard is a static GitHub Pages site. Shared online edits use Firestore.

## 1. Create Firebase

1. Create or open a Firebase project.
2. Add a Web app.
3. Copy the generated Firebase config into a local `firebase-config.js` file.
4. Enable Firestore Database.

## 2. GitHub Pages Configuration

The production site receives its Firebase configuration only while GitHub
Actions builds the Pages artifact. Store the complete local
`firebase-config.js` file as the repository secret `FIREBASE_CONFIG`:

```bash
gh secret set FIREBASE_CONFIG --repo chatgptricks/sentient-project-status-dashboard < firebase-config.js
```

`firebase-config.js` is intentionally ignored by Git. Use
`firebase-config.example.js` as the checked-in development template.

The same deployment also expects `NASA_API_KEY`, stored as a GitHub Actions
secret. It supplies the random high-resolution APOD wallpaper.

## 3. Firestore Document

The dashboard writes one document:

```text
dashboards/sentient-project-status
```

Shape:

```json
{
  "scores": {
    "Sentient CRM": 88
  },
  "content": {
    "Sentient CRM": {
      "advancements": ["..."],
      "pending": ["..."]
    }
  },
  "updatedAt": "server timestamp"
}
```

## 4. Temporary Rules

For a quick private handoff, restrict editing before sharing widely. If you need
fast testing first, these public rules work technically but are not appropriate
for a public link long term:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /dashboards/sentient-project-status {
      allow read: if true;
      allow write: if true;
    }
  }
}
```

Safer production options:

- Use Firebase Auth and allow writes only for selected signed-in accounts.
- Keep public read enabled and write through a protected admin flow.
- Use App Check plus Auth if the link will be broadly shared.
