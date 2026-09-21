# Ayeza Electrical Supplies Inc.

Static marketing website for Ayeza Electrical Supplies Inc., an ECRA/ESA licensed electrical
contractor in St. Thomas, Ontario. No framework, no build step, no runtime dependencies.

## Run it locally

```bash
python3 -m http.server 8123
# open http://localhost:8123
```

Any static host works: Netlify, Vercel, Cloudflare Pages, GitHub Pages, S3, or plain nginx.
Upload the repository root as-is.

## Files

```
index.html          Home
services.html       Residential, commercial, EV charging, design and permits
about.html          Company, standards, process, coverage
contact.html        Quote request form, phone, text, email, hours
credits.html        Project media provenance
404.html            Not found page
robots.txt          Crawler rules
sitemap.xml         Sitemap
assets/css/styles.css   Design system, single stylesheet
assets/js/main.js       Navigation, scroll reveal, form handling
assets/fonts/           Self-hosted Geist and Geist Mono (variable woff2)
assets/img/             Favicon, touch icon, and social sharing image
assets/newimages/       Supplied project photography and optimized video
```

## What to replace before launch

Everything in this table is a placeholder or an unverified claim. Nothing else on the site is
business-specific, so this is the whole list.

| Where | Current value | What to do |
|---|---|---|
| Phone number, every page | `(647) 919-7869` and `tel:+16479197869` | Confirm this is the number that should receive calls and texts. It came from a public contractor directory listing. |
| Domain, canonical and OG tags, `robots.txt`, `sitemap.xml`, JSON-LD | `https://ayezaelectrical.ca` | Replace with the live domain, then update `sitemap.xml` and every JSON-LD block. |
| Address, footer and about page | `230 Edward Street, St. Thomas, ON` | Confirm against the real place of business. |
| Opening hours, footer, contact page, JSON-LD | Mon to Fri 9am to 5pm, Sat 9am to 3pm, Sun closed | Confirm the real hours. |
| Licence claim, footer, hero, FAQ | "ECRA/ESA licensed contractor" | Add the real ECRA/ESA licence number. The listing confirmed an active licence but did not publish the number. |
| Insurance wording, about page and FAQ | "certificate of insurance available on request" | Confirm a current policy exists, or remove the line. |
| Service list, home and services pages | Residential, commercial, EV charging, lighting, design and permits | Confirm the company does each of these. The list is standard for an Ontario licensed contractor, not taken from the business. |
| Engineering wording, services page | "sealed engineering drawings ... bring in the right professional" | Confirm the arrangement for sealed drawings. Do not claim engineering work that requires a P.Eng unless one is engaged. |
| Service area, home and about pages | 16 Elgin County municipalities | Confirm which areas are actually served. |
| Response time, home page FAQ | "Most quotes are scheduled within a few business days" | Confirm or remove. |
| Trading name | "Ayeza Electrical" on site, "Ayeza Electrical Supplies Inc." in the footer | Confirm the name the company trades under. |

## The contact form

`assets/js/main.js` has one constant near the form section:

```js
var FORM_ENDPOINT = '';
```

* **Empty (current):** the form validates, then hands the completed message to the visitor's mail
  client with `mailto:`. It never silently drops a submission.
* **Set:** the form posts JSON to that URL and shows sending, success, and error states. Works with
  Formspree, Netlify Forms, Basin, or any endpoint that accepts `POST` JSON.

The form also has a honeypot field (`name="company"`) that is invisible to people. Service cards
deep-link into the form with `contact.html?service=ev-charging`, and the matching option is
pre-selected on load.

## Design system notes

* One accent colour for the whole site (`--accent`, copper `#9c3b08`). White text on it passes
  WCAG AA at 6.9:1, and the accent on the paper background passes at 6.33:1.
* One theme for the whole site. No section inverts to a dark panel.
* Shape rule, applied everywhere: containers 20px, media nested in containers 14px, inputs 12px,
  interactive controls pill.
* Type is self-hosted Geist and Geist Mono, variable weight, latin and latin-ext subsets, 84KB total.
* Motion is CSS and IntersectionObserver only. Every animation uses `transform` and `opacity`, and
  everything is disabled under `prefers-reduced-motion`. The nav state uses a 1px sentinel plus
  IntersectionObserver, so no scroll listener runs on scroll frames.
* Icons are Phosphor (MIT), vendored as an inline SVG sprite at the top of each page. No icon
  library is loaded at runtime.
* The hero image is the largest contentful paint. It is fetched with `fetchpriority="high"` and
  decoded synchronously. Do not add `decoding="async"` to it: that delays paint of the first
  screen. The lazy images below the fold keep `decoding="async"`.

## Accessibility

* Skip link, landmark elements, and one `h1` per page.
* Visible focus rings on every interactive element.
* Mobile menu traps focus, closes on Escape, and restores focus to the trigger.
* Form errors are announced through `aria-live` and tied to their fields with `aria-invalid`.
* No text below 12px. Colour pairs used for text meet WCAG AA.

## Project media

The site uses project photographs and video supplied directly by Ayeza Electrical. Optimized WebP
images and an audio-free, fast-start MP4 live in `assets/newimages/`; the original supplied files
remain in the same folder. `credits.html` records the media provenance.
