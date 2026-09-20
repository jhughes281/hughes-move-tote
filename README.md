# Titan Move & Tote Co.

Single-page marketing site for a local moving company: box-truck moving crews plus
reusable stackable tote rentals. Dark industrial look — hazard-stripe accents,
Oswald / Chakra Petch type, yellow-on-near-black palette.

**Live:** https://jhughes281.github.io/titan-move-tote/

## Sections

| Anchor | What's there |
| --- | --- |
| (hero) | Headline, dispatch phone CTA, tote hero image |
| `#services` | Service cards — tote rental, full move, truck + tote combo |
| `#fleet` | Box truck lineup and capacity |
| `#tote-rentals` | Tote specs, clothes-bag add-on (`#clothes-totes`) |
| `#how-it-works` | Drop-off → pack → move → pickup steps |
| `#tote-calculator` | Interactive estimator — tote count, bags, rental length |
| `#pricing` | Flat-rate packages |
| (FAQ) | Accordion |
| (footer) | Service radius, hours, direct dispatch contact |

A booking modal collects name / phone / email / move date / move type.

## Stack

Plain static HTML — no build step, no dependencies to install. Loaded from CDNs at runtime:

- Tailwind CSS (`cdn.tailwindcss.com`, JIT in the browser; config is inline in `<head>`)
- Google Fonts — Inter, Oswald, Chakra Petch
- Font Awesome 6.4

All page JS is inline at the bottom of `index.html` (calculator, modal, FAQ, toast, mobile nav).

## Local preview

```
py -m http.server 8756 --directory C:/Users/JayHu/Sites/titan-move-tote
```

Then open http://localhost:8756 — or use the `titan-move-tote` entry in `~/.claude/launch.json`.

## Deploy

GitHub Pages, `main` branch, `/ (root)`. Push to `main` and it redeploys.

## Before this goes live — placeholders to replace

Everything below is invented sample copy, not real business data:

- **Phone** `(555) 848-2668` — appears as display text and in the `tel:` link
- **Email** `dispatch@titanmovetote.com`
- **Address** `400 Industrial Parkway, Bay 4`
- **Service areas** "Metro Central, Suburbs, North District, Riverfront Corridor (45 Mile Max Radius)"
- **Hours** Mon–Sun 7:00 AM – 8:00 PM
- **"USDOT & State Licensed Carrier"** badge in the footer — only keep this once there is a real
  USDOT number; put the actual number next to it
- **Pricing** in `#pricing` and the `#tote-calculator` rates — confirm every number
- **Per-size pricing.** The totes come in four real sizes (below) but the calculator still
  charges one flat `$1.50/tote/week` and the packages just say "25 Totes", "40 Totes". Either
  price by size or say somewhere that the rate is the same whichever size you take.
- **Clothes bag rate** `$4.00/bag per week` — still invented
- **Sanitizing claims** — "high-pressure thermal wash", "botanical sanitization",
  "hospital-grade steam clean" describe a process nobody specified
- **"Ideal for" lines** in the size table (books/kitchen/everyday/bedding) are my packing
  suggestions, not something the owner specified — reword freely
- **Footer legal links** (Terms, Privacy, Tote Rental Agreement) all point at `#`

### The booking form does not send anything

`handleReservationSubmit()` calls `preventDefault()`, closes the modal, and shows a
confirmation toast. No request is made — submissions go nowhere. Wire it to a form
backend (Formspree, Netlify Forms, a serverless endpoint) before taking real bookings,
or the toast is telling customers a reservation was made when it wasn't.

### Tailwind CDN

`cdn.tailwindcss.com` compiles classes in the browser and logs a production warning to the
console. Fine for a demo; for a real launch, build the CSS once and ship a static stylesheet.

## Tote sizes

Real, owner-supplied. Metric is the source; inches and gallons are converted from it.

| | Volume | Dimensions (L × W × H) |
| --- | --- | --- |
| Small | 25 L / 6.6 gal | 47 × 33 × 27 cm — 18.5 × 13 × 10.6 in |
| Medium | 55 L / 14.5 gal | 60.5 × 39 × 38.5 cm — 23.8 × 15.4 × 15.2 in |
| Large | 100 L / 26.4 gal | 75.5 × 51 × 36.5 cm — 29.7 × 20.1 × 14.4 in |
| X-Large | 150 L / 39.6 gal | 92 × 52 × 42 cm — 36.2 × 20.5 × 16.5 in |

These replaced a single invented "27 gallon / 400 lb stack capacity" spec sheet.

## Assets

Photos of the real equipment, supplied by the owner:

| File | What it is | Used for |
| --- | --- | --- |
| `tote-stackable.png` | Black tote, yellow snap lid | Everything except clothes — hero + spec sheet + social image |
| `clothes-bag-green.png` | Green woven zip-top bag | Clothes |
| `clothes-bag-blue.png` | Blue woven zip-top bag | Clothes |
| `wardrobe-box.png` | Cardboard wardrobe box, hanging bar | Clothes that stay on hangers |

The hero card has a four-way switcher (`showGear()`) that swaps between them. These replaced
a stock render that was hot-linked from the generator's CDN and had "YOUR LOGO" printed on it.
