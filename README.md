# Valley Details website

One self-contained `index.html`. No build step, no dependencies, no framework.
Drop it on any static host (Fly static, Netlify, Vercel, Cloudflare Pages, S3).

Everything configurable lives in the `CFG` object near the bottom of the file.

---

## Two things to do first

### 1. Add the logo
Save it as **`assets/logo.png`** (transparent PNG, 600px wide is plenty). Until it
exists the header falls back to a built-in "VD / VALLEY DETAILS" wordmark, so nothing
looks broken.

### 2. Add photos
Instagram is login-walled to automated tools, so the posts could not be pulled down.
Drop files into `assets/` with these names and they appear immediately. Missing files
render as a labelled placeholder, never a broken image.

Four files. That is all.

| File | Where it shows |
|---|---|
| `ba1-before.jpg` / `ba1-after.jpg` | Slider 1, interior |
| `ba2-before.jpg` / `ba2-after.jpg` | Slider 2, exterior |

Shoot **the same angle** before and after. Stand in the same spot both times.
That is the whole effect.

**Three ways to run this section, pick whichever you can do today:**

1. **Real pairs.** Drop the four files above. Sliders appear.
2. **Finished shots only.** No "before" needed. Put filenames in `CFG.SHOTS`
   (`SHOTS: ['assets/shot1.jpg','assets/shot2.jpg']`) and drop them in. The
   sliders vanish, a clean grid takes their place, and the heading changes
   itself from "Drag the slider" to "Recent work."
3. **Nothing yet.** Leave `BA` and `SHOTS` both empty and the entire Our Work
   section removes itself. No placeholders, no gaps, page still reads complete.

**Do not generate fake "before" photos.** A dirtied-up clone of a real finished
car, captioned "same car, same day", is a fabricated result. It is the image
version of writing your own five-star reviews, and AI artifacts in reflections,
plates and trim are exactly what car people notice.

---

## Pricing

Three size tiers, currently half off. Regular prices sit at the Sacramento market rate
(Yelp reports a $167 typical, $95 low, $250 high across 50k local quotes).

| Tier | Regular | Promo |
|---|---|---|
| Sedan or coupe | $149 | $75 |
| SUV or crossover | $169 | $85 |
| Truck or 3-row | $189 | $95 |

Prices live in two places, both plain text:
- **Tiers section:** `data-promo` and `data-reg` attributes on each `.tier .amt`, plus
  the `.tier .old` strikethrough.
- **Hero slab:** `CFG.PRICE.headlineReg` / `headlinePromo` / `sizesReg` / `sizesPromo`.

Add-ons: pet hair $30, engine bay $35, stain and odor $40, headlights $50, ceramic $85.

Standing discounts (not tied to the promo, so they survive Sept 15):
- **$25 off the second car** at the same address, same stop. Costs no extra drive time.
- **Referral, $20 each way.** Friend gets $20 off their first, you get $20 off your next.
- **Club member rate, $119** on a sedan detail.

> Do **not** offer a discount for leaving a Google review. Google prohibits incentivised
> reviews and will filter or penalise them. Ask for reviews, just never pay for them.

---

## The promo countdown

```js
PROMO_END : '2026-09-15T00:00:00-07:00',   // midnight ending Mon Sept 14, Pacific
```

The instant it lapses, with no deploy, the page:

- turns the red top bar grey and reads "Promo has ended"
- drops the "Half Off" corner tag and the `$149` strikethrough
- reprices the hero slab to `$149` and the size line to the regular numbers
- rewrites all three tier cards to `$149 / $169 / $189` and hides the strikethroughs
- swaps the countdown for "Regular pricing / Ask about specials"
- changes "Claim Half Off" to "Book a Detail" and updates the closing section

Verified by running the page with a past `PROMO_END`.

For the next promo, change `PROMO_END`, the two `CFG.PRICE` promo strings, the
`data-promo` attributes, and the top-bar text.

---

## Booking

The calendar is **two steps on purpose**. Step one is only a day rail and time buttons,
so the section reads as a calendar rather than a form. Everything else (service type,
name, phone, vehicle, address) stays hidden until a time is tapped, and the note field
is behind an "Add a note" link. "Change" collapses it back.

