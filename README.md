# Dalí Tarot Guide

A beautiful, offline-ready Tarot card reference app inspired by the Salvador Dalí Tarot deck from TASCHEN. Look up meanings for all 78 Tarot cards — Major Arcana and all four Minor Arcana suits — in Vietnamese.

![Theme](https://img.shields.io/badge/theme-dark%20%26%20gold-1a1a2e)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)

## Features

- **Complete 78-card deck** — Major Arcana (22 cards) + Wands, Cups, Swords, Pentacles (14 each)
- **Bilingual search** — Find cards by Vietnamese or English name
- **Detailed interpretations** — Upright & reversed meanings, plus Love, Career, and Spirituality contexts
- **Dark, elegant UI** — Gold-accented design inspired by the Dalí Tarot deck
- **Fully offline** — Works without an internet connection after first load
- **Installable PWA** — Add to your home screen on mobile
- **Zero dependencies** — Single HTML file, no build step required

## Demo

Open `index.html` in any modern browser:

```bash
# Python 3
python3 -m http.server 8000

# Node.js
npx serve .

# PHP
php -S localhost:8000
```

Then visit `http://localhost:8000`.

## Project Structure

```
.
├── index.html    # The entire application (~123 KB)
└── README.md     # This file
```

That's it — the whole app is a single self-contained HTML file with inline CSS, JavaScript, and structured data.

## Technology Stack

| Layer | Technology |
|-------|------------|
| Markup | HTML5 |
| Styling | CSS3 (inline) |
| Logic | Vanilla JavaScript ES6+ (inline) |
| Build Tool | None |
| Dependencies | None |

## Usage

1. **Browse cards** — Scroll through the grid grouped by Major Arcana and suits
2. **Search** — Type a card name in Vietnamese or English in the search bar
3. **Filter** — Click category buttons to show only Major Arcana, Wands, Cups, Swords, or Pentacles
4. **View details** — Click any card to open a modal with full interpretations
5. **Toggle orientation** — Switch between **Xuôi** (Upright) and **Ngược** (Reversed) meanings

## Deployment

Because this is a single static HTML file, you can deploy it to any static host:

- [GitHub Pages](https://pages.github.com/)
- [Netlify](https://netlify.com)
- [Vercel](https://vercel.com)
- [Cloudflare Pages](https://pages.cloudflare.com)
- Any CDN or static file server

> **Note:** Update the canonical URL in `<link rel="canonical">` and the `start_url` in the inline Web App Manifest before deploying to production.

## Browser Support

Works in all modern browsers that support:
- CSS Grid & Flexbox
- ES6+ JavaScript
- Service Workers (for offline caching)

## Credits

- Card meanings based on traditional Rider-Waite-Smith Tarot interpretations
- Visual theme inspired by the **Dalí Tarot** deck published by TASCHEN

## License

MIT License — feel free to use, modify, and distribute.
