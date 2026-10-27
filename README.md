# Bjaao

A music landing page with Firebase-powered rooms. Devices in a room appear
in a live list, and each device controls its own playlist and playback.

## Set up Firebase connectivity

Rooms require a Firebase project. The room code alone does not connect devices;
Firebase provides the shared realtime connection.

1. In [Firebase Console](https://console.firebase.google.com/), create a
   project and register a **Web app**.
2. In **Build → Authentication → Sign-in method**, enable **Anonymous**.
3. In **Build → Realtime Database**, create a database. Choose a location and
   copy its database URL.
4. Copy the Firebase web app settings from **Project settings → Your apps**
   into `firebase-config.js`. Replace every `YOUR_FIREBASE_...` placeholder,
   including `databaseURL` with the exact URL shown in the console.
5. Install the Firebase CLI, sign into the Google account that owns the
   project, and select your project:

   ```bash
   npm install -g firebase-tools
   firebase login
   firebase projects:list
   firebase use --add
   ```

   When prompted, select the project from step 1 and use `default` as its
   alias. The selected project mapping is kept in `.firebaserc`, which is
   intentionally excluded from Git.
6. Publish the database rules and website from the project folder:

   ```bash
   firebase deploy --only database,hosting
   ```

   The Firebase CLI prints the public Hosting URL when deployment completes.
   Open that URL on each phone/computer, create a room, and share the room
   code. Members and device status update live.

The project includes `firebase.json` for Hosting and
[`database.rules.json`](./database.rules.json) for Realtime Database security.
The rules only let signed-in users join a room as themselves, and only let a
member read that room or update their own device status. A device presence
entry and room membership are removed when that browser disconnects.

For local development, start a static server after setting up Firebase:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/music.html
```

Open **Authentication → Settings → Authorized domains** in Firebase Console
and add your Hosting domain if it is not already listed. To connect phones
while testing locally, they need to open the computer's LAN IP and port, and
the computer's firewall must allow it; deploying with Firebase Hosting is the
simpler option.

## Included files

- `music.html` – main webpage
- `WhatsApp Audio 2026-10-27 at 1.29.39 PM.mp4` – Diwali audio used by the page

## Notes

The supplied Diwali audio is the only file this page can play directly.
Other playlist tracks link to YouTube; adding full playable catalogs requires
licensed audio sources or a supported streaming provider. Firebase's web
configuration is public client configuration, not an admin credential; keep
the database rules enabled and never put service-account keys in this page.
