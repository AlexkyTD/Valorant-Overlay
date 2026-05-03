# Releasing a new version

This is the canonical procedure for cutting a release. **No tag or release should be created without an explicit request from the project owner** — this file just describes *how* to do it once asked.

---

## Reference dimensions

Always double-check these match what's actually in `overlay.html` (`html, body { width / height }`) before announcing them anywhere:

| Asset | Native size | Notes |
| --- | --- | --- |
| `overlay.html` | **400 × 900** | Transparent background. OBS Browser Source must be set to this exact size for crisp rendering. |
| `dock.html` | responsive | Min-width 220px. Recommended starting size for OBS Custom Browser Docks: `280 × 720`. |

If you change either size, update **all** of:
1. `README.md` (install steps + tech notes + troubleshooting table)
2. `CHANGELOG.md` (the reference dimensions block at the top + the relevant entry)
3. The current GitHub release notes (via `gh release edit`)
4. Any draft release notes you're preparing for the new version

---

## Pre-release checklist

- [ ] Working tree is clean (`git status`)
- [ ] All changes for this version are committed and pushed to `master`
- [ ] `CHANGELOG.md` has a new entry under `[Unreleased]` describing the changes (move it to a versioned section in the next step)
- [ ] Overlay still loads and the dock still syncs (open both in a browser, click around)
- [ ] Reference dimensions in README + CHANGELOG match `overlay.html` actual values

---

## Cut the release

Pick a version following SemVer (see `CHANGELOG.md` header for the policy).

Replace `vX.Y.Z` below with the actual version (e.g. `v1.1.0`).

### 1. Update CHANGELOG.md

- Move everything under `[Unreleased]` into a new `[X.Y.Z] — <one-line summary>` section above it.
- Leave `[Unreleased]` empty (`_Nothing yet._`) for the next cycle.
- Commit:

```bash
git add CHANGELOG.md
git commit -m "chore: changelog for vX.Y.Z"
git push
```

### 2. Tag and push

```bash
git tag -a vX.Y.Z -m "vX.Y.Z — <one-line summary>"
git push origin vX.Y.Z
```

### 3. Build the release zip

From the repo root, on Windows (PowerShell):

```bash
powershell -NoProfile -Command "Compress-Archive -Path 'overlay.html','dock.html','README.md' -DestinationPath 'valorant-overlay-vX.Y.Z.zip' -Force"
```

On macOS / Linux:

```bash
zip -r valorant-overlay-vX.Y.Z.zip overlay.html dock.html README.md
```

> Don't include `CHANGELOG.md`, `RELEASING.md`, or any dev files in the zip — streamers only need the runnable HTMLs and the README.

### 4. Publish on GitHub

Use the GitHub CLI (`gh`):

```bash
gh release create vX.Y.Z \
  valorant-overlay-vX.Y.Z.zip overlay.html dock.html README.md \
  --title "vX.Y.Z — <short title>" \
  --notes-file release-notes-vX.Y.Z.md
```

Where `release-notes-vX.Y.Z.md` is a temporary file containing the release notes in the format below. Delete it after the release is published.

---

## Release notes template

```markdown
<one-paragraph summary of the version>

## Installation

Download **valorant-overlay-vX.Y.Z.zip** below, unzip somewhere, then add two Browser Sources in OBS:

| Source | File | Size |
| --- | --- | --- |
| **Stream overlay** (visible to viewers) | `overlay.html` | 400 × 900 |
| **Control dock** (just for you) | `dock.html` | responsive, e.g. 280 × 720 |

Full step-by-step in the [README](https://github.com/AlexkyTD/Valorant-Overlay#installation-in-obs-studio).

## What's new in this version

<bullet list of user-visible changes — pull from CHANGELOG.md>

## Disclaimer

Not affiliated with or endorsed by Riot Games. VALORANT and all related assets are trademarks of Riot Games, Inc.
```

---

## Editing an already-published release

If you need to fix a typo or update the notes of an existing release:

```bash
# Replace notes
gh release edit vX.Y.Z --notes-file new-notes.md

# Add a missing asset
gh release upload vX.Y.Z some-file.zip

# Replace an asset (delete then re-upload)
gh release delete-asset vX.Y.Z some-file.zip --yes
gh release upload vX.Y.Z some-file.zip
```

---

## Rolling back a release

Only do this if the release is broken and there's no fix-forward path.

```bash
gh release delete vX.Y.Z --yes
git push origin :refs/tags/vX.Y.Z   # remove the remote tag
git tag -d vX.Y.Z                    # remove the local tag
```

Prefer cutting a `vX.Y.Z+1` patch instead — that's almost always less disruptive for streamers who already downloaded the broken zip.
