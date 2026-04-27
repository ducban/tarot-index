# B3's Tarot Index

A beautiful, offline-ready Tarot card reference app with a dark, gold-accented UI. Look up meanings for all 78 Tarot cards — Major Arcana and all four Minor Arcana suits — with content in Vietnamese.

**Live:** [tarrot.hiem.co](https://tarrot.hiem.co)

![Theme](https://img.shields.io/badge/theme-dark%20%26%20gold-1a1a2e)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)

## Features

- **Complete 78-card deck** — Major Arcana (22) + Wands, Cups, Swords, Pentacles (14 each)
- **Bilingual search** — Find cards by Vietnamese or English name
- **Detailed interpretations** — Upright & reversed meanings for general, love, career, and spirituality
- **Yes / No / Maybe** — Quick answer badge for each card (upright and reversed)
- **Card Combinations** — Select two cards and see their combined meaning
- **Astrology correspondences** — Zodiac, planet, and element for each card (collapsible)
- **Card flip animation** — 3D flip effect on the card grid
- **Intro guide** — Built-in modal explaining Tarot basics and how to use the app
- **Dark, elegant UI** — Gold-accented design inspired by the Dalí Tarot deck
- **Fully offline** — Works without an internet connection after first load
- **Installable PWA** — Add to your home screen on mobile
- **Zero dependencies** — Single HTML file, no build step required

## Getting Started

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
├── index.html              # The entire application (~192 KB)
├── favicon.svg             # SVG favicon (tarot card + star icon)
├── favicon.png             # 32×32 PNG favicon
├── favicon.ico             # ICO favicon
├── apple-touch-icon.png    # 180×180 Apple Touch Icon
├── og-image.png            # 1200×630 Open Graph image
├── AGENTS.md               # AI agent documentation
└── README.md               # This file
```

The whole app is a single self-contained HTML file with inline CSS, JavaScript, and structured data.

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
4. **Flip a card** — Click a card to see its back, click again to open details
5. **View details** — See full interpretations: general, love, career, spirituality
6. **Toggle orientation** — Switch between **Xuôi** (Upright) and **Ngược** (Reversed) — all sections update
7. **Yes/No/Maybe** — Check the quick answer badge at the top of the modal
8. **Astrology** — Expand the "Tương ứng Chiêm học" section for zodiac, planet, element
9. **Combine cards** — Click "Xem Kết Hợp" to select a second card and read the combined meaning
10. **Beginner guide** — Click the ❓ button in the header for a Tarot introduction

## Deployment

Because this is a single static HTML file, you can deploy it to any static host:

- [Vercel](https://vercel.com)
- [GitHub Pages](https://pages.github.com/)
- [Netlify](https://netlify.com)
- [Cloudflare Pages](https://pages.cloudflare.com)
- Any CDN or static file server

## Browser Support

Works in all modern browsers that support:
- CSS Grid, Flexbox & 3D transforms
- ES6+ JavaScript
- Service Workers (for offline caching)

## Credits

- Card meanings based on traditional Rider-Waite-Smith Tarot interpretations
- Astrology correspondences based on Golden Dawn system
- Visual theme inspired by the **Dalí Tarot** deck published by TASCHEN

## License

MIT License — feel free to use, modify, and distribute.
