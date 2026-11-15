# Valley Details site concepts: shared brief

You are building ONE design concept for a redesign of valleydetails.site, a real mobile car detailing business in Elk Grove, California. The owner (Elijah) will open it on his iPhone and compare it against two other concepts built by other people from this same brief. He wants the site to stand out from other detailers and, above all, to make more money (more bookings, bigger tickets). Your concept direction is given in your task message. Everything in this file is shared by all three concepts.

## Output
- One self-contained page: `index.html` in your concept folder, with inline CSS and JS. No build step, no frameworks, no CDN scripts. Google Fonts via a `<link>` is fine.
- Images come from the shared pack at `../assets/` (relative path, see `../assets/MANIFEST.txt` for every file and its pixel size). Do not add images from anywhere else. Do not edit the shared assets folder; if you need a derived image (a crop, a blur), write it into your own concept folder.
- Add `<meta name="robots" content="noindex">`. This is a preview, not the live site.
- Only write inside your own concept folder. Never touch git, never deploy, never contact anyone.

## Phone first
- Design for 375x812 first, then make sure 1280 wide is not broken. Most traffic is Instagram and Facebook ads on phones.
- `document.documentElement.scrollWidth` must equal 375 at every scroll position. Set overflow-x hidden on html and body and clip decorative overflow.
- Motion must work on scroll and touch, never hover only. Respect prefers-reduced-motion.
- Nothing linear: entrances ease out (arrive fast, settle slow), exits ease in, interface elements use a spring with a small overshoot.
- Keep it fast: lazy-load images below the fold, give every img width and height, never blur a full-screen layer live on the phone (use the pre-blurred files in `../assets/bg/`).

## The one-accent rule (hard rule)
- Valley red `#e0161e` appears ONLY on the Book buttons (the button you want pressed) and inside the logo image. Not on dots, rules, icons, chips, selected states, prices, links, stars, tints or borders.
- Everything else is the two base colors of your concept plus neutral greys.
- One Book button visible per phone screen where possible. A sticky bottom bar with Call (neutral) and Book (red) is expected once the hero button scrolls away, and it should hide while the booking section is on screen.

## Writing rules (hard rules, the owner spots violations instantly)
- No em dashes and no en dashes anywhere a person can read. Use commas, periods, or "to" for ranges.
- Short plain sentences, sentence case. No "it's not just X, it's Y", no rule-of-three rhythm, no inflated adjectives, no marketing speak, no hype caps, no exclamation marks.
- Never invent a fact, a stat, a testimonial, a name, a date or a photo caption. Use only what is in this file. If you need something that is not here, leave it out.
- No bracketed placeholders.

## Design tells to avoid (he calls these "AI-made")
- Glowing or pulsing status dots, eyebrow caps over every section, sparkle or star marquees, glass pills, a serif-italic accent word in a headline, a gradient over every photo, the near-black plus cream "AI theme" look, gold on black.
- Flat dead backgrounds. He wants the page to feel alive.
- Decorative visuals that carry no information (random icons, squiggles, stock-style illustrations, icons that repeat their label). Every visual must be a real photo of their work or carry information.
- Generic stock structure: hero, three icon cards, testimonial carousel, FAQ. Find a structure that comes from this business.
- It must look like THIS business: real driveways, real cars, the real logo, their own words.

## Brand
- Logo: `../assets/logo-900.png` and `logo-420.png` (transparent). Chrome and white wordmark "VALLEY", red spaced "DETAILS", snowy mountain peaks over a white car outline with a red roof line, tagline "CLEANER CARS · BRIGHTER DAYS" inside the image. It is made for dark backgrounds: the chrome and white parts disappear on a light background, so always place it on a dark surface.
- Logo fonts: Saira Expanded (Black for VALLEY, Bold for DETAILS), Montserrat for the tagline. You may use Saira or Saira Expanded for headings to match, or pick another font if your direction calls for it. One or two font families, no more.
- Tagline you may use as a line of copy: "Cleaner cars. Brighter days."

