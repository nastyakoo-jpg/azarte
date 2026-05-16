# AZARTE — Strategic Design & Brand Intelligence Studio

Single-page trilingual studio website (ES default, EN, RU). Vanilla HTML + CSS + JS, no build step, no frameworks, no Webflow runtime.

```
azarte-site/
├── index.html        ← the entire site (HTML + embedded CSS + embedded JS)
├── README.md         ← this file
└── assets/
    ├── favicon.svg
    ├── banner.jpg              ← hero background, desktop
    ├── banner-mobile.jpg       ← hero background, mobile
    ├── hero-video.mp4          ← optional hero video (alternative to banner)
    ├── poster-hero.jpg         ← poster frame for hero video
    ├── poster-work.jpg         ← reserved poster frame for future work videos
    ├── work-eugemakeup.jpg     ← project card · Euge Gri
    ├── work-svetlana.jpg       ← project card · Svetlana Evtushenko
    ├── work-aditi.jpg          ← project card · Aditi Pilates
    ├── work-azarte.jpg         ← reserved · self-card variant uses gradient by default
    ├── about-1.jpg             ← about / studio mood image 1
    ├── about-2.jpg             ← about / studio mood image 2
    └── about-3.jpg             ← about / studio mood image 3 (wide)
```

---

## Quick start — open locally

The site has zero dependencies. To preview it on your machine:

**Option A — just open the file**
Double-click `index.html`. It works in any modern browser.
*Caveat:* some browsers (mostly Chrome on macOS) block autoplay videos when opened via `file://`. If you switch to the video hero, use option B instead.

**Option B — run a tiny local server**
From inside the `azarte-site` folder, in Terminal:

```bash
# Python 3
python3 -m http.server 5500
```

Then open http://localhost:5500 — the video hero will work correctly.

---

## Replacing the media

All media lives in `assets/`. Filenames must stay exactly as listed below — the HTML references them by name. Just overwrite the placeholder JPGs with your real ones, keeping the same filename, and the site picks them up automatically.

| File                       | Purpose                              | Recommended size      | Format        |
|----------------------------|--------------------------------------|-----------------------|---------------|
| `banner.jpg`               | Hero background, desktop             | **1920×1080**         | JPG, ≤ 400 KB |
| `banner-mobile.jpg`        | Hero background, mobile (vertical)   | **800×1200**          | JPG, ≤ 200 KB |
| `hero-video.mp4`           | Hero video (optional, see below)     | 1920×1080, ~15 sec    | MP4 H.264, ≤ 4 MB |
| `poster-hero.jpg`          | Static frame shown before video loads| **1920×1080**         | JPG, ≤ 300 KB |
| `work-eugemakeup.jpg`      | Project card · Euge Gri              | **1200×1500** (4:5)   | JPG, ≤ 350 KB |
| `work-svetlana.jpg`        | Project card · Svetlana Evtushenko   | **1200×1500** (4:5)   | JPG, ≤ 350 KB |
| `work-aditi.jpg`           | Project card · Aditi Pilates         | **1200×1500** (4:5)   | JPG, ≤ 350 KB |
| `work-azarte.jpg`          | Reserved (self-card uses gradient)   | **1200×1500** (4:5)   | JPG, ≤ 350 KB |
| `about-1.jpg`              | About collage, top-left              | **900×1200** (3:4)    | JPG, ≤ 250 KB |
| `about-2.jpg`              | About collage, top-right             | **900×1200** (3:4)    | JPG, ≤ 250 KB |
| `about-3.jpg`              | About collage, full-width            | **1600×900** (16:9)   | JPG, ≤ 350 KB |
| `favicon.svg`              | Browser tab icon                     | 32×32 vector          | SVG           |

**Don't rename these files.** If you need different filenames, you'll have to update the references inside `index.html`.

