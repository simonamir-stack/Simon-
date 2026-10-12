# Public link setup (no sign-in)

This puts the calendar at a plain web address anyone in the band can open, and saves answers in a free Firebase database. It takes about 10 minutes, once.

## 1. Create the database (Firebase, free)

1. Go to https://console.firebase.google.com and sign in with your Google account.
2. Click **Create a project**. Name it `dentallica-calendar`. You can turn off Google Analytics.
3. In the left menu open **Build > Firestore Database** and click **Create database**.
   - Location: any US location (for example `nam5`).
   - Start in **production mode**.
4. Open the **Rules** tab, replace everything with the rules below, and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /members/{member} {
      allow read: if true;
      allow write: if member in ['Jamie', 'James', 'Phillip', 'Simon']
        && request.resource.data.keys().hasOnly(['days']);
    }
  }
}
```

   These rules let anyone with the link read and mark nights for the four of you, and nothing else.

5. Click the gear icon > **Project settings**. Under **Your apps**, click the web icon `</>`, name it `calendar`, and click **Register app** (skip Firebase Hosting).
6. Copy the `firebaseConfig` block it shows. It looks like this:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "dentallica-calendar.firebaseapp.com",
  projectId: "dentallica-calendar",
  storageBucket: "dentallica-calendar.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
};
```

7. Send that block to Claude, or paste it into `index.html` in place of `const FIREBASE_CONFIG = null;`. These values identify the project; they are not passwords.

## 2. Put the page online (GitHub Pages, free)

GitHub Pages needs the repository to be public on a free GitHub plan. The repository holds only the calendar page, the logo images and these notes.

1. On GitHub open **simonamir-stack/Simon-** > **Settings** > **General**, scroll to **Danger Zone**, and choose **Change visibility > Make public**.
2. Open **Settings > Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, pick the branch `claude/dentallica-availability-calendar-cb9n2r` (or `main` once it is merged) and folder `/ (root)`, then **Save**.
3. After a minute or two the calendar is live at:

   **https://simonamir-stack.github.io/Simon-/**

Send that link to the band. Nobody needs an account.

## Good to know

- Anyone who has the link can mark nights for any of the four of you. Keep the link within the band.
- Answers entered in the claude.ai version don't carry over to this one; they are separate databases.
- If you'd rather keep the repository private, the same `index.html` and `assets` folder can be dragged onto https://app.netlify.com/drop instead of using GitHub Pages.
