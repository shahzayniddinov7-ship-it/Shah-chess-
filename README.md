# chess.shah🥶 — Android + Website

A modern responsive chess app starter. It includes:
- Legal chess moves, check/checkmate/draw rules (powered by `chess.js`).
- Local game against a simple minimax computer opponent.
- Online private rooms and shareable room links using Firebase Realtime Database.
- Android packaging through Capacitor.

## 1. Run the website locally

Install Node.js 20 or newer, then:

```bash
npm install
npm run dev
```

Open the local URL shown by Vite. The computer mode works without any cloud setup.

## 2. Enable online rooms (Firebase)

Online rooms need a Firebase project. This is a free-tier backend option; usage limits and pricing are controlled by Firebase.

1. Create a project at https://console.firebase.google.com/.
2. Add a Web App in Project settings and copy its config.
3. Enable **Realtime Database**.
4. Copy `.env.example` to `.env` and fill in the values from your Firebase web app config. `VITE_FIREBASE_DATABASE_URL` must be the database URL Firebase gives you.
5. For a quick private test, you can temporarily use these Realtime Database rules:

```json
{
  "rules": {
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

**Important:** those open rules are only for a short test with people you trust. Anyone who knows a room ID could read or change its game. Before public launch, add Firebase Authentication, validate player membership and moves on a trusted server, and lock down database rules. Do not publish a production app with open write rules.

Restart the dev server after changing `.env`.

## 3. Publish the website and use links

Build with:

```bash
npm run build
```

Deploy the generated `dist/` folder to a static host such as Firebase Hosting, Netlify, or Vercel. Configure the host to serve `index.html` for app routes. Once deployed, room links will look like:

`https://your-domain.example/?room=ABC123`

The friend opens the link in any modern browser. Both players need internet access and the same Firebase project configuration in the deployed app.

## 4. Build the Android app

Install Android Studio and its Android SDK. Then:

```bash
npm install
npm run android:add
npm run android:sync
npm run android:open
```

Android Studio opens the native project. Select a device/emulator and press Run. To create a release APK, use **Build → Generate Signed Bundle / APK** in Android Studio.

If you change the website code later, run `npm run android:sync` again before rebuilding Android.

## Current prototype limitations

- The computer opponent is a lightweight beginner-level search, not a strong chess engine.
- Online rooms are a prototype: there is no login, player authentication, server-side move validation, clock synchronization, chat, or reconnect protection yet.
- Promotion currently defaults to a queen. A promotion picker can be added next.
- The displayed 10:00 values are visual placeholders; timed chess clocks are not wired up yet.