**Tip — keep weights low.** Use [Squoosh](https://squoosh.app) or [TinyPNG](https://tinypng.com) before uploading. Large images make the hero feel slow even if everything else is well-coded.

---

## Hero — image OR video

The hero supports two versions. You pick one. Image is active by default.

Open `index.html` and find this block (around the `HERO · / 01` section):

```html
<div class="hero__bg" aria-hidden="true">

  <!-- HERO IMAGE VERSION (active by default) ===================== -->
  <picture>
    <source media="(min-width: 720px)" srcset="assets/banner.jpg">
    <img src="assets/banner-mobile.jpg" alt="" width="1600" height="900">
  </picture>

  <!-- HERO VIDEO VERSION (uncomment to activate; comment image above) ===
  <video autoplay muted loop playsinline poster="assets/poster-hero.jpg">
    <source src="assets/hero-video.mp4" type="video/mp4">
  </video>
  ==================================================================== -->

</div>
```

**To switch to video:**
1. Wrap the `<picture>...</picture>` block in HTML comment markers `<!-- ... -->`
2. Remove the comment markers around the `<video>` block
3. Make sure `assets/hero-video.mp4` and `assets/poster-hero.jpg` exist

The `autoplay muted loop playsinline` attributes are required — together they're what makes background video work on iOS Safari.

---

## How to edit translations

Every visible text on the site lives in one place — the **`I18N`** object near the top of the `<script>` block at the bottom of `index.html`.

The structure is:

```js
const I18N = {
  es: { 'nav.work': 'Trabajos', ... },
  en: { 'nav.work': 'Work',     ... },
  ru: { 'nav.work': 'Работы',   ... }
};
```

To change any piece of copy:

1. Open `index.html` in any text editor
2. Press Cmd+F (Mac) / Ctrl+F (Windows) and search for `I18N DICTIONARY`
3. Find the key you want — for example, `'hero.statement'` or `'serv.1.desc'`
4. Edit the text inside the quotes for `es`, `en`, and `ru`
5. Save the file and refresh your browser

**Important rules when editing:**

- **Don't remove the keys.** Every key must exist in all three languages (`es`, `en`, `ru`). If a translation is missing, the language switcher will skip that node and leave the previous language showing.
- **Keep the `<em>` tags.** They mark the burgundy accent words. For example: `'La atención no se da. <em>Se diseña.</em>'` — the part inside `<em>` becomes burgundy.
- **Escape apostrophes.** Inside single-quoted strings, write `\'` instead of `'`. Example: `'don\'t fail'`.
- **HTML is allowed inside values.** You can keep `<a>`, `<em>`, `<span>`, `<br>` — they render correctly because the script uses `innerHTML`.

**Spanish is the default.** The site loads in Spanish first time. Once a visitor picks a language, that choice is remembered in `localStorage` under the key `azarte-lang` and persists on return.

**Adding a new language?** Duplicate the `ru: { ... }` block, rename to e.g. `pt:`, translate every key, then add a button to the `<!-- LANGUAGE SWITCHER -->` block:

```html
<button type="button" data-lang-btn="pt" aria-pressed="false">PT</button>
```

Also add the new language to the `SUPPORTED` array and `HTML_LANG` object in the script — both are clearly labeled.

---

## Deploying

The site is one folder. Drop it on any host. No build, no install.

### Netlify (easiest, drag-and-drop)

1. Go to https://app.netlify.com/drop
2. Drag the entire `azarte-site` folder onto the page
3. Done. Netlify gives you a live URL in 10 seconds.
4. Optional: connect a custom domain in Site settings → Domain management.

### GitHub Pages

1. Create a new GitHub repo (public).
2. Upload the contents of the `azarte-site` folder to the root of the repo (so `index.html` sits at the top, not inside another folder).
3. Repo → Settings → Pages → Source: `main` branch, root folder.
4. Wait ~1 minute. Site lives at `https://YOUR-USERNAME.github.io/REPO-NAME/`.

### Vercel

1. Push the folder to a GitHub repo (as above).
2. https://vercel.com → Add New Project → import the repo.
3. Framework preset: **Other**. Build command: leave empty. Output directory: `.`
4. Deploy.

### Custom domain (any host)

Once the site is live on Netlify / Vercel / GitHub Pages, point your domain's DNS to the host's instructions. The host's docs walk through it — typically a CNAME record.

---

## Brand colors (for reference)

The brand tokens live as CSS custom properties at the top of the `<style>` block.

| Token                  | Value      | Use                              |
|------------------------|------------|----------------------------------|
| `--bg`                 | `#0D0D0D`  | Main background (near-black)     |
| `--bg-deep`            | `#080705`  | Section alternation              |
| `--ink`                | `#F4F1EB`  | Primary text (warm ivory)        |
| `--ink-soft`           | `#C8C2B5`  | Secondary text                   |
| `--ink-muted`          | `#8F8B82`  | Tertiary / labels                |
| `--burgundy`           | `#600318`  | Brand primary                    |
| `--burgundy-bright`    | `#8C1F38`  | Brand accent / hover             |
| `--burgundy-deep`      | `#3A0010`  | Deeper burgundy / gradients      |

Type:

| Token            | Family                                         |
|------------------|------------------------------------------------|
| `--font-display` | Archivo (700–900) — wordmarks and headings    |
| `--font-body`    | Manrope (400–600) — body copy                 |
| `--font-mono`    | JetBrains Mono — eyebrows, indices, labels    |

Fonts load from Google Fonts via `<link>` in the `<head>`. No npm, no build.

---

## Tech notes

- One file: `index.html` contains all HTML, all CSS (in `<style>`), all JS (in `<script>`).
- No external libraries. No jQuery, no React, no Webflow runtime.
- Animations use IntersectionObserver and CSS transitions. Lightweight, GPU-friendly.
- Respects `prefers-reduced-motion`. Marquee and entrance animations stop for users with that preference enabled.
- The navbar auto-hides on scroll-down for a less crowded reading experience and reappears on scroll-up.
- Mobile menu, language switcher, and footer year are all driven by ~80 lines of vanilla JS at the bottom of the file.

---

## Contact

Questions about the site itself? Edit and re-deploy.
Questions about the studio? Replies in 24 h on:

- WhatsApp · +54 9 11 3830 4569
- Email · azarte.studio.ar@gmail.com
- Instagram · @azarte.studio
- Telegram · @azartestudio

— AZARTE