## True facts (use these, word them plainly)
- Valley Details. Mobile car detailing, Elk Grove, California. Owner-operated since 2024. The people who run it are the ones doing your car.
- We come to you: your driveway, apartment lot or work. Elk Grove and south Sacramento. No travel charge within about 10 miles of Elk Grove. Past 10 miles it is $1 a mile for the miles over ten.
- We bring the equipment. We need a parking spot, an outside spigot and an outdoor outlet, and we take $5 off for using them.
- No deposit. You look the car over when we are done, then you pay: cash, Zelle, Cash App, Venmo, Apple Pay or card.
- 5.0 on Google from 15 reviews. Google link: https://g.page/r/CVoDC0vKzixlEBM
- Real review quotes (verbatim, do not edit, do not add others):
  - "Much needed and well appreciated and fast service !! Quality service and lasting detail !" Jeffrey G., Google
  - "Arrived on time friendly faces and great service" Mason A., Google
  - "Professional Service, cleaned everything well." Jose P., Google
- Phone: (916) 249-0631, `tel:+19162490631`. Text: `sms:+19167537822`. Instagram: https://www.instagram.com/valleydetailsca/
- Open 8am to 8pm every day.
- Two cars at the same address: $25 off the second car.
- Rescheduling is free. Canceling within 12 hours of your appointment is a $20 fee.
- Rain: if the forecast looks bad we text you the day before and move you to the next open slot for free.
- They do NOT wax, and do not do ceramic coating, sealant, paint correction, PPF or odor machines. Never mention any of those as included. They do have a hot water carpet extractor and a steamer.

## The offer (new structure, this is the main content change)
Put every price in one `CFG` object at the top of your script so prices can be changed in one place. Two tiers, shown side by side or one after the other, plus one quiet line for people who want less.

**Express** (inside and out, about 2 hours): sedan $150, SUV $170, truck or 3-row $190.
- Inside: vacuumed from the seats to the trunk. Pet hair and grit lifted out. Leather cleaned and conditioned. Dash, console, doors and vents wiped down. Glass and mirrors.
- Outside: hand washed from the roof to the rockers. Wheels, wells and tires degreased. Bugs and tar taken off. Tires dressed. Glass, trim and door jambs.

**Full** (everything in Express, plus the deep interior, about 3.5 to 4 hours): sedan $250, SUV $280, truck or 3-row $310.
- Carpets, mats and cloth seats shampooed with a hot water extractor.
- Steam on the vents, cupholders, console, seat rails and door pockets.
- Set-in stains worked on. If one will not come out, we tell you before we start.
- This is the tier you want people to choose when their interior is actually dirty. The stained Camry seats and the Tesla and Kia carpets in the photo pack are the proof for it.

**Need something less?** A quiet secondary line, never competing with the two tiers: "Need something less? Get a quick price." It opens a small instant picker (inside only or outside only, then vehicle size) that shows the price right away and leads into booking. Inside only: $100 / $110 / $120. Outside only: $75 / $85 / $95. No form to wait on, no "we will get back to you".

**Add-ons** (offered inside the booking flow, not as a separate section): heavy pet hair $40. Seat or carpet shampoo on an Express $50.

There is no $50 Express any more and no "Showroom" name on the page.

## Proof (the photo pack is the best thing this business has)
All photos are their own jobs, shot on phones in customers' driveways, plates already blurred. Captions below are the only facts you may state about them.
- Before and after pairs with matching framing (good for a drag slider): `jobs/pacifica/02.jpg` then `05.jpg` (2023 Chrysler Pacifica cargo floor, crumbs, then vacuumed). `jobs/rav4/04.jpg` then `05.jpg` (2016 Toyota RAV4 cargo area). `jobs/rav4/02.jpg` then `03.jpg` (RAV4 wheel). `work/merc-trunk-before.jpg` then `merc-trunk-after.jpg` (Mercedes trunk lid, water spots). `jobs/tesla-model-y/01.jpg` then `04.jpg` (Tesla Model Y, hair worked into the carpet, then cleared).
- Before and after pairs with different framing (use side by side or in sequence, not a slider): `jobs/camry/01,02,03.jpg` before and `04,05,06.jpg` after (2013 Toyota Camry, stains set into cloth seats front and back, then cleaned). `jobs/kia-soul/01 to 04` before and `05 to 08` after (2018 Kia Soul interior, dirt and hair ground into the mats and seats). `work/cruze-floor-before.jpg` and `-after.jpg`, `work/merc-floor-before.jpg` and `-after.jpg`, `work/tesla-sap-before.jpg` and `-after.jpg` (tree sap on a Tesla).
- Finished cars: `jobs/bmw-5-series/*` (white BMW 530e, tan leather, all after). `jobs/cadillac/*` (1995 Cadillac DeVille, exterior only, wire wheels). `jobs/pacifica/01.jpg`, `jobs/rav4/01.jpg`. In `work/`: tesla-white, tesla-rear, tesla-int, scat-front, scat-side, scat-wheel (blue Dodge Charger Scat Pack), mustang-driveway, mustang-rear, mustang-int, eclass, merc-wheel, lexus-dusk, lexus-front, lexus-gx, bmw-front, bmw-tail, bmw540, audi, taycan, altima-night, pathfinder.
- Pre-blurred 720px backdrops in `bg/`.
- Look at the actual images before placing them (open them). Do not put a "before" photo where it reads as finished work. Label before and after plainly.
- There are no photos of the crew and no video in the pack. Do not fake either. Do not use any AI or stock imagery.

