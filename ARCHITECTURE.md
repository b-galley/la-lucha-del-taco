# La Lucha del Taco — Architecture & Edit Map

> Navigation map for `index.html`. Designed for targeted edits without rewriting the file.
>
> **How to use this doc:** When you (or Claude) want to change something, look it up in [§1 Quick Reference](#1-quick-reference--i-want-to-change-x) first. Line numbers are accurate as of the most recent verification (3,673 lines total). If they drift after edits, use the **search anchor** strings — they're unique within the file and won't drift.

---

## 1. Quick Reference — "I want to change X"

| I want to change… | File location | Line(s) | Search anchor |
|---|---|---|---|
| Page title (browser tab text + date) | `<head>` `<title>` | 6 | `<title>La Lucha del Taco` |
| Color palette (red/gold/black/cream/green) | `<style>` :root vars | 11–23 | `--crimson: #C8102E` |
| Fonts (Impact / Georgia stacks) | `<style>` :root vars | 21–22 | `--font-display:` |
| Crawl date/time (countdown target) | `<script>` let TARGET | 2764 | `let TARGET = new Date(` |
| Splash credentials line | `<body>` splash-credentials | 1738 | `class="splash-credentials"` |
| Sticky header date label | `<body>` sticky-date | 1787 | `class="sticky-date"` |
| The 7 scoring categories | `<script>` CATEGORIES array | 1963–1971 | `const CATEGORIES = [` |
| Default starting scores | `<script>` DEFAULT_SCORES | 1973 | `const DEFAULT_SCORES =` |
| Pre-populated taco stops (5 of them) | `<script>` INITIAL_STOPS array | 1975–2010 | `const INITIAL_STOPS = [` |
| Badge definitions (14 badges) | `<script>` BADGES array | 2013–2030 | `const BADGES = [` |
| Map default center / zoom | `<script>` initMap | 3100 | `map = L.map('map'` |
| Confetti colors / blast pattern | `<script>` fireConfetti | 3620–3639 | `function fireConfetti()` |
| Copy-results clipboard format | `<script>` copyResultsBtn click | 3605–3638 | `'copyResultsBtn'` |
| "El Veredicto" notes label | `<script>` renderStops template | ~3210 | `★ El Veredicto ★` |
| Firebase project config | `<script>` firebaseConfig | 1948–1958 | `const firebaseConfig = {` |
| Room code generation | `<script>` generateRoomCode | 2756 | `function generateRoomCode()` |
| Add Stop modal form fields | `<body>` add-stop-form | 1851–1884 | `id="addStopModal"` |
| Challenge modal | `<body>` challengeModal | 1886–1912 | `id="challengeModal"` |
| Results modal layout | `<body>` resultsModal | 1913–1929 | `id="resultsModal"` |
| Time adjuster overlay | `<body>` timeAdjustOverlay | 1930–1941 | `id="timeAdjustOverlay"` |

---

## 2. File Anatomy (Line Ranges)

Single self-contained HTML file. Four top-level sections:

```
1–9        <head> + CDN imports (Leaflet, canvas-confetti)
10–1725    <style> ───────────────── all CSS, ~1,715 lines
1727–1943  <body> ────────────────── HTML structure + Firebase SDK scripts
1944–3672  <script> ──────────────── all vanilla JS, ~1,728 lines
3673       closing tags
```

> ⚠️ Firebase SDK scripts (`firebase-app-compat.js` + `firebase-database-compat.js`) are loaded at the top of `<body>` (lines 1730–1731) rather than in `<head>`, so they're available before the main `<script>` block runs.

### CSS sections (inside `<style>`)

| Section | Lines | What's there |
|---|---|---|
| `:root` vars + base reset | 11–53 | Color palette, font stacks, body styles, stripe pattern |
| Splash | 54–224 | Full-screen entry overlay, countdown cells, lobby fields, Enter button |
| Main App / Sticky Header | 225–295 | App shell visibility, sticky top bar, room info bar |
| Layout grid | 296–314 | `.main-grid` desktop 2-col / mobile 1-col |
| Map | 315–363 | Map container, Leaflet popup overrides |
| Stops | 364–546 | Stop card, header, number badge, score pill, expand/collapse, leader highlight |
| Sliders | 547–639 | Score row layout, custom taco SVG thumb, margarita green accent |
| Notes / Veredicto | 640–664 | Textarea styling |
| Actions / Buttons | 665–730 | `.btn` variants, add-stop, reveal-btn, challenge bar |
| Leaderboard | 731–822 | Sidebar/sticky behavior, rank-1 gold treatment, mobile collapse toggle, per-user breakdown rows |
| Modal (shared overlay) | 823–968 | Add Stop, Challenge, Results, and Time Adjuster modals |
| Add Stop form | 969–994 | Form-specific input/label/coord-row styles |
| Time Adjuster | 995–1040 | Creator-only overlay for adjusting crawl start time |
| Toast | 1041–1060 | Bottom-center transient message |
| Lobby | 1061–1191 | Splash lobby: name input, room code char inputs, error message |
| Submit button | 1192–1229 | Per-stop submit/update button states |
| Avatar chips | 1230–1338 | Circular luchador avatar chips + avatar row layout |
| Connection status | 1339–1351 | Online/offline dot indicator |
| Badges | 1352–1467 | Badge gallery grid, badge cards, badge popup overlay |
| La Ruleta (Spinner) | 1468–1492 | Spin wheel container, pointer, result card, history list |
| Challenge bar | 1493–1724 | Inline challenge bar below stops list + challenge modal list items |

### HTML sections (inside `<body>`)

| Block | Lines | ID(s) / Notes |
|---|---|---|
| Firebase SDK scripts | 1730–1731 | Compat versions loaded before `<script>` block |
| Splash screen + lobby | 1733–1779 | `#splash`, `#playerName`, `.room-char` (×4), `#joinRoomBtn`, `#createRoomLink`, `#lobbyError`, `#cd-h/m/s` countdown |
| Sticky header | 1781–1800 | `#stickyLeaderName`, `#stickyLeaderScore`, `#roomInfoBar`, `#connDot`, `#roomCodeDisplay`, `#scoringAs`, `#roomRoster`, `#userCountDisplay` |
| Map section | 1804–1807 | `#map` |
| Stops section + actions | 1809–1832 | `#stopsList` (rendered dynamically), `#addStopBtn`, `#challengeBar`, `#challengeBarCount`, `#openChallengeModalBtn`, `#revealBtn` |
| Leaderboard sidebar | 1833–1837 | `#leaderboard`, `#leaderboardList`, `#leaderboardToggle` |
| Badge gallery | 1834–1849 | `#badgeGallery`, `#badgeGrid`, `#badgePopupOverlay`, `#badgePopup` (with `#badgePopupEmoji/Title/Desc/Earners`) |
| Add Stop modal | 1851–1884 | `#addStopModal`, fields: `#newStopName/Desc/Addr/Lat/Lng` — doubles as Edit Stop modal |
| Challenge modal | 1886–1912 | `#challengeModal`, `#challengeListWrap`, `#challengeList`, `#newChallengeEmoji/Title/Text`, `#confirmAddChallenge` |
| Results modal | 1913–1929 | `#resultsModal`, `#modalChampName/Score`, `#modalBody`, `#copyResultsBtn` |
| Time adjuster overlay | 1930–1941 | `#timeAdjustOverlay`, `#timeAdjustInput` (datetime-local), `#timeAdjustSave/Cancel` |
| Toast | 1943 | `#toast` |

### JS sections (inside `<script>`)

| Section | Lines | Purpose |
|---|---|---|
| FIREBASE CONFIG | 1947–1959 | `firebaseConfig` object + `firebase.initializeApp()` + `const db` |
| CONFIG | 1961–2011 | `CATEGORIES`, `DEFAULT_SCORES`, `INITIAL_STOPS` — **edit these to change content** |
| BADGE CONFIG | 2012–2031 | `BADGES` array — 14 badge definitions with id, emoji, title, desc |
| SPINNER CONFIG | 2032–2036 | `allChallenges` array — populated from Firebase, not hardcoded |
| STATE | 2037–2053 | All mutable state vars: `stops`, `allRatings`, `myLocalScores`, `mySubmittedStops`, `allUsers`, `currentBadges`, `spinHistory`, `map`, `mapMarkers`, `roomCode`, `userName`, `roomRef`, etc. |
| UTILS | 2054–2099 | `escapeHtml`, `getUserComposite`, `getAggregatedScores`, `getRanked`, `getLeaderId`, `getMyComposite`, `nameToColor`, `avatarChip`, `avatarRow` |
| BADGE COMPUTATION | 2100–2417 | `computeBadges()` — evaluates all 14 badge conditions across all users; `getBadgeEmojisForStop()` |
| BADGE GALLERY RENDER | 2418–2471 | `renderBadgeGallery()` — builds badge grid + wires popup overlay |
| LA RULETA (SPINNER) | 2472–2642 | `renderSpinner()`, `spinWheel()` — SVG pie chart wheel with CSS spin animation |
| CHALLENGE MANAGEMENT | 2643–2762 | `renderChallengeList()` — renders/wires challenge add form and delete buttons; writes to Firebase |
| COUNTDOWN | 2763–2790 | `let TARGET` date constant + ticker interval |
| TIME ADJUSTER | 2791–2840 | Triple-tap handler on sticky-date (creator-only) → datetime picker → writes `startTime` to Firebase |
| ROOM CODE INPUT UX | 2841–2864 | Auto-advance focus between 4 char inputs, backspace handling |
| LOBBY: CREATE ROOM | 2865–2896 | Generates room code, writes initial stops + user to Firebase, calls `enterRoom()` |
| LOBBY: JOIN ROOM | 2897–2928 | Validates room exists, registers user in Firebase, calls `enterRoom()` |
| ENTER ROOM | 2929–2965 | Hides splash, shows app shell, sets up onDisconnect, listens for `startTime`, calls `initMap()` + `attachFirebaseListeners()` after 60ms |
| FIREBASE LISTENERS | 2966–3077 | `attachFirebaseListeners()` — sets up `value` listeners on stops, ratings, users, challenges, and `.info/connected` |
| MAP | 3078–3122 | `makeNumberedIcon()`, `initMap()`, `refreshMarkers()` |
| RENDER | 3123–3330 | `render()`, `renderStops()`, `renderLeaderboard()`, `updateStickyLeader()`, `updateLeaderHighlight()`, `updateAllSubmitButtons()` |
| EVENT WIRING | 3331–3495 | `attachStopEvents()` — sliders, photo upload, notes, prev/next, edit, remove, submit, ruleta toggle |
| ADD STOP | 3496–3543 | Open modal (new or edit), validate, write to Firebase |
| RESULTS / REVEAL | 3544–3640 | `revealChampion()`, `fireConfetti()`, copy-to-clipboard |
| MOBILE LEADERBOARD TOGGLE | 3641–3650 | Collapse/expand below 920px |
| CLICK OUTSIDE MODAL | 3651–3672 | Click-outside + Escape key for all three modals |

---

## 3. State Model

### Firebase state (synced across all users in a room)

| Path | Shape | Notes |
|---|---|---|
| `rooms/{code}/stops/{id}` | `{ name, desc, address, lat, lng }` | Written by room creator; editable by anyone |
| `rooms/{code}/ratings/{userName}/{stopId}` | `{ tortilla, relleno, salsa, precio, vibra, comodin, margarita }` | Written on "Submit My Ratings" |
| `rooms/{code}/users/{userName}` | `{ joinedAt, online }` | `online` set to false via `onDisconnect()` |
| `rooms/{code}/challenges/{id}` | `{ emoji, title, text }` | Shared challenge pool for the spinner |
| `rooms/{code}/startTime` | timestamp (ms) | Set by room creator via time adjuster; syncs countdown for all |
| `rooms/{code}/createdBy` | string | Used to identify the room creator for privileged actions |

### Local/in-memory state (JS vars, reset on refresh)

| Variable | Purpose |
|---|---|
| `stops` | Local array mirroring Firebase stops; holds `expanded`, `photo`, `notes`, `scores` (local slider state) |
| `myLocalScores` | `{ stopId: { cat: val } }` — slider positions before submit |
| `mySubmittedStops` | `{ stopId: true }` — tracks which stops this user has submitted |
| `allRatings` | Mirror of `rooms/{code}/ratings` — updated by Firebase listener |
| `allUsers` | Mirror of `rooms/{code}/users` — updated by Firebase listener |
| `currentBadges` | `{ userName: [badgeId, ...] }` — recomputed by `computeBadges()` on every ratings change |
| `spinHistory` | `{ stopId: [{ name, title, emoji }, ...] }` — ephemeral, local only |
| `map`, `mapMarkers` | Leaflet instance and marker registry |
| `roomCode`, `userName`, `roomRef` | Set on room join; drive all Firebase operations |
| `isRoomCreator` | Boolean; gates time adjuster access |
| `editingStopId` | Tracks which stop is being edited in the Add Stop modal (`null` = adding new) |

### `CATEGORIES` array — line 1963
The 7 scoring categories, in display order. Each:
```js
{ key: 'tortilla', label: 'La Tortilla', sublabel: '...' }
```
Adding/removing here **automatically** updates: every stop card's score grid, the composite calculation, the results modal detail line, and the copy-results clipboard text. **Do not change `key` values without also updating `DEFAULT_SCORES`.**

### `DEFAULT_SCORES` object — line 1973
Starting value for every category on a new stop. Currently all 7s except `precio: 6`. **Must contain a key for every `CATEGORIES[].key`.**

### `INITIAL_STOPS` array — lines 1975–2010
The 5 pre-populated taco stops written to Firebase when a room is created. Shape:
```js
{
  id: 's1',
  name: 'Puesto Headquarters',
  desc: '...',
  address: '789 W Harbor Dr',
  lat: 32.7100, lng: -117.1696
}
```
Note: `photo`, `notes`, `scores`, and `expanded` are **not** stored in Firebase — they live in local `stops` state only.

---

## 4. Theme Tokens (CSS Variables)

All colors and fonts route through `:root` at lines 11–23. Change once, propagates everywhere.

```
--crimson       #C8102E   primary red (lucha + Mexican flag)
--crimson-dark  #8B0A1F   shadow / gradient stop
--gold          #FFB81C   championship gold
--gold-dark     #C8920F   gradient stop
--jet           #0F0F0F   off-black (body bg, headers)
--bone          #FFF5E1   warm cream (text on dark)
--verde         #006847   Mexican flag green (margarita accent)
--paper         #F4E8C8   stop card background

--font-display  Impact / Haettenschweiler / Arial Narrow Bold / Bebas Neue
--font-body     Georgia / Times New Roman
```

The margarita category gets `--verde` accents specifically in the sliders section.

---

## 5. Hardcoded Values (things you'd change for a different crawl)

| Value | Where | Line |
|---|---|---|
| Page title (`<title>` tag, includes date) | `<head>` | 6 |
| Crawl kickoff datetime (countdown target) | `let TARGET` | 2764 |
| Map default center coords | `setView([32.7115, -117.1593], 15)` | 3100 |
| Default new-stop coords (Gaslamp center + jitter) | `confirmAddStop` lat/lng fallback | ~3530 |
| 5 pre-populated stop coords | `INITIAL_STOPS[].lat/.lng` | 1975–2010 |
| Splash credentials date+location | `splash-credentials` div | 1738 |
| Sticky header date | `sticky-date` div | 1787 |
| Date stamp on results modal | `modal-stamp` div | 1920 |
| Date stamp in copy-results text | `lines.push('Mayo 5, 2026...')` | 3611 |
| Firebase project credentials | `firebaseConfig` object | 1948–1958 |

> ⚠️ **The date string is hardcoded in 5 display places** (lines 6, 1738, 1787, 1920, 3611) plus the `TARGET` constant on line 2764. To find them all: grep `Mayo 5` for display strings, and `let TARGET` for the countdown. There is no central date constant.

---

## 6. External Dependencies

Loaded in `<head>` (lines 7–9) and top of `<body>` (lines 1730–1731):

| Library | Version | CDN | Used for | Global |
|---|---|---|---|---|
| Leaflet CSS | 1.9.4 | cdnjs | Map tile styles | — |
| Leaflet JS | 1.9.4 | cdnjs | Interactive map + numbered markers | `L` |
| canvas-confetti | 1.9.3 | cdnjs | Reveal celebration burst | `confetti` |
| firebase-app-compat | 10.12.2 | gstatic | Firebase SDK core | `firebase` |
| firebase-database-compat | 10.12.2 | gstatic | Realtime Database | `firebase.database()` |

Tile layer is OpenStreetMap (line 3101) — no API key needed.

---

## 7. Key Functions Cheat Sheet

| Function | Line | What it does |
|---|---|---|
| `getUserComposite(scores)` | 2059 | Returns the average of all 7 category scores for a single user's score object |
| `getAggregatedScores()` | 2064 | Returns stops array with `composite` (avg across all raters) and `raterCount` added |
| `getRanked()` | 2077 | Returns stops sorted by aggregate composite, descending — drives leaderboard |
| `getLeaderId()` | 2082 | Returns the leading stop's id (or null if no ratings yet) |
| `computeBadges()` | 2100 | Evaluates all 14 badge conditions; writes to `currentBadges` |
| `renderBadgeGallery()` | 2418 | Rebuilds badge grid and wires popup overlay |
| `renderSpinner(stopId)` | 2472 | Builds SVG pie wheel for a stop's Ruleta section |
| `spinWheel(stopId)` | 2548 | Animates spin, picks winner + challenge, updates history |
| `attachFirebaseListeners()` | 2966 | Wires all Firebase `value` listeners (stops, ratings, users, challenges, connection) |
| `enterRoom()` | 2929 | Transitions from lobby to app; sets up Firebase refs + onDisconnect |
| `render()` | 3123 | Full re-render: stops + leaderboard + map markers + sticky leader |
| `renderStops()` | 3131 | Rebuilds the stop card list HTML, then calls `attachStopEvents()` |
| `renderLeaderboard()` | 3253 | Rebuilds sidebar list with per-user breakdowns |
| `updateAllSubmitButtons()` | 3307 | Syncs submit button states + submission avatar chips without full re-render |
| `attachStopEvents()` | 3331 | **Wires every interactive element on every card.** Re-runs after every `renderStops()`. |
| `revealChampion()` | 3556 | Fills results modal with ranked stops + per-category averages, opens it, fires confetti |
| `fireConfetti()` | 3620 | 6-burst sequence alternating lucha + Mexican flag colors |
| `initMap()` | 3090 | One-time Leaflet init after room is entered |
| `refreshMarkers()` | 3108 | Tears down all markers and rebuilds — called inside `render()` |

---

## 8. Behavior / Event Flow

**Lobby → Room transition** (line 2929):
- Create room: generate code → write stops + user to Firebase → `enterRoom()`
- Join room: validate code → write user to Firebase → `enterRoom()`
- `enterRoom()`: hide splash, show `#appShell`, set onDisconnect, listen for `startTime`, call `initMap()` + `attachFirebaseListeners()` after 60ms

**Slider drag** (line ~3380 — most performance-critical path):
- `input` event → update `myLocalScores[stopId][cat]` → update single value box + composite pill **without re-rendering the whole card** → re-render leaderboard + sticky leader + leader-card highlight.

**Submit ratings** (line ~3410):
- Tap "Submit My Ratings" → write `myLocalScores[stopId]` to `rooms/{code}/ratings/{userName}/{stopId}` in Firebase → Firebase listener fires → `allRatings` updates → `computeBadges()` → `renderLeaderboard()` + `renderBadgeGallery()` + `updateAllSubmitButtons()`.

**Add / Edit stop** (line 3496):
- Add: write new stop to `rooms/{code}/stops/{newId}` → Firebase listener fires → all clients re-render
- Edit: update existing stop fields in Firebase → same flow

**Remove stop** (line ~3470):
- `confirm()` dialog → `roomRef.child('stops/' + id).remove()` → Firebase listener fires → all clients re-render

**Spin La Ruleta** (line 2548):
- Pick random user + random challenge → CSS `transform: rotate()` animation (3s cubic-bezier) → on `transitionend`, show result card + update local `spinHistory`

**Reveal champion** (line 3556):
- Build modal body from `getRanked()` with per-category averages across all raters → activate overlay → fire 6-burst confetti

**Time adjuster** (line 2791, creator-only):
- Triple-tap on `.sticky-date` → open datetime-local picker → on save, write `startTime` to Firebase → all clients' `TARGET` is updated via Firebase listener

**Map markers**: rebuilt on every `render()` — never edited in place.

---

## 9. Editing Gotchas

1. **Don't break the `CATEGORIES` ↔ `DEFAULT_SCORES` contract.** Every stop's scores object must have a key for every `CATEGORIES[].key`. If you add a category, update both.

2. **`INITIAL_STOPS` only affects room creation.** Editing `INITIAL_STOPS` won't change an already-created Firebase room — the stops are written once to `rooms/{code}/stops` and live there. To change stops mid-crawl, use the in-app Add/Edit Stop flow.

3. **Date is hardcoded in 5 display places + 1 logic place** (see §5). Grep `Mayo 5` to find the display strings; `let TARGET` for the countdown.

4. **Slider drag intentionally avoids full re-render.** If you add new derived UI that depends on scores, update it in the slider `input` handler in `attachStopEvents()`, not just in `renderStops()`.

5. **Leaflet needs the map container visible before `initMap()`.** That's why there's a 60ms `setTimeout` in `enterRoom()` (line ~2960). Don't remove it.

6. **Photos and notes are local-only.** They are never written to Firebase. Refreshing the page or rejoining loses them. If you want persistence, add Firebase Storage for photos and a `rooms/{code}/notes/{userName}/{stopId}` path for notes.

7. **`'s' + Date.now()` id pattern** (in add stop flow, ~line 3535) could collide if a user spam-clicks Add Stop within the same millisecond. Unlikely in practice.

8. **`computeBadges()` runs on every ratings change.** It's O(users × stops × badges). Fine for a crawl-sized dataset (5–10 users, 5–8 stops), but don't add expensive logic here.

9. **Mobile viewport meta blocks zoom** (line 5: `maximum-scale=1.0, user-scalable=no`). Intentional — the UI is mobile-tuned and pinch-zoom conflicts with the sticky header. Remove if you need accessibility-zoom support.

10. **Room codes are 4 uppercase alphanumeric characters** generated by `generateRoomCode()` (line 2756). There's no server-side collision prevention beyond a single re-roll on create — sufficient for single-crawl use.

---

## 10. File Inventory

```
lucha-del-taco/
├── index.html        ← The app. Single self-contained file. (~3,673 lines)
├── ARCHITECTURE.md   ← This doc.
├── README.md         ← Project overview and setup guide.
└── emojione_taco.svg ← Source SVG for the taco slider thumb (inlined as base64 in CSS).
```

**Last verified line count:** 3,673 total. CSS ~1,715. HTML ~216. JS ~1,728.
**Last verified:** May 3, 2026.
