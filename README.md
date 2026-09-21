# Hughes Move & Tote Co

Single-page marketing site for a local moving company: box-truck moving crews plus
reusable stackable tote rentals. Dark industrial look — hazard-stripe accents,
Oswald / Chakra Petch type, yellow-on-near-black palette.

**Live:** https://jhughes281.github.io/hughes-move-tote/

## Sections

| Anchor | What's there |
| --- | --- |
| (hero) | Headline, dispatch phone CTA, tote hero image |
| `#services` | Service cards — tote rental, full move, truck + tote combo |
| `#fleet` | Box truck lineup and capacity |
| `#tote-rentals` | Tote specs, clothes-bag add-on (`#clothes-totes`) |
| `#how-it-works` | Drop-off → pack → move → pickup steps |
| `#keep-totes` | Keep totes after the move + garage storage install |
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

## Mobile

Audited at 375 / 768 / 1440 on 21 Sep 2026. No horizontal scroll at any width.

- **Tap targets** — 45 of 46 interactive elements are 44px or larger. The exception is a link
  inside a sentence in How It Works step 3; padding it to 44px would break the paragraph, and
  inline prose links are not controls. `.tap` and `.tap-icon` in the `<style>` block enforce the
  minimum; use them on any new link or button.
- **Range sliders** are custom-styled (`input[type=range].slider`): a 10px track with a 28px
  thumb inside a 44px hit area. The stock Tailwind sliders were 10px tall and unusable by thumb.
- **`prefers-reduced-motion`** is honoured — animations and smooth scrolling are cut.
- **Slider headers** stack under 640px; label and count collided on one line at 375.
- The decorative blur in the tote spec card sits at `right-0`, not `-right-12`. It escaped its
  clipping parent and put ~11px of sideways scroll on tablet. **Do not fix that class of bug with
  `overflow-x: hidden` on `html`** — the header is `position: sticky` and it would break.

Known and deliberate: roughly 25 elements use 10–11px text, mostly mono spec detail (tote
dimensions, captions). Legible but small; worth revisiting if anyone complains.

## Local preview

```
py -m http.server 8756 --directory C:/Users/JayHu/Sites/hughes-move-tote
```

Then open http://localhost:8756 — or use the `hughes-move-tote` entry in `~/.claude/launch.json`.

## Deploy

GitHub Pages, `main` branch, `/ (root)`. Push to `main` and it redeploys.

## Before this goes live — placeholders to replace

Everything below is invented sample copy, not real business data:

- **Phone** `(555) 848-2668` — appears as display text and in the `tel:` link
- **Email** `dispatch@hughesmovetote.com`
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
  toteTiers: [                        // standalone rental, per tote per week
    { min: 40, rate: 2.25 },
    { min: 20, rate: 2.75 },
    { min: 0,  rate: 3.50 }
  ],
  toteWithMove:     1.50,             // per tote per week, booked with the crew
  toteMinimum:      99.00,            // floor on a rental-only order
  toteKeepPrice:    35.00,            // buy a tote outright after the move
  bagPerWeek:       3.00,
  bagWithMove:      2.00,
  wardrobePerMove:  8.00,
  truckHourly:      { '26ft': 129, '16ft': 109 },   // includes 2 movers
  extraMoverHourly: 45,
  tripFee:          95.00,
  minimumHours:     2
};
```

### Why these numbers

Set Sept 2026 against Houston market data and the owner's real cost: **totes land at about $16
each** (100 for ~$1,600 delivered), so every tier repays a tote within a handful of rentals and
then runs at high margin for years.

**Tote rates are tiered because a small order costs the same two round trips as a big one.** A
flat rate priced the small jobs below cost and the big jobs above the competition — Houston
rivals (Stack, HiveBoxx, Rentacrate) sell home-size packages whose curve is much flatter than a
linear per-tote rate. The tiers plus the $99 minimum keep every size roughly 5–35% under Stack's
package while covering the drive.

**The hourly rate came down and a trip fee went on.** Houston's going rate for two movers and a
truck is roughly $90–135/hr, and local firms add an $80–175 trip fee. The old $140/hr sat above
the band with no fee — the worst of both, since the headline rate is what customers compare.
$129/hr plus a $95 trip fee nets more on a 4-hour job ($611 vs $560) while advertising less.

**`toteWithMove` at $1.50 is close to break-even** once handling is counted — it is a hook to win
the move, not a profit line. Raise it to ~$1.75 if it should contribute.

**`toteKeepPrice` is $35, not the ~$20 first suggested.** A tote in rotation earns roughly $37 a
year; selling at $20 would net about $4 and kill the recurring income.

### Still needed

Loaded labour cost per mover-hour (wage plus payroll tax, insurance, comp) and the monthly truck
and insurance cost. Those give a true break-even hourly rate and firm up the per-rental handling
cost, currently assumed at $1.00–1.50 per tote. **Rails and shelving for the Keep Your Totes
service are still unpriced** — quote-only on the page.

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
| `storage-shelving.jpg` | Wire shelving in a garage holding the totes | Keep Your Totes |
| `storage-overhead-rails.jpg` | Totes mounted overhead on rails | Keep Your Totes |
| `storage-overhead-loaded.jpg` | Overhead totes holding gear | Keep Your Totes |

The hero card has a four-way switcher (`showGear()`) that swaps between them. These replaced
a stock render that was hot-linked from the generator's CDN and had "YOUR LOGO" printed on it.

**The three `storage-*.jpg` files are the vendor's product photography**, taken from the website
of the supplier we buy the hardware from (the overhead rail system is Koova's). Listing UI —
favourite and zoom icons, photo-count and SKU badges — was painted out, and the one shot with
the vendor logo on the product was dropped, so nothing live carries their branding.

Being on the vendor's site is not a licence, though. **Ask them for written permission or a
dealer/media asset pack** — installers using a manufacturer's imagery is the normal case and
usually granted, often with better files than these. Swap in photos of a real install as soon as
there is one.