## Booking section (a working mock, never a real submit)
The live site's booking flow is the thing that makes money, and this redesign also has to fix why people cancel. Build the booking section as an interactive front-end mock in your concept's style:
1. Pick a tier (Express or Full), or arrive from the "something less" picker.
2. Pick vehicle size (sedan, SUV, truck or 3-row). The price updates everywhere.
3. Pick a day (show the next 7 days as chips) and a time (8 AM to 6 PM on the hour). Times are sample data.
4. Where: "We come to you" or "I'll drop it off" (drop-off is 9280 Trout Way, Elk Grove).
5. New required question: "Is there an outside spigot and an outdoor outlet we can use?" with Yes, Not sure, No. If Not sure or No, show: "No problem. We will text you before we drive over to sort it out."
6. Add-ons as two optional toggles.
7. Name, mobile, vehicle (year and model), and an optional opt-in checkbox "Text me open-slot deals. Reply STOP anytime." unchecked by default.
8. A summary line with the service, size, day, time and total, then the red Book button.
9. Under the button, plain text: "No deposit. You pay when the car is done. Rescheduling is free. Canceling within 12 hours of your appointment is a $20 fee." and "After you book, text us a few pics of your car if you can. If it needs extra time, we tell you the time and price before we come out."
10. Pressing the final Book button must NOT send anything anywhere. Show an inline message: "This is a design preview. Booking is live at valleydetails.site." No network requests at all from the page except fonts and the images.

`../LIVE-index-reference.html` is the current live site's source. Read it for how the current booking flow, sticky bar and FAQ are worded. Reuse its plain wording where it is still true, but do not copy its design, and do not copy its prices, its Express, its wax lines or its "no fee" cancel wording (those are out of date).

## Sections every concept needs (order and form are yours)
Header with the logo and a small Book button. A first screen that makes someone from an ad want to keep scrolling and shows the price. The two tiers with what is in each. Before and after proof. Reviews with the Google rating. How it works (pick a time, we pull up, look it over then pay). The booking section. A short FAQ (5 or 6 questions, from the facts above). Footer with phone, text, Instagram, service area and hours.

## Self-check before you finish (run these, do not skip)
A Node script with playwright-core is available: `require('C:/Users/elija/AppData/Local/Temp/claude/98e6c1dee5240e94/32980476-8a86-412a-8989-dc28d3d22381/scratchpad/shots/node_modules/playwright-core')`, launch chromium with `executablePath: 'C:/Program Files/Google/Chrome/Application/chrome.exe', headless: true`. Open your page by file URL. Write your scripts with the Write tool into your concept folder (bash heredocs on this machine strip backslashes).
1. At 375x812 (isMobile, hasTouch, deviceScaleFactor 2): scroll the whole page in steps, confirm scrollWidth is 375 at each step, no console or page errors, no failed image requests.
2. Take screenshots of every screen of the page and LOOK at them. Fix what looks broken, cramped, overlapping, unreadable or generic. Check text over photos is readable.
3. Scan computed styles (including pseudo-elements, borders, SVG fills) and confirm red `rgb(224, 22, 30)` is only on Book buttons.
4. Search the visible text for the em dash and en dash characters and for the words wax, ceramic, sealant, showroom. None may appear.
5. Walk the booking mock: pick each tier and size, confirm the total is right, confirm the final button sends no network request.
6. Check 1280x800 once for obvious breakage.

## Final report (short)
Say what you built in 5 lines or fewer, what makes this concept different from a normal detailer site, anything from this brief you could not do, and the result of each self-check with numbers. Save 3 phone screenshots a person would judge the concept by (first screen, tiers, proof) as `shot-1.png`, `shot-2.png`, `shot-3.png` in your concept folder.