Running in **demo mode** with invented availability. To go live, set one value:

```js
API_BASE : 'https://dashboard.boyerscales.com',
SLUG     : 'valleydetails',
```

It then talks to the same contract as `boyerscales-schedule`:

```
GET  {API_BASE}/api/public/schedule/{SLUG}
 -> { hours, booked, slotMinutes, horizonDays, leadHours }

POST {API_BASE}/api/public/schedule/{SLUG}/book
     { name, phone, start, vehicle, mode, address, notes, promo, service }
 -> { ok: true } | { error: "..." }

POST {API_BASE}/api/public/schedule/{SLUG}/vip
     { name, phone, plan, source }
```

`mode` is `"mobile"` or `"dropoff"`. `promo` is `"HALFOFF"` so promo bookings stay
attributable. The server payload overrides the local `HOURS` / `SLOT_MIN` defaults, so
hours stay database-driven in `client_settings` rather than in this file.

---

## Reviews

`CFG.REVIEWS` is empty, so the reviews section is **completely hidden**. No invented
testimonials. Add real ones and it switches itself on:

```js
REVIEWS: [
  { stars:5, text:'their words', name:'Marcus T.', vehicle:'2018 Civic', where:'Google' }
],
GOOGLE_REVIEW_URL: 'https://g.page/r/.../review',
```

At three or more reviews an aggregate "5.0 from N customers" badge appears.

---

## Domain

`valleydetailsca.com` is dropped. Canonical and JSON-LD point at
**valleydetails.site**, with **getvalleydetails.com** worth buying as a redirect.

Free as of Sept 8 2026, checked against the Verisign registry directly (not a
reseller): `valleydetails.site`, `getvalleydetails.com`, `govalleydetails.com`,
`bookvalleydetails.com`, `valleydetailsco.com`, `getvalleydetails.site`,
`detailedbyvalley.com`, `916detail.com`, `sacdetail.com`, `grovedetail.com`,
`grovedetailing.com`, `thegrovedetail.com`.

Taken, so stop looking at them: `valleydetails.com`, `valleydetail.com`,
`valleydetailing.com`, `valleyauto.com`, `valleyautodetail(s|ing).com`,
`valleyautospa.com`, `valleyautoworks.com`, `valleyautocare.com`, `autovalley.com`.

### Planned migration to .com

`valleydetails.site` is the launch domain. `valleydetails.com` is held by a dormant
one-year registration (created 2025-12-30, expires **2026-12-30**, serves nothing).
If it lapses it clears the drop cycle around **March or April 2027**.

Put a backorder on it now (Namecheap, GoDaddy or DropCatch, roughly $20 to $60, only
charged if it actually drops). When it lands:

1. `sed -i '' 's|valleydetails.site|valleydetails.com|g' index.html README.md`
2. Serve a permanent 301 from every `.site` URL to the matching `.com` URL.
3. **Keep `.site` renewed for at least a year after the switch** so links already sent
   out in booking texts do not dead-end.
4. Update the Instagram bio link and the Google Business Profile URL.

### Switching domains generally

Change the `<link rel="canonical">` and the two `url` / `image` fields in the JSON-LD
block. One sed does it:
`sed -i '' 's|valleydetails.site|newdomain.com|g' index.html`

---

## Copy

Deliberately written without em dashes or the usual AI cadence. Plain sentences, short
words, contractions where a person would use them. If you edit copy, keep it that way.

---

## Local preview

```bash
python3 -m http.server 5188
```

Then open <http://localhost:5188>.

---

## Notes

- Mobile first. The promo bar scrolls away on phones instead of pinning, a sticky
  Call / Book bar sits at the bottom, inputs are 16px so iOS does not zoom, and the
  page is pinned to the viewport width with no horizontal scroll.
- No account required anywhere. The Valley Club is purely additive.
- `AutoDetailing` JSON-LD with a 15 mile `GeoCircle` around Elk Grove.
- Dark only by design. Respects `prefers-reduced-motion`.
