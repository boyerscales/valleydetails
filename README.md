# Valley Details website

One self-contained `index.html`. No build step, no dependencies, no framework.
Drop it on any static host (Fly static, Netlify, Vercel, Cloudflare Pages, S3).

Everything configurable lives in the `CFG` object near the bottom of the file.

> **Redesign, 2026-09-27.** The page was rebuilt to look premium ahead of raising the
> full detail toward $250. Some sections below describe the old page (the menu in
> *Pricing* below is the current one). What changed:
>
> - **Prices are written into the markup by hand** (hero note, Full Detail price row,
>   Other Services rows, FAQ, structured data). `CFG.SERVICES` now only drives the
>   booking picker. Change a price in both places.
> - **Photos live in `assets/v2/`**, each in a phone size and a large size
>   (`name-800.jpg`, `name-1200.jpg` or larger), wired up with `srcset`. `CFG.SHOTS`,
>   `CFG.BA`, the promo countdown, the 3-Pack and the reviews config are gone; the
>   reviews and the work photos are plain HTML.
> - **Camera originals go in `assets/originals/`** (gitignored). Cut web sizes from
>   them with `sips -Z <width> -s formatOptions 72`.
> - Booking, referrals and the dashboard API are unchanged.

---

## What is left to do

1. **Update the dashboard's `booking_config.services`** to the menu in *Pricing* below.
2. **Paste the g.page review link** into `CFG.GOOGLE_REVIEW_URL`. Real reviews
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

Menu live 2026-11-15 (it replaced the one-day two-tier menu of 2026-11-14). Five services, three sizes each, all booked on the page. The names on the page match the dashboard's service ids. The business does not do ceramic, sealant, paint correction or PPF.


| Service | `id` | Two-car `id` | Time | Sedan | SUV | Truck / 3-row |
|---|---|---|---|---|---|---|
| Express (quick clean, inside and out) | `express` | `express2` | about 1 hour | $60 | $70 | $80 |
| Full (inside and out) | `full` | `full2` | about 2 hours | $150 | $170 | $190 |
| Showroom (Full plus the deep interior) | `showroom` | `showroom2` | about 3.5 to 4 hours | $250 | $280 | $310 |
| Interior Only (the inside of the Full) | `interior` | `interior2` | about 90 minutes | $100 | $110 | $120 |
| Exterior Only (the outside of the Full) | `exterior` | `exterior2` | about 1 hour | $75 | $85 | $95 |

The Showroom's deep interior: carpets, mats and cloth seats shampooed with a hot water
extractor; steam on vents, cupholders, console, seat rails and door pockets; set-in
stains worked on.

Add-ons: heavy pet hair $40. Seat or carpet shampoo on a Full or Interior Only $50.

Standing discounts:
- **$25 off the second car** at the same address, on a Full or Showroom. No second-car discount on Express, Interior Only or Exterior Only.
- **$5 off** for using the customer's outside spigot and outdoor outlet.
- **Referral, $20 each way.**

There is no quote sheet any more: the $60 Express took its place and books like the
others. Interior Only and Exterior Only are back on the page as bookable rows next to
Express (2026-11-15). A thank-you gift link takes money off a Full or Showroom only.

Policies: no deposit, pay when the car is done (cash, Zelle, Cash App, Venmo, Apple Pay
or card). Rescheduling is free. Canceling within 12 hours of the appointment is a $20
fee. Travel is free within about 10 miles of Elk Grove, then $1 a mile over ten. Hours
8am to 8pm every day. The customer-facing wording lives in `policy.html`.

> The dashboard's `booking_config.services` (the durations and amounts the booking API
> returns) was changed to this menu on 2026-11-15. Its `full3` to `full5` entries are
> for jobs the owner enters by hand.

> Do **not** offer a discount for leaving a Google review. Google prohibits incentivised
> reviews and will filter or penalise them. Ask for reviews, just never pay for them.

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
     { name, phone, start, vehicle, mode, address, notes, promo, utm, service }
 -> { ok: true } | { error: "..." }

POST {API_BASE}/api/public/schedule/{SLUG}/vip
     { name, phone, plan, source }
```

`mode` is `"mobile"` or `"dropoff"`. `promo` is the code from a promo link (see
*Promo links* below), or empty, so promo bookings stay attributable. `utm` is
`{ source, medium, campaign, content }` from the `utm_*` tags on the link the visitor
arrived on, remembered for 7 days (Meta's click window), or left out. The dashboard
shows it on the job as `Came from: meta / paid / launch / e-from-50`, so ad bookings
can be told apart from everything else. Unlike `ref` and `promo`, the tags are left in
the address bar, because the Meta Pixel reads its click ID from there. Any other link
can be tagged the same way, e.g. the Instagram bio link with
`?utm_source=instagram&utm_medium=bio`. The server payload overrides the local `HOURS` / `SLOT_MIN` defaults, so
hours stay database-driven in `client_settings` rather than in this file.

---

## Promo links

A link like `valleydetails.site/?promo=story` tags the booking. The page remembers the
code for 14 days, strips it from the address bar, shows the banner from `CFG.PROMOS`
above the booking form, and sends the code with the booking. The dashboard then shows
`Promo: story` on the appointment, and the discount is taken off in person at payment.
Nobody who types the address in by hand gets the tag.

Each entry in `CFG.PROMOS` has a `banner` (above the form), a `booked` line (in the
confirmation) and an `ends` date. The code is good through that day; after it, the
banner stops and the code is no longer sent, so a stale link books at the normal price.
Delete the entry once the deal is over. A code not in the list is still sent, it just
shows no banner.

The `story` code (the Instagram story deal) ended 2026-10-04.

## Reviews

A few real Google reviews are live in `CFG.REVIEWS`, quoted word for word. Typos,
double exclamation marks and the stray space before a `!` are all theirs and are all
deliberate: the unevenness is what a real review looks like. Do not tidy them up.

```js
REVIEWS: [
  { stars:5, where:'Google', name:'Jeffrey G.', text:'...' },
],
REVIEW_COUNT: 0,    // the owner does not want a count shown
REVIEW_AVG  : 5.0,
GOOGLE_REVIEW_URL: 'https://g.page/r/CVoDC0vKzixlEBM/review',
```

`name` is first name plus last initial, which is the convention here even though the
profile shows full names. `vehicle` is deliberately left off every entry: we do not
know what any of them drove, and filling it in would be inventing.

The owner does not want the number of Google reviews shown anywhere. Say "5.0 on Google"
with no count. Keep any count out of the badge, the copy and the structured data.

Picked for spread rather than for being the most flattering: one says he turned up on
time, one says the work was thorough, one says it lasted. Quotes saying the same
thing read worse than quotes saying different things.

Not used: the two from a Boyer and a Randhawa (owner's own circle, and the first thing
a suspicious reader checks), and Jugraj Bains' "Did a good job" (too thin to put on a
page, though he is a Local Guide if you ever want the badge).

`GOOGLE_REVIEW_URL` is set as of 2026-09-19, to the same g.page link the dashboard
already had in `client_settings.google_review_link`. A "Leave a Google review" line
now shows under the cards. If it ever needs changing, it lives in two places: here
and that column, which is what the post-job review text sends.

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
- No account required anywhere.
- `AutoDetailing` JSON-LD with a 10 mile `GeoCircle` around Elk Grove.
- Dark only by design. Respects `prefers-reduced-motion`.
