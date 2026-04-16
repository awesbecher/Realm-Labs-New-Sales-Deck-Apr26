# Realm Labs Pitch Deck

Single-file HTML pitch deck deployed to Vercel. Any push to `main` triggers an automatic deployment.

## Structure
- `index.html` — The deck (13 slides, keyboard nav, print-to-PDF)
- `assets/` — Fonts, logos, headshots, diagrams, investor logos
- `vercel.json` — Cache headers for static assets

## Local preview
Open `index.html` directly in Chrome, or run a local server:
```
python3 -m http.server 8000
```
then visit http://localhost:8000

## Controls
- Arrow keys / spacebar / click — navigate slides
- `P` or the top-right button — save as PDF
