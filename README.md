# Mötestid 🗓️

> *Hitta när alla kan* — a lightweight scheduling tool for finding meeting times that work for everyone.

---

## What it does

Mötestid lets a session organizer create a shared availability session, distribute unique personal links to each participant, and instantly see overlapping free slots in real time — no accounts, no logins, no back-and-forth emails.

Built for situations like coordinating meetings with Arbetsförmedlingen, Försäkringskassan, healthcare providers, or any group of people who can never seem to find a time.

---

## How it works

1. **Create a session** — give your meeting a name and add 2–6 participants
2. **Share links** — each person gets their own unique URL
3. **Mark availability** — participants click or drag to highlight free hours on a weekly grid (08:00–18:00)
4. **See results** — the results page updates live, highlighting slots where everyone is free

---

## Features

- 🟥 **Color-coded participants** — each person has a distinct color on the grid
- ⚡ **Real-time sync** — availability updates live via Firebase Realtime Database
- 📅 **Week navigation** — scroll forward/backward through weeks to mark future availability
- 👆 **Click & drag** — tap a single slot or drag across multiple to quickly mark ranges
- 📱 **Mobile-friendly** — touch drag supported, responsive layout
- 🌑 **Dark mode UI** — warm dark theme built with CSS custom properties
- 🔒 **No accounts needed** — session links are the only access mechanism

---

## Tech stack

| Layer | Technology |
|---|---|
| UI | React 18 (via CDN, no build step) |
| Styling | CSS custom properties + DM Mono / Instrument Serif (Google Fonts) |
| Database | Firebase Realtime Database |
| Hosting | Any static file host (single HTML file) |

---

## Setup

### 1. Firebase

This tool requires a Firebase project with **Realtime Database** enabled.

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a project
2. Enable **Realtime Database** (start in test mode or configure rules as needed)
3. Copy your project config

### 2. Configure

Open `index.html` and replace the `firebaseConfig` block near the top:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.YOUR_REGION.firebasedatabase.app",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Deploy

Since this is a single HTML file with no build step, just serve it statically:

```bash
# Quick local preview
npx serve .

# Or upload index.html to any static host:
# GitHub Pages, Netlify, Vercel, Firebase Hosting, etc.
```

---

## Firebase database rules (recommended)

```json
{
  "rules": {
    "sessions": {
      "$sid": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

> ⚠️ For production use, tighten these rules to prevent unauthorized writes to other sessions.

---

## Data structure

```
sessions/
  {sessionId}/
    meta/
      sid:       string
      title:     string
      created:   timestamp
      persons:   [ { id, name, color } ]
    avail/
      {personId}/
        {dateStr}__{hour}: true   ← e.g. "2025-03-10__14": true
```

---

## URL routing

The app uses hash-based routing (no server config needed):

| Route | View |
|---|---|
| `#` | Home / create session |
| `#session/{sid}/results` | Results & link dashboard |
| `#session/{sid}/person/{pid}` | Individual availability picker |

---

## Limitations

- Hours are fixed to **08:00–18:00**
- Sessions have no expiry (clean up old data manually in Firebase console)
- No authentication — anyone with a session link can view results
- Up to **6 participants** per session

---

## License

MIT — do whatever you want with it.


