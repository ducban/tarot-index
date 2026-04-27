# AGENTS.md — B3's Tarot Index

> This file is written for AI coding agents. It describes the actual state of the project as found in the working tree. Do not assume anything that is not documented here.

---

## Project Overview

**B3's Tarot Index** is a static, single-page web application that lets users look up meanings for all 78 Tarot cards. The interface and content are in Vietnamese. The app is styled with a dark, gold-accented theme inspired by the Dalí Tarot deck from TASCHEN.

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
    love: string,           // Upright love meaning
    loveReversed: string,   // Reversed love meaning
    career: string,         // Upright career meaning
    careerReversed: string, // Reversed career meaning
    spirituality: string,   // Upright spirituality meaning
    spiritualityReversed: string, // Reversed spirituality meaning
    yesNo: { upright: "yes"|"no"|"maybe", reversed: "yes"|"no"|"maybe" },
    zodiac: string,         // e.g. "Aries ♈" or "" for cards without zodiac
    planet: string,         // e.g. "Mars" or ""
    element: string         // "Fire"|"Water"|"Air"|"Earth"
  }
  ```
- **Rendering** is imperative: `renderAll()` filters the data, groups it by suit/arcana, builds HTML strings, and injects them into `cardContainer.innerHTML`.
- **Cards** use a CSS 3D flip animation — first click flips the card, second click opens the detail modal.
- **Modal** shows detailed meanings with an orientation toggle (upright/reversed). All three sub-sections (love, career, spirituality) now have separate reversed text. Includes Yes/No badge, collapsible astrology section, and a combo button.
- **Combo (Card Combinations)** — From the detail modal, users can click "Kết Hợp" to select a second card and see a generated combination meaning.
- **Intro Modal** — A ❓ button in the header opens a guide explaining Tarot basics and how to use the app.
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

## Content & Language Guidelines

- **Content language:** Vietnamese (`vi`). Headings and terminology stay in English.
- **Writing style:** Use layman's terms — simple, accessible language as seen in existing card descriptions.
- **Agent responses:** Always respond in Vietnamese. Confirm understanding of the task, wait for user approval before implementing.
- **Documentation reference:** Always use Context7 for development documentation.

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

- The canonical URL is `https://tarrot.hiem.co/`. The OG image, Twitter Card, and breadcrumb structured data all reference this domain.
- The inline Web App Manifest has `start_url` set to `./` (root).
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
- Card data is inlined as a large JavaScript array, making the HTML file ~192 KB. This is intentional for offline-first behavior but means any content edit requires modifying the single source file.
- There are no image assets; card faces are rendered purely with CSS and emoji icons.
- **Suit descriptions:** When a category filter is selected (e.g. Wands), a description block is shown above the card grid in the format: `Title (Vietnamese name) \n\n X lá bài \n\n <explanation in Vietnamese>`. The description data lives in the `suitDescriptions` object inside `<script>`.
- **Card flip:** Cards in the grid use CSS 3D flip (first click = flip to reveal back pattern, second click = open modal). Implementation uses `.tarot-card-wrapper` with `.tarot-card-inner` for perspective transform.
- **Yes/No/Maybe:** Each card has a `yesNo` object with upright and reversed values. Displayed as a colored badge in the modal.
- **Astrology:** Each card has `zodiac`, `planet`, `element` fields. Shown in a collapsible section within the modal (click "🔮 Tương ứng Chiêm học" to expand).
- **Card Combinations:** Users can select a second card from the detail modal via the "🔗 Xem Kết Hợp" button. The combination meaning is generated algorithmically based on card properties (element, suit, arcana, keywords).
