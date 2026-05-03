# VALORANT Stream Overlay

A clean, fully-themed VALORANT overlay for OBS Studio, with a compact control dock to switch agents on the fly. No build step, no backend — two HTML files that talk to each other.

![VALORANT](https://img.shields.io/badge/Game-VALORANT-FD4556?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## Features

- **28 agents** with bespoke element-themed animated SVG auras (fire, wind, poison, smoke, ice, electric, shadow, surveillance, seismic, explosive, soul, tech, nature, dimension, cosmic, suppression, luxury, lightning, nightmare, water, creature, nano, barrier, immortal, metal, missile, radiance, stealth)
- **Hex Tower carousel** showing the previous / current / next agent at a glance
- **Compact control dock** with sort (A→Z / Release / By Role), type-ahead search, prev/next navigation and keyboard shortcuts
- **Display options** that the streamer can toggle on the fly:
  - **Blur Next** — hide the upcoming agent on stream
  - **Mark Previous Played** — green ✓ on the agent just left (lobby progress)
  - **Queue Next** — amber ⏳ on the next agent
- **Per-sort progression** — switch sort mode and the overlay restores the agent you were on for *that* category
- **Persistent state** — sort, options, and per-category selections survive OBS restarts (`localStorage`)
- **Live sync** — BroadcastChannel keeps overlay and dock in sync instantly
- **Pulls real portraits** from the public [valorant-api.com](https://valorant-api.com) (no asset management)
- Fully **transparent** overlay background, **responsive** dock that fills any browser-source size

---

## Installation in OBS Studio

### 1. Download

Grab the latest release zip from the [Releases page](https://github.com/AlexkyTD/Valorant-Overlay/releases) and unzip it somewhere stable, for example:

```
C:\Users\<you>\Documents\ValorantOverlay\
  ├─ overlay.html
  └─ dock.html
```

### 2. Add the overlay (visible on stream)

In OBS:

1. **Sources** → ➕ → **Browser**
2. Name it `VALORANT Overlay` → OK
3. Tick **Local file**, browse to `overlay.html`
4. **Width** = `400`, **Height** = `900`
5. Leave the rest at defaults → OK
6. Resize and place the source on the side of your gameplay scene

> The background is fully transparent — no chroma key needed.

### 3. Add the control dock (only for you)

You have two options:

**Option A — Custom Browser Dock (recommended):**

1. **Docks** menu → **Custom Browser Docks…**
2. **Dock Name**: `VAL Control`
3. **URL**: paste the full path with `file:///` prefix, e.g.
   ```
   file:///C:/Users/<you>/Documents/ValorantOverlay/dock.html
   ```
4. Apply → close. Drag the new dock anywhere in OBS.

**Option B — Browser Source on a hidden scene:**

Same steps as the overlay, but use `dock.html`. The dock is responsive so any reasonable size works (default `280×720`).

---

## Using the dock

| Control | Action |
| --- | --- |
| **A→Z / REL / ROLE** | Switch sort mode. Each mode keeps its own progress. |
| **Search box** | Type a name, then `Enter` to jump. `↑/↓` to navigate, `Esc` to clear. |
| **Prev / Next buttons** | Step through the current sorted list. |
| **← / →** keyboard arrows | Same as prev/next (when not typing). |
| **Display Options checkboxes** | Toggle visual states on the overlay's prev/next hex chips. |

The little `LIVE` dot in the dock header lights up green once an agent is selected, so you know the overlay is showing something.

### Display Options explained

- 🟥 **Blur Next** → adds a `blur(6px)` filter to the bottom hex chip on the overlay. Useful if you don't want viewers to see what you'll pick next.
- 🟩 **Mark Previous Played** → green border + ✓ badge on the top hex chip (the agent you just left).
- 🟧 **Queue Next** → amber drop-shadow + ⏳ badge on the bottom hex chip (the upcoming pick).

All three can be on at once.

---

## How it works

- **`overlay.html`** — 400×900 transparent panel with a Hex Tower carousel, animated SVG aura per element, and bust portrait.
- **`dock.html`** — Compact preview-only control surface (current agent face + meta + nav + options).
- **Sync** — Both files communicate over a `BroadcastChannel('valorant-overlay')`. The dock pushes `SELECT_AGENT`, `SET_SORT`, and `SET_OPTIONS` messages; the overlay listens and re-renders.
- **Persistence** — Both files load/save state to `localStorage` under the key `valorant-overlay:state` so reopening OBS restores everything.

> Because OBS Browser Sources sometimes have isolated storage per URL, both files double up: each saves on every state change, and they sync at runtime via BroadcastChannel.

---

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Overlay shows nothing | Make sure the OBS Browser Source size is **400×900** and that `valorant-api.com` is reachable (it's a public CDN, no key needed). |
| Dock doesn't update overlay | Both browser sources must be loaded from the **same path** (same origin) so they share `BroadcastChannel`. Refresh both sources after moving files. |
| Settings reset each time OBS opens | OBS Browser Sources have a per-source storage profile. Check that **both** sources point to the *same* `dock.html`/`overlay.html` paths each session. |
| An agent is missing | The roster is pulled from the public Valorant API — newly added agents will appear automatically once the API ships them. |

---

## License

MIT — do whatever you want with it. A credit/star on the repo is appreciated but not required.

## Credits

Built with ♥ for Twitch streamers playing VALORANT.
Agent data + portraits from [valorant-api.com](https://valorant-api.com), an unaffiliated community API.
This project is **not affiliated with or endorsed by Riot Games**. VALORANT and all related assets are trademarks of Riot Games, Inc.
