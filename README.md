# Study Ops

A gamified study platform prototype: upload your notes, get a fully narrated,
segmented lesson with content-aware diagrams, take quizzes as you go, and
level up through a constellation-themed progress map.

**[Try the live demo →](https://praharshita77-dotcom.github.io/the-orbit/)**

Comes preloaded with two full sample lessons (H2 Physics — kinematics, and
H2 Chemistry — bonding) so you can explore everything immediately, with no
setup or API key required.

## What it does

- **Overview** — upload up to 5 study materials (PDF or pasted text) and build a day's lesson plan
- **Lesson** — each topic is broken into segments, narrated aloud (browser text-to-speech), with short on-screen highlights and a content-aware diagram per segment — a real graph, an animated displacement marker, a molecule's bond structure, a probability bar chart, and more, picked automatically based on what the segment is actually teaching
- **Timetable** — a live checklist of what's done and what's left, with points earned
- **The Orbit** — points unlock real constellations (Ursa Minor → Lyra → Cassiopeia → ... → Leo) laid out as a winding path, plus a streak tracker

Full technical notation is rendered properly with [KaTeX](https://katex.org/)
— integrals, probability notation, chemical formulas — not spelled out in
words.

## Generating new lessons

The two sample lessons work out of the box. To generate a lesson from your
**own** notes, add an Anthropic API key under **Settings** on the Overview
tab. Get one at [console.anthropic.com](https://console.anthropic.com).

The key is stored only in the browser (`localStorage`) and is sent only to
Anthropic's API when generating a lesson — never anywhere else, and never
committed to this repo.

## Tech

Single-file HTML/CSS/JS — no build step, no framework. Uses:
- Browser `SpeechSynthesis` for narration
- [PDF.js](https://mozilla.github.io/pdf.js/) for client-side PDF text extraction
- [KaTeX](https://katex.org/) for math/chemistry notation
- The Anthropic API (`claude-sonnet-5`) for lesson generation, called directly from the browser using Anthropic's documented ["bring your own key" CORS support](https://simonwillison.net/2024/Aug/23/anthropic-dangerous-direct-browser-access/)
- `localStorage` for progress persistence (materials, points, streaks)

## For developers

### Running it locally

No build step — it's a single static HTML file.

```bash
git clone https://github.com/praharshita77-dotcom/the-orbit.git
cd the-orbit
# open index.html directly in a browser, or serve it:
python3 -m http.server 8000
```

### Deploying your own copy

This is a static site with no backend, so hosting it is just a matter of
serving the files. To deploy on GitHub Pages:

1. Fork or clone this repo.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Save — GitHub publishes it at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

## Limitations

- **No shared/global leaderboard.** The Orbit tab shows local stats only —
  a real multi-user leaderboard needs a backend server to pool scores across
  visitors, which a static site doesn't have.
- **Video/audio uploads** are shown in the UI but not yet implemented — only
  PDF and pasted text work.
- Progress is local to each visitor's browser; clearing site data resets it.
