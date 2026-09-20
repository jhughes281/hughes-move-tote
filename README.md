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
| `#tote-calculator` | Two-tab estimator — tote rental, or full move with totes added |
| `#pricing` | The two service tiers |
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
- **Hourly moving rates** — `$140/hr` (26ft), `$110/hr` (16ft), `+$45/hr` per extra mover are
  all still invented. They drive the full-move estimate, so they are the most visible
  remaining placeholder.
- **Clothes bag / wardrobe box rate** `$4.00 per week` — still invented
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

## Rates

All prices come from one `RATES` object in the page script. `renderRates()` paints them into
any element tagged `data-rate="..."`, and both calculator tabs read the same object — change a
number in `RATES` and the tier cards, the body copy and the estimates all follow.

```js
const RATES = {
  totePerWeek: { rentalOnly: 2.50, withMove: 1.50 },
  bagPerWeek:       4.00,                           // PLACEHOLDER
  truckHourly:      { '26ft': 140, '16ft': 110 },   // PLACEHOLDER
  extraMoverHourly: 45                              // PLACEHOLDER
};
```

**Real (owner-supplied):** totes are **$2.50/tote/week** on their own, **$1.50/tote/week** when
booked with the moving crew. Same price whichever of the four sizes you take. Everything marked
PLACEHOLDER still needs a real number.

## The two tiers

1. **Totes Only** — customer moves themselves, we drop off and collect. Totes at the higher rate.
2. **Moving Service + Totes** — crew and box truck, billed hourly, totes at the lower rate.

`#pricing` presents them as two cards; the calculator's two tabs price them. Each tab shows the
other tier's number too, so the saving is visible either way: the rental tab says what the same
totes would cost with a move booked, and the move tab shows what was saved.

This replaced three invented flat packages ($89 / $149 / $239 for 15 / 30 / 50 totes) whose
totals matched no per-tote rate, and a "20% off bundle" that was really a hardcoded $95.

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
