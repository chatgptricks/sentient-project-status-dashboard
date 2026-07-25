# Sentient Project Status

Static project status dashboard for Sentient and adjacent operating projects.

The site is published through GitHub Pages from `index.html`.

Shared online edits are supported through Firebase/Firestore. The Firebase
configuration is injected into the deployed Pages artifact from an encrypted
GitHub Actions secret; it is never committed. See
[`FIREBASE_SETUP.md`](FIREBASE_SETUP.md).
