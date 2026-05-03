# 🌮 La Lucha del Taco

A mobile-first, lucha-libre-themed taco crawl scorecard for **Cinco de Mayo 2026** in San Diego's Gaslamp District.

Players join a shared room, rate tacos across 7 categories, spin a challenge wheel, earn personality badges, and crown **El Campeón** at the end of the crawl — all synced live via Firebase.

## Live Demo

> **GitHub Pages:** [https://&lt;username&gt;.github.io/lucha-del-taco/](https://github.io/lucha-del-taco/)
>
> _(Update the link above after enabling Pages.)_

## Features

- **Multiplayer rooms** — one person creates a 4-character room code, everyone else joins; all scoring is live-synced via Firebase Realtime Database
- **Luchador names** — each player picks a name in the lobby; shown as avatar chips throughout the app
- **7 scoring categories** — La Tortilla, El Relleno, La Salsa, El Precio, La Vibra, El Comodín, El Margarita
- **Taco slider thumbs** 🌮 — custom SVG sliders for scoring (1–10)
- **Per-user ratings** — everyone scores independently; the leaderboard shows averaged composites with individual breakdowns
- **Submit & update** — scores are local until you tap "Submit My Ratings"; you can re-submit to update
- **Interactive Leaflet map** — numbered markers + dashed route line for each stop
- **Add / edit / remove stops** on the fly (writes to Firebase, syncs to all users)
- **Photo uploads** — snap a pic at each stop (local/in-memory per device)
- **El Veredicto** — per-stop tasting notes textarea (local per device)
- **🎡 La Ruleta** — per-stop SVG spin wheel that randomly picks a luchador and assigns a challenge from the shared challenge pool
- **⚡ Challenge pool** — room-shared list of custom challenges; manage via modal; stored in Firebase
- **14 personality badges** — auto-computed from rating patterns (e.g. El Disidente, El Exigente, Los Gemelos); shown in a badge gallery with popup descriptions
- **Reveal El Campeón** — confetti-blasting results modal with full rankings, per-category averages, and rater counts
- **Copy results** — shares final standings to clipboard
- **Countdown timer** — builds hype on the splash screen; room creator can adjust the start time via triple-tap on the sticky header date
- **Connection indicator** — live dot shows Firebase connection status
- **Room roster** — avatar chips for all connected luchadores in the room info bar
- **Zero client-side build** — Leaflet 1.9.4 + canvas-confetti 1.9.3 via cdnjs; Firebase 10.12.2 via gstatic

## Tech

Single self-contained HTML file. No build step, no framework.

| Constraint | Detail |
|---|---|
| Language | Vanilla JavaScript |
| Layout | One `index.html` file (~3,673 lines) |
| CDN deps | Leaflet 1.9.4, canvas-confetti 1.9.3 (cdnjs), Firebase 10.12.2 (gstatic) |
| Backend | Firebase Realtime Database (project: `la-lucha-del-taco`) |
| State | Firebase-synced for stops, ratings, users, challenges; in-memory for photos, notes, spin history |
| Fonts | System font stacks (no Google Fonts) |

## Files

```
lucha-del-taco/
├── index.html           ← The app (~3,673 lines)
├── ARCHITECTURE.md      ← Edit-navigation map with line numbers & search anchors
├── emojione_taco.svg    ← Source SVG for the taco slider thumb (inlined as base64)
├── .gitignore
├── LICENSE              ← MIT
└── README.md            ← You are here
```

## GitHub Pages Setup

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch**, branch `main`, folder `/ (root)`
4. The app will be live at `https://<username>.github.io/lucha-del-taco/`

Since the main file is `index.html`, it serves at the root URL automatically.

## How a Crawl Works

1. **One person creates a room** — tap "or create a new room", enter a luchador name, get a 4-character room code. The 5 pre-populated Gaslamp stops are written to Firebase automatically.
2. **Everyone else joins** — enter luchador name + room code → tap "Enter the Ring".
3. **At each stop** — expand the stop card, drag sliders, tap "Submit My Ratings". Scores push to Firebase and the leaderboard updates for everyone instantly.
4. **Spin La Ruleta** — tap 🎡 on any stop to spin the wheel and assign a challenge to a random luchador.
5. **After the last stop** — tap "Reveal the Champion" for the final rankings and confetti.

## Firebase Data Shape

```
rooms/
  {CODE}/
    createdAt:  timestamp
    createdBy:  "luchadorName"
    startTime:  timestamp          ← set by room creator via time adjuster
    stops/
      s1/  { name, desc, address, lat, lng }
      s2/  ...
    ratings/
      {userName}/
        {stopId}/  { tortilla, relleno, salsa, precio, vibra, comodin, margarita }
    users/
      {userName}/  { joinedAt, online }
    challenges/
      {id}/  { emoji, title, text }
```

## Customizing for a Different Crawl

See `ARCHITECTURE.md` §5 (Hardcoded Values) for everything you'd need to change: date, stops, map center, and coordinates. The date string is hardcoded in 5 display places — grep `Mayo 5` to find them, and look for `TARGET` for the countdown variable.

## License

MIT
