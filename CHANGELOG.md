# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to [Semantic Versioning](https://semver.org/).

> Versioning policy: bump **MAJOR** when the OBS browser-source contract changes (size, file names, channel name); bump **MINOR** for new visible features (new aura, new dock control, new option); bump **PATCH** for fixes that don't change how a streamer interacts with it.

> Reference dimensions for OBS Browser Sources:
> - Overlay: **400 × 900** (transparent)
> - Dock: responsive, recommended `280 × 720` minimum

---

## [Unreleased]

_Nothing yet._

---

## [1.0.0] — First public release

### Added
- 28 element-themed animated SVG auras (fire, wind, poison, smoke, ice, electric, shadow, surveillance, seismic, explosive, soul, tech, nature, dimension, cosmic, suppression, luxury, lightning, nightmare, water, creature, nano, barrier, immortal, metal, missile, radiance, stealth)
- Hex Tower carousel on the overlay (prev / current / next agent), with no wrap-around at the list edges
- Compact portrait-orientation control dock (responsive, fills any browser-source size; min-width 220px)
- Sort modes: Alphabetical, Release order, By Role
- Type-ahead search with `Enter` to jump, `↑/↓` to navigate matches, `Esc` to clear
- Prev / Next nav buttons with `←` / `→` keyboard shortcuts
- Display options pushed live to the overlay:
  - Blur Next (red), Mark Previous Played (green ✓), Queue Next (amber ⏳)
- Per-sort progression — each sort category remembers its own selected agent
- `localStorage` persistence (key: `valorant-overlay:state`) so OBS restarts restore everything
- Live BroadcastChannel sync (`valorant-overlay`) between dock and overlay
- Default: first agent of the current sort category is selected on first launch
- README with full OBS install + usage docs

### Notes
- Overlay native size is **400 × 900** (transparent background, no chroma key required).
- Pure HTML/CSS/JS — no build, no backend, no dependencies. Agent data + portraits come from the public [valorant-api.com](https://valorant-api.com).
