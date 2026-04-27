# AGENTS.md — Dalí Tarot Guide

> This file is written for AI coding agents. It describes the actual state of the project as found in the working tree. Do not assume anything that is not documented here.

---

## Project Overview

**Dalí Tarot Guide** is a static, single-page web application that lets users look up meanings for all 78 Tarot cards. The interface and content are in Vietnamese. The app is styled with a dark, gold-accented theme inspired by the Dalí Tarot deck from TASCHEN.

- **Primary language of content:** Vietnamese (`vi`)
- **Primary language of code comments:** English
- **No server-side runtime required.** The entire app is a self-contained HTML file.

---

## Technology Stack

| Layer | Technology |
|-------|------------|
| Markup | HTML5 |
| Styling | CSS3 (all inline in `<style>`) |
| Logic | Vanilla JavaScript (ES6+, all inline in `<script>`) |
| Build Tool | None |
| Package Manager | None |
| Dependencies | None |
| Test Framework | None |

The project is deliberately zero-dependency. There is no bundler, transpiler, or framework.

---

## Project Structure

```
.
└── index.html          # The entire application (507 lines)
```

There is only one file. Everything lives inside `index.html`:

1. **`<head>`** — Meta tags, SEO / Open Graph / Twitter Card tags, inline Web App Manifest (base64 data URL), and all CSS.
2. **`<body>`** — Application shell (header, search bar, filter buttons, card grid container, footer, modal overlay).
3. **First `<script>`** — Card data array (`const cards = [...]`), DOM references, state, render functions, event listeners, inline service-worker registration, and initialization.
4. **JSON-LD `<script>` blocks** — Structured data for Schema.org (`WebApplication` and `BreadcrumbList`).

---

## Runtime Architecture

The app runs entirely in the browser:

- **State** is held in three module-level variables: `currentFilter`, `searchQuery`, `currentOrientation`.
- **Data** is a hard-coded array of 78 card objects. Each card object has the following shape:
  ```js
  {
    id: string,
    name: string,           // English card name
    vietnameseName: string, // Vietnamese card name
    number: string,         // Roman numeral or number
    arcana: "major" | "minor",
    suit: "wands" | "cups" | "swords" | "pentacles" | null,
    keywords: string,
    upright: string,        // General upright meaning (Vietnamese)
    reversed: string,       // General reversed meaning (Vietnamese)
    love: string,
    career: string,
    spirituality: string
  }
  ```
- **Rendering** is imperative: `renderAll()` filters the data, groups it by suit/arcana, builds HTML strings, and injects them into `cardContainer.innerHTML`.
- **Modal** shows detailed meanings. An orientation toggle switches between upright and reversed interpretations. Love, career, and spirituality sections do not yet have separate reversed text (the code references the same property regardless of orientation).
- **Service Worker** is created dynamically from an inline string and registered as a Blob URL. It implements a simple cache-first fetch strategy.

---

## How to Run / Build

Because there is no build step, simply open the file in a browser or serve it with any static file server.

Examples:

```bash
# Using Python
python3 -m http.server 8000

# Using Node.js (if npx serve is available)
npx serve .

# Using PHP
php -S localhost:8000
```

Then navigate to `http://localhost:8000/index.html`.

---

## Code Style Guidelines

- **Single-file architecture.** All new features, styles, and data must be added to `index.html` unless there is a strong reason to split files.
- **CSS** is organized with section comments (`/* ========== SECTION NAME ========== */`). Keep that convention.
- **JavaScript** uses `const`/`let`, template literals, and arrow functions. Follow the existing imperative-DOM style.
- **Data modifications** (adding new cards or updating meanings) should preserve the existing JSON schema so that `renderAll()` and the modal continue to work.

---

## Testing

There is no automated test suite. Manual testing checklist:

1. Open `index.html` in a modern browser.
2. Verify all 78 cards render grouped by Major Arcana, Wands, Cups, Swords, Pentacles.
3. Test search: type keywords in Vietnamese or English and confirm filtering works.
4. Test filters: click each category button and confirm correct subset appears.
5. Test modal: click a card, toggle Xuôi / Ngược, verify content updates.
6. Test keyboard: press `Escape` to close the modal.
7. Test responsive layout on mobile widths.
8. Verify the page works offline after the first load (service worker caching).

---

## Deployment Considerations

- The canonical URL placeholder in the `<link rel="canonical">` tag and in breadcrumb structured data is `https://your-domain.com/tarot.html`. Update this before deploying to production.
- The inline Web App Manifest contains a placeholder `start_url` of `./tarot.html`. Align this with the actual deployed filename (`index.html` or `tarot.html`).
- No environment variables or secret keys are present.
- Because the app is a single static file, it can be deployed to any static host (GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3, etc.).

---

## Security Considerations

- There is no user input that gets written to the DOM via `innerHTML` from an untrusted source. Search terms are only used for `.includes()` comparisons against the hard-coded data array, and the rendered HTML is generated from trusted template strings. Therefore, XSS risk from the search feature is minimal.
- No external scripts, CDNs, or trackers are loaded.
- The service worker caches all requests unconditionally; review the fetch handler if the app is expanded to include API calls.

---

## Known Limitations / Notes for Agents

- The `love`, `career`, and `spirituality` fields currently show the same text for both upright and reversed orientations. The modal code reads `card.love`, `card.career`, and `card.spirituality` regardless of `currentOrientation`. If separate reversed interpretations are needed, the data schema and `buildMeaningsHTML` will require updates.
- Card data is inlined as a large JavaScript array, making the HTML file ~123 KB. This is intentional for offline-first behavior but means any content edit requires modifying the single source file.
- There are no image assets; card faces are rendered purely with CSS and emoji icons.
