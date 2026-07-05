# Starry Fiddler 🌌

A calm, non-competitive space "fidget" for children ages 3–5. A soothing dark sky
where celestial objects drift by, and every key press produces a gentle, delightful
reaction — while grown-ups keep the computer safe.

Built from the [design document](./Design%20Doc.dc.html) as a **single self-contained
web app**: no build step, no framework, no network calls, no data collection.

## What it is

- **Every press is a "yes."** Any letter/number key does something pleasant. Matching an
  object's label triggers that object's signature reaction; any other key floats a big soft
  letter up with a chime.
- **Click / tap works too** — touch and mouse friendly.
- **Two worlds** — Space and Under-the-sea. A random one greets you each visit, or pick
  one in the grown-up panel.
- **Space** — rockets, asteroids, alien ships, an alien train, comets, satellites, astronauts,
  the moon (with phases), 8 planets (spoken aloud), constellations, and ambient shooting stars.
- **Under the sea** — sharks, clownfish, blackfish, seahorses, slithering sea snakes, sea lions,
  penguins, crabs scuttling along the sand, seashells that open to reveal pearls, and rising
  bubbles. The brightness slider becomes day & night underwater.
- **Gentle milestones** — every ~30 presses a supernova celebration blooms with the child's name.
- **Grown-up panel** — type `PARENT` on the keyboard to open settings (sound, sky busyness,
  brightness, visual style, child's name, favorite planet, fullscreen). Persists to `localStorage`.
- **Synthesized audio** (Web Audio API) — no sound files. Planet names use the browser's speech synthesis.

## Project layout

```
index.html          ← the entire app (this is what gets deployed)
vercel.json         ← static hosting config + cache/security headers
.vercelignore       ← keeps the design-tool sources out of the deployment
Design Doc.dc.html  ← design document (not deployed)
Starry Fiddler.dc.html, Style Options.dc.html, support.js  ← original design-tool prototype (not deployed)
```

The app is a static site — `index.html` is served at the domain root. There is no build
and no server; the `.dc.html` files are the design-tool prototype and are excluded from
deploys via `.vercelignore`.

## Deploy to Vercel

**Option A — Vercel CLI**

```bash
npm i -g vercel      # once
vercel               # preview deploy (follow prompts, accept the static defaults)
vercel --prod        # promote to your production domain
```

No framework preset is needed — choose **Other** / static when asked. Output/root
directory is the project root; there is no build command.

**Option B — Git import (recommended for a real domain)**

1. Push this folder to a GitHub/GitLab/Bitbucket repo.
2. In the Vercel dashboard: **Add New… → Project → Import** the repo.
3. Framework preset: **Other**. Build command: *(none)*. Output directory: *(root)*.
4. Deploy, then add your custom domain under **Project → Settings → Domains**.

**Option C — drag & drop**

Drag the project folder onto the Vercel dashboard's "deploy" area for an instant static deploy.

## Local preview

Because the app calls `localStorage`, `speechSynthesis`, and fullscreen APIs, preview it
over `http://` rather than `file://`:

```bash
python3 -m http.server 8000
# then open http://localhost:8000/
```

## Notes

- **Fonts** (Fredoka / Nunito) load from Google Fonts with a system-font fallback, so the
  app still works if that request is blocked. To make it fully offline/self-hosted, download
  the two font families into the project and swap the `<link>` in `index.html` for local `@font-face` rules.
- **Kiosk safety:** a web page cannot block OS shortcuts (Cmd/Win key, Alt-Tab). For a true
  child-proof lockdown, wrap the deployed URL in a kiosk shell (Electron/Tauri, iPad Guided
  Access, or Android screen pinning). Fullscreen (in the grown-up panel) reduces accidental exits.
- **Privacy:** no accounts, no analytics, no network calls beyond loading the web fonts. All
  state stays in the browser's `localStorage`.
