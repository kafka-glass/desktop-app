# KafkaGlass Explorer — website (GitHub Pages)

Landing page, support portal and privacy policy for **KafkaGlass Explorer**, a native macOS client for Apache Kafka.

Static HTML + CSS, no build step. Open `index.html` in a browser or serve the folder:

```bash
python3 -m http.server 8000
```

| File | Purpose |
| --- | --- |
| `index.html` | Landing page: hero, features, tour, security, FAQ, download |
| `support.html` | Support & troubleshooting |
| `privacy.html` | Privacy policy (App Store Connect privacy URL) |
| `style.css` | Shared styles |
| `icon.png` | App icon (512 px, from the app's asset catalog) |
| `screenshots/` | Real app screenshots (see below) |
| `og.png` | Social preview image, 1200 × 630 (**to add**) |
| `robots.txt`, `sitemap.xml`, `site.webmanifest` | SEO / metadata |

## Screenshots

Every screenshot slot on the landing page shows a CSS mock of the app until the matching file
exists. Drop a PNG into `screenshots/` and it replaces the mock automatically — no HTML edits.

| File | Where | What to capture | Suggested size |
| --- | --- | --- | --- |
| `screenshots/hero.png` | Hero | Main window: sidebar, a topic's **Data** tab and the message inspector | 2240 × 1160 or wider, ~1.93 : 1 |
| `screenshots/inspector.png` | Tour · Message Inspector | Inspector with a decoded Avro record | ~1.4 : 1 |
| `screenshots/produce.png` | Tour · Produce | Produce Message sheet after a successful send | ~1.4 : 1 |
| `screenshots/statistics.png` | Tour · Statistics | Statistics › Overview with live charts | ~1.4 : 1 |
| `screenshots/schema-registry.png` | Tour · Schema Registry | Subjects list with a version diff | ~1.4 : 1 |

Tips: capture windows **without the shadow** (⌘⇧4, Space, then hold ⌥ while clicking), use dark
appearance to match the site, and keep production data out of the shots. Optimize PNGs
(e.g. `oxipng` or ImageOptim) — aim for under ~500 KB each.

## Download links

At the bottom of `index.html`:

```js
var APP_STORE_URL = "";        // e.g. https://apps.apple.com/app/id0000000000
var DIRECT_DOWNLOAD_URL = "";  // e.g. https://github.com/kafka-glass/desktop-app/releases/latest/download/KafkaGlassExplorer-macOS.zip
```

While empty, the buttons scroll to the Download section and the direct download shows "Soon".

## Domain

All canonical / Open Graph / sitemap URLs use `https://kafka-glass.github.io/desktop-app/`. If the site lives
elsewhere, replace that string across `*.html`, `robots.txt` and `sitemap.xml`.
