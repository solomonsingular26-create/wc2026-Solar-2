# World Cup 2026 Predictor ⚽ (Firebase version)

A real web page — plain HTML, CSS, and JavaScript — for predicting every game of the 2026 World Cup
with your friends. Everyone uses their own phone and shares one live leaderboard. All 48 teams in
their 12 groups, all 72 group games, and the full knockout bracket to the final (104 matches).

- **No logins.** Open the link, tap your name, predict.
- **Picks lock the moment you save them.**
- **Live shared leaderboard** (exact score +20, correct result +15, miss 0).
- **Backfill built in** — enter results of games already played, and type in past picks by hand.

Data lives in **Firebase (Firestore)** — its own free, separate database, nothing shared with your
other projects. The games load themselves into it automatically the first time the page runs.

---

## The files

```
index.html        the page itself
styles.css        how it looks (edit colors/sizes here)
app.js            how it works — also has the 104 fixtures baked in
config.js         ← the ONLY file you must edit (paste your Firebase config)
firestore.rules   the access rules to paste into Firebase (open, no-login)
```

---

## Setup — click by click (~10 minutes)

### 1 · Create a Firebase project
1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Add project**, give it a name (e.g. *wc2026-predictor*), continue.
3. Google Analytics is optional — you can turn it **off**. Click **Create project**.

### 2 · Turn on the database (Firestore)
1. In the left menu: **Build → Firestore Database → Create database**.
2. Choose a location near you, and start in **Test mode** (we'll lock it to our own rules next).
   Click **Create**.

### 3 · Set the access rules
1. Still in Firestore, open the **Rules** tab.
2. Delete what's there, paste the contents of `firestore.rules` from this folder, and click
   **Publish**. (This keeps it open for your pool and stops the test-mode rules from expiring.)

### 4 · Register a Web app and copy the config
1. Click the **gear icon → Project settings**.
2. Scroll to **Your apps**, click the **`</>` (Web)** icon, give it any nickname, **Register app**.
   (You can skip "Firebase Hosting".)
3. It shows a `firebaseConfig = { ... }` block. Copy those values.

### 5 · Paste the config into the page
Open `config.js` and fill in the values you just copied:
```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "wc2026-predictor.firebaseapp.com",
  projectId: "wc2026-predictor",
  storageBucket: "wc2026-predictor.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123...:web:abc...",
};
```

### 6 · Try it
**Double-click `index.html`** to open it. The first time it runs it writes all 104 games into your
database automatically — give it a couple of seconds, then the fixtures appear. Make a pick; if it's
still there after refreshing, you're connected.

> If your browser blocks it when opened directly, run a tiny local server: `npx serve` (needs
> Node.js) and open the address it prints.

---

## Put it online with Vercel

**Easiest (drag & drop):** at https://vercel.com, **Add New → Project → deploy a folder**, and drop
this folder in. No framework, no build settings.

**Or via GitHub:**
```bash
git init && git add . && git commit -m "WC2026 predictor"
git remote add origin https://github.com/YOU/wc2026-predictor.git
git branch -M main && git push -u origin main
```
Import the repo on Vercel and Deploy. **If you get a 404**, your files are in a subfolder — in the
Vercel project go to **Settings → Build and Deployment → Root Directory** and set it to the folder
name, then redeploy.

Share the Vercel link with your friends. Done.

---

## How everyone uses it

- **Fixtures** – tap **"Who are you?"**, pick your name (remembered after). Type a score and hit
  **lock in pick**. Once locked it shows 🔒 and can't be changed.
- **Leaderboard** – live standings as results come in.
- **Manage** – the pool-runner's tools (no password, so share this part only with whoever runs it):
  - **Results** – enter any game's final score, including ones already played; locks it and scores it.
  - **Deadlines** – set when picks close for each game. Enter a kickoff time and predictions
    auto-lock at that moment, or hit **Close now** to lock a game instantly. Each section also has a
    **Close all now** button to lock a whole matchday/round at once. After the deadline, players
    can't add or change a pick for that game.
  - **Knockout** – knockout games start "TBD vs TBD"; type the two teams as the bracket fills.
  - **Predictions** – pick a player and enter/fix their scores (backfilling past picks, fixing typos).

---

## Changing things

- **Scoring:** near the top of `app.js` → `const POINTS = { EXACT: 20, RESULT: 15, MISS: 0 };`
- **Frozen games:** `const EXCLUDED_MATCH_IDS = [...]` near the top of `app.js` lists games that are
  locked from predictions and left out of the leaderboard. Add or remove match IDs to freeze/unfreeze.
- **Players:** anyone adds themselves with **+ new** on Fixtures. To change the starting six, edit the
  `PLAYERS` line at the very top of `app.js` *before* the first run.
- **Look:** `styles.css` — colors are at the very top under `:root`.

> The games seed only once (the app checks if the database is empty first), so re-opening the page
> won't create duplicates.

## A note on "no logins"

There are no passwords (you asked for that), so anyone with the link can pick any name and open the
Manage tab. Perfect for a friendly pool — just keep the link to your group and don't store anything
sensitive.
