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
- **Tote specs** — "27 gallon", "30.5 x 20.5 x 15 in", "400 lbs stack capacity", "$1.50/week" and
  the "$4.00/bag per week" clothes-bag rate are all still generator-invented; measure and price
  the real units
- **Sanitizing claims** — "high-pressure thermal wash", "botanical sanitization",
  "hospital-grade steam clean" describe a process nobody specified
- **Footer legal links** (Terms, Privacy, Tote Rental Agreement) all point at `#`

### The booking form does not send anything

`handleReservationSubmit()` calls `preventDefault()`, closes the modal, and shows a
confirmation toast. No request is made — submissions go nowhere. Wire it to a form
backend (Formspree, Netlify Forms, a serverless endpoint) before taking real bookings,
or the toast is telling customers a reservation was made when it wasn't.

### Tailwind CDN

`cdn.tailwindcss.com` compiles classes in the browser and logs a production warning to the
console. Fine for a demo; for a real launch, build the CSS once and ship a static stylesheet.

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
