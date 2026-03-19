# 🏀 Techne × AI — 2026 March Madness Bracket Challenge

Live leaderboard tracking 7 Techne team members vs. Claude, ChatGPT, and Gemini across the full NCAA Tournament.

**Live site:** `https://spencer-techne.github.io/march-madness-2026`

---

## Quick Links

| Tab | What it does |
|---|---|
| **Leaderboard** | Live standings — points, round breakdown, ranked |
| **Bracket** | Any participant's full bracket, color-coded as results come in |
| **Enter Picks** | Step-by-step wizard for team members to submit their bracket |
| **Admin ⚙** | Enter game results, manage participant names (PIN protected) |

---

## Setup (One Time)

### 1. Create the GitHub repo

```bash
# In the spencer-techne org, create a new repo called:
march-madness-2026

# Clone it locally
git clone https://github.com/spencer-techne/march-madness-2026.git
cd march-madness-2026
```

### 2. Add the files

Drop `index.html` and `data.json` into the repo root.

```bash
git add .
git commit -m "Initial deploy"
git push origin main
```

### 3. Enable GitHub Pages

- Go to repo → **Settings → Pages**
- Source: **Deploy from branch** → `main` → `/ (root)`
- Save. Live in ~60 seconds at `https://spencer-techne.github.io/march-madness-2026`

### 4. Create an Admin PAT (Personal Access Token)

You need this for the Admin panel to commit results directly from the browser.

1. Go to [github.com/settings/tokens/new](https://github.com/settings/tokens/new)  
   *(Or: Settings → Developer settings → Personal access tokens → Fine-grained tokens)*
2. Name it something like `march-madness-admin`
3. Set expiration to **90 days** (covers the full tournament)
4. Under **Repository access** → select `march-madness-2026`
5. Under **Permissions** → **Contents** → set to **Read and write**
6. Click **Generate token** — copy it immediately, you won't see it again

Keep this token private. It's stored in your browser's localStorage only.

### 5. Configure the Admin panel

1. Open the live site → click **Admin ⚙**
2. Enter PIN: **`2026`** *(you can change this in `index.html` line ~1 of the admin JS)*
3. Fill in:
   - **Owner:** `spencer-techne`
   - **Repo:** `march-madness-2026`
   - **Branch:** `main`
   - **Token:** your PAT from step 4
4. Click **Save & Test** — status dot should go green

---

## Collecting Team Picks

Each team member uses the **Enter Picks** tab:

1. They go to the live site, click **Enter Picks**
2. Walk through the bracket step by step (~5 min)
3. At the end, they get a JSON block — they copy it and send to Spencer (Slack, email, whatever)
4. Spencer goes to Admin → (in a future update) paste import, or manually updates `data.json`

### Manually adding picks to data.json

Find the right `humanX` slot in `participants[]` and replace the name + picks:

```json
{
  "id": "human1",
  "name": "RJ",
  "type": "human",
  "color": "#a78bfa",
  "submitted": true,
  "picks": {
    "FF1": "Texas",
    "FF2": "Howard",
    ...
  }
}
```

Then push to GitHub — the live leaderboard updates within seconds.

---

## Logging Game Results (Every Round)

**Preferred method — Admin panel (no code required):**

1. Open the live site → **Admin ⚙** → enter PIN
2. Find the game in the round list
3. Click the winning team — it highlights green
4. Repeat for all finished games
5. Hit **Push Results** in the sticky bar at the bottom
6. That's it — scores update live for everyone

**Manual method — edit data.json directly:**

```json
"results": {
  "FF1": "Texas",
  "FF2": "Howard",
  "E1": "Duke",
  "E2": "TCU"
}
```

The key is the game ID, the value is the exact team name string. Push and done.

### Game ID reference

| Region | Round 1 | R32 | S16 | E8 |
|---|---|---|---|---|
| East | E1–E8 | E9–E12 | E13–E14 | E15 |
| West | W1–W8 | W9–W12 | W13–W14 | W15 |
| Midwest | M1–M8 | M9–M12 | M13–M14 | M15 |
| South | S1–S8 | S9–S12 | S13–S14 | S15 |
| First Four | FF1–FF4 | — | — | — |
| Final Four | — | — | — | FF5, FF6 |
| Championship | NC | — | — | — |

---

## Scoring

| Round | Points |
|---|---|
| Round of 64 | 10 |
| Round of 32 | 20 |
| Sweet 16 | 40 |
| Elite Eight | 80 |
| Final Four | 160 |
| National Championship | 320 |

Max possible score: **1,920 points**

---

## Adding ChatGPT / Gemini Picks

Same process as human picks — prompt each AI to fill out the bracket, collect their picks as JSON, and update `data.json`. Their slots are already reserved with IDs `chatgpt` and `gemini`.

---

## Changing the Admin PIN

In `index.html`, find this line near the top of the admin JS section:

```js
const ADMIN_PIN = '2026';
```

Change to whatever 4-digit PIN you want. Push the change and it's live.

---

## Repo Structure

```
march-madness-2026/
├── index.html     # Full app — leaderboard, bracket, entry wizard, admin panel
└── data.json      # All bracket picks + results — only file you'll ever edit
```

Everything is in those two files. No build step, no dependencies, no backend.

---

## Content / LinkedIn Series

Suggested post cadence:

| When | Content |
|---|---|
| **Now** | "We gave 3 AIs our bracket challenge. Here's how they picked it." |
| **After R64** | "Round 1 results: here's where the AIs went wrong (or right)" |
| **After Sweet 16** | Mid-tournament update with leaderboard screenshot |
| **After Final Four** | "One AI is still alive. Our team is not." |
| **After Championship** | Final results + winner callout post |

Attach a screenshot of the leaderboard to each post. Link to the live site.
