# Valley Details website

One self-contained `index.html`. No build step, no dependencies, no framework.
Drop it on any static host (Fly static, Netlify, Vercel, Cloudflare Pages, S3).

Everything configurable lives in the `CFG` object near the bottom of the file.

---

## What is left to do

1. ~~Add the two services on the dashboard.~~ **Done 2026-09-17.** `interior` (90 min,
   $70) and `exterior` (60 min, $60) are live in `booking_config.services`, verified
   against the API. See *Interior only and exterior only* if it ever needs redoing.
2. **Paste the g.page review link** into `CFG.GOOGLE_REVIEW_URL`. Three real reviews
   are already live; this just adds the "leave one" link under them. See *Reviews*.

## Photos

The logo is `assets/logo.png`. Until it exists the header falls back to a built-in
"VD / VALLEY DETAILS" wordmark, so nothing looks broken.

Drop files into `assets/` with these names and they appear immediately. Missing files
render as a labelled placeholder, never a broken image.

| File | Where it shows |
|---|---|
| `hero.jpg` | Behind the headline |
| `svc-full/interior/exterior/express.jpg` | The four pricing menu cards |
| `work1.jpg` … `work6.jpg` | The Recent Work grid, listed in `CFG.SHOTS` |
| `ba1-before.jpg` / `ba1-after.jpg` | Slider 1, interior (optional) |
| `ba2-before.jpg` / `ba2-after.jpg` | Slider 2, exterior (optional) |

The current set was cut from `google-profile-photos/` with `sips`. Card photos are
900px wide, the gallery 1000px, the hero 1600px. Keep new ones in that range: the
whole `assets/` folder is about 2.5MB and should stay there.

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

Four services, three sizes each. **Every number lives in one place: `CFG.SERVICES`.**
The pricing table, the four menu cards and the booking picker are all rendered from
it on load, so changing a price there changes it everywhere. The numbers written into
the HTML are only the no-JS fallback; keep them in step when you edit a price, but
nothing on a real browser reads them.

| Service | `id` | Minutes | Sedan | SUV | Truck / 3-row |
|---|---|---|---|---|---|
| Full Detail | `full` | 120 | $150 | $170 | $190 |
| Interior Only | `interior` | 90 | $85 | $95 | $105 |
| Exterior Only | `exterior` | 60 | $75 | $85 | $95 |
| Express Detail | `express` | 60 | $50 | $60 | $70 |

**Raised on 2026-09-19.** The full detail went 100/115/130 → 150/170/190. Interior and
exterior went up $15 across the board so the arithmetic below still holds. The express
deliberately did **not** move: it is the cheap way in, and the wider the gap between
$50 and $150 the more obvious that is. The menu, the FAQ and the express card all tell
people to book the express when the car only needs a wipe down and a quick wash.

Interior and exterior are deliberately priced so that buying both separately ($160)
costs more than the full detail ($150). That is the honest answer to "should I just
get both", and the page says it out loud under the menu rather than hoping nobody
does the arithmetic. The sentence adds the two numbers up itself, so it cannot go
stale.

Add-ons: shampoo + steam $45, engine bay $35, odor $40, headlights $50, ceramic $85.

Standing discounts:
- **$25 off the second car** at the same address, same stop.
- **Referral, $20 each way.**
- **The 3-Pack, $375 prepaid** for three full details, $125 each against $150.

> Do **not** offer a discount for leaving a Google review. Google prohibits incentivised
> reviews and will filter or penalise them. Ask for reviews, just never pay for them.

---

## Interior only and exterior only

Two things had to change for this to work end to end.

**1. The site.** `interior` and `exterior` are in `CFG.SERVICES`. The pricing menu has
a card each, and the card's "Book This" button scrolls to the calendar with that
service already selected, so picking is the first half of booking rather than
something you do twice.

**2. The dashboard.** The `/api/public/schedule/{slug}` response owns how long a job
holds and what it is worth (`bookingDuration` / `bookingAmount` in the Client Dash
`server.js`), and it only knows the services in `client_settings.booking_config`.
The site used to take the server list verbatim, so anything not on it vanished off
the page; it now **merges** — server fields win, this file's ordering wins, and a
service the server has not been told about still appears and still books.

**This was applied on 2026-09-17** and verified against the live API: `interior`
(90 min, $70) and `exterior` (60 min, $60) now sit in `booking_config.services`
alongside `full`, `express` and `full2`–`full5`. `price` is the sedan price, matching
how `full` is stored as 100.

> **Out of date as of 2026-09-19.** The prices above are what the dashboard was told in
> September; the site has since gone to 150/170/190 for `full`, 85/95/105 for `interior`
> and 75/85/95 for `exterior`. Until `booking_config.services` is updated to match, the
> calendar still books correctly and customers still pay the price on the site, but the
> dashboard's own revenue figures under-report every job. The fix is written and waiting:
> `valley-details-prices-2026-09.sql` in the Client Dash repo. Paste it into the Supabase
> SQL editor and it is done. Jobs already on the calendar keep the amount they were
> booked at, which is what those customers were actually quoted.

The SQL is kept in the Client Dash repo as `valley-details-interior-exterior.sql` and
is safe to re-run. It matters because of what happens without it: the booking still
lands correctly on the calendar and the job still reads *"Interior Only (no exterior)"*
on the schedule, but the server falls back to `slotMinutes` (120) and records the job
at the full detail's $100. The customer never sees that — they pay in person at the
price on the site — but the dashboard's own duration and revenue figures go wrong
quietly, which is the worst way for them to go wrong.

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

Three real Google reviews are live in `CFG.REVIEWS`, quoted word for word. Typos,
double exclamation marks and the stray space before a `!` are all theirs and are all
deliberate: the unevenness is what a real review looks like. Do not tidy them up.

```js
REVIEWS: [
  { stars:5, where:'Google', name:'Jeffrey G.', text:'...' },
],
REVIEW_COUNT: 10,   // what the Google profile actually shows
REVIEW_AVG  : 5.0,
GOOGLE_REVIEW_URL: '',
```

`name` is first name plus last initial, which is the convention here even though the
profile shows full names. `vehicle` is deliberately left off every entry: we do not
know what any of them drove, and filling it in would be inventing.

`REVIEW_COUNT` and `REVIEW_AVG` drive the badge above the cards, so it can read
"5.0 from 10 Google reviews" while only three are quoted. **They are an assertion
about the live Google profile, so keep them matching it.** Set either to 0 and the
badge goes back to counting and averaging the quoted reviews instead.

Picked for spread rather than for being the most flattering: one says he turned up on
time, one says the work was thorough, one says it lasted. Three quotes saying the
same thing reads worse than three saying different things.

Not used: the two from a Boyer and a Randhawa (owner's own circle, and the first thing
a suspicious reader checks), and Jugraj Bains' "Did a good job" (too thin to put on a
page, though he is a Local Guide with 18 reviews if you ever want the badge).

`GOOGLE_REVIEW_URL` is still empty. Profile → "Ask for reviews" → copy the g.page
link → paste it in, and a "Leave a Google review" line appears under the cards.

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
- `AutoDetailing` JSON-LD with a 10 mile `GeoCircle` around Elk Grove.
- Dark only by design. Respects `prefers-reduced-motion`.
