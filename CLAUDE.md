# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file static wedding invitation website for **Arya & Silvi** (Indonesian: *Undangan Pernikahan*). Wedding date: **10 Mei 2026**. The entire frontend is in `undangan_pernikahan.html` (~2000+ lines) and is served as a static file — no build step, no framework, no package manager.

## Running / Previewing

Open `undangan_pernikahan.html` directly in a browser. No server required for most features. To test RSVP/ucapan (Google Sheets integration), a live server is needed to avoid CORS issues:

```bash
python3 -m http.server 8080
# then open http://localhost:8080/undangan_pernikahan.html
```

## Architecture

Everything lives in `undangan_pernikahan.html`:

- **CSS** (`<style>` in `<head>`): ~1150 lines. Uses CSS custom properties (`--gold`, `--brown`, `--cream`, etc.) defined in `:root`. Mobile-first, max-width 430px container (`#app`).
- **HTML sections** (in order):
  - `#cover` — landing/cover page with open button
  - `#welcome` — curtain animation reveal with couple names & date
  - couple section — bride & groom profiles with photos
  - `#akad` — ceremony details + countdown timer
  - location/maps — embedded Google Maps iframe
  - `#rsvp` — attendance form
  - ucapan (wishes) — guestbook loaded from Google Sheets
  - wedding gift — bank transfer details (BCA & Mandiri)
  - footer
- **JavaScript** (inline `<script>` at end of `<body>`): countdown timer, RSVP form submission, wishes display, music toggle, flower rain animation.

## Google Apps Script Integration

RSVP and ucapan (wishes) are stored in Google Sheets via a deployed Apps Script web app. The endpoint URL is hardcoded in the JS:

```js
const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbyDtuCOPDmgwBsADcg9JwFmHrhKPNts4THopWCj4oPl3ZE6xQbqDswUsCa31MeXgwqD/exec';
```

The script URL and deployment ID are also recorded in `app-script.md`. Form submissions use a hidden `<iframe>` target to avoid page reload. Ucapan data is fetched via `GET ?type=ucapan`.

## Assets

All in `assets/`:
- `background.png` — repeating vertical background texture
- `arya.png`, `silvi.png`, `revisi-foto-pria.png`, `revisi-foto-perempuan.png` — couple photos
- `bca.jpeg`, `mandiri.jpeg` (and duplicate named versions) — bank logos
- `Denny caknan feat Bella bonita  SINARENGAN  MV TEASER.mp3` — background music (autoplay blocked until user interaction)

## Design System

| Variable | Value | Usage |
|---|---|---|
| `--gold` | `#C9A84C` | Primary accent |
| `--gold-light` | `#E8C97A` | Hover states |
| `--gold-dark` | `#8B6914` | Text on light bg |
| `--brown` | `#1A3528` | Dark green/brown (primary dark) |
| `--cream` | `#FDF6E3` | Card backgrounds |

Fonts: **Playfair Display** (headings), **Cormorant Garamond** (body), **Noto Serif Javanese** (Javanese script ornaments). Icons: Font Awesome 6.5.

## Key Customization Points

- Couple names, titles, parent names: search for `Arya`, `Silvi`, `Muhammad Agus` etc. in the HTML
- Wedding date/time: `new Date('2026-05-10T10:00:00')` in JS, plus text mentions in akad section
- WhatsApp number for ucapan: `https://wa.me/6285156952779` in `sendWA()`
- Google Maps embed: `<iframe>` src in the location section
- Music file: `<audio id="bgMusic" src="assets/...mp3">`
