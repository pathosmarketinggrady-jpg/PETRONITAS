# Petronita's, Southern & Latin Cuisine

A four-page static website for Petronita's, 1349 N. Market Street, Jacksonville FL 32206.

Plain HTML, CSS and JavaScript. No framework, no build step to view it, no CDN,
no web fonts. Extract the folder and double-click `index.html` and the whole
site works, styled, with no internet connection.

---

## The pages

| File | What it is |
|---|---|
| `index.html` | Home. Hero, the story in short, the five courses, brunch and bar, visit block |
| `menu.html` | Every menu, with a live filter and search across all 25 items |
| `story.html` | Petronita, how the kitchen works, the neighbourhood |
| `visit.html` | Address, hours, and the reservation panel |

Plus `css/styles.css`, `js/menu.js`, `js/main.js`, and `assets/`.

---

## Editing the menu

**`js/menu.js` is the single source of truth.** Every course, description,
price, the address, the phone number and the opening hours are written there
once and read everywhere else: the header, all four footers, the open/closed
dot and the structured data Google reads.

The menu is also written into `menu.html` and `index.html` as real markup, so
the full menu is readable with JavaScript switched off, indexable by search
engines, and printable. That means after editing `js/menu.js` you update the
matching block in the HTML between its marker comments:

- `<!-- GROUPS:START -->` … `<!-- GROUPS:END -->` in `menu.html`
- `<!-- COURSES:START -->` … `<!-- COURSES:END -->` in `index.html`
- `<!-- CHIPS:START -->`, `<!-- HOURS:START -->`, `<!-- HOURSTABLE:START -->`

Keep the two in step. If a price changes in one and not the other, the page
shows one number and the filter counts another.

### Adding an item

Copy an existing `<article class="mitem">` block. The one thing not to forget
is `data-hay`, the lowercase, accent-free, punctuation-free string the search
box matches against. "jalapeño" is written `jalapeno` there so that typing
either spelling finds it.

---

## What the JavaScript does, and does not do

`js/main.js` is progressive enhancement only. Switch JavaScript off and you
lose the filter, the animation and the live open/closed dot. You lose no
content. It adds:

- the sticky header that shrinks on scroll
- the mobile menu sheet, which closes on Escape, on a click outside, and on any link
- the open/closed dot, computed against the visitor's own clock, rechecked every minute
- today's row highlighted in the hours table
- the menu filter, chips and search
- the marquee under the hero
- the fade-in on scroll

Anything already on screen when the page loads is shown immediately rather than
waiting on the scroll observer. That is deliberate: an observer callback is
asynchronous, and if it does not fire, an element sits invisible forever. Above
the fold that is not a risk worth taking.

---

## Reservations, and a fee worth knowing about

Booking goes to OpenTable, using the address published on the current
Petronita's site:

```
https://www.opentable.com/r/petronitas-jacksonville
```

**That is a plain profile link.** It carries no `restref` and no `ot_source`.
OpenTable treats a booking arriving that way as network-sourced and bills the
restaurant a cover fee per person, even though the restaurant's own website
sent the guest.

A restaurant's **direct** link, the one in their OpenTable dashboard, looks
like this and is free of that fee:

```
https://www.opentable.com/r/petronitas-jacksonville?restref=XXXXXXX&lang=en-US&ot_source=Restaurant%20website
```

If Petronita's can pull that link, put it in `js/menu.js` under
`SHOP.opentable.url`, update the button in `visit.html`, and set
`direct: true`. Same bookings, no fee. Worth a five-minute conversation.

---

## Things on the current site that do not agree with each other

All content here was taken from petronitas.com and from the five PDF menus
linked on its menu page. Nothing was invented. Where the sources conflicted,
the choice made is recorded below so it can be checked rather than trusted.

**1. The homepage dishes are not on any menu.**
petronitas.com currently shows four "Signature" dishes with prices: Camarón al
Ajillo $32, Braised Short Rib $48, Hot Honey Chicken $36, Tres Leches d'Or $18
with 24k gold leaf. **None of those appear on any of the five PDF menus.** The
actual dinner service is a fixed five-course tasting menu with no per-dish
prices at all. Braised Short Rib is real, but it is served with celery root
purée and Aleppo pepper demi glace, not the sweet corn polenta the homepage
describes. This site uses the PDFs. **Please confirm which is current.**

**2. The phone number.**
The contact page lists **(713) 555-0193**. That cannot be right: 555-01xx is
the block reserved for fiction, and 713 is Houston. The same page also lists
**(904) 465-6568**. This site uses the 904 number throughout. **Please confirm.**

**3. A stray email address.**
The contact page also shows `info@covesocial.com`, which belongs to a different
business and looks like leftover template content. It is not used here. Only
`reserve@petronitas.com` is.

**4. Closing time.**
The homepage says dinner runs **6 PM to 11 PM**. The contact page says **6:00 PM
to 1:00 AM**. This site uses 6 to 11, which is what the homepage and the visit
block both say. If 1 AM is right, change `SHOP.hours` in `js/menu.js` (write a
past-midnight close as `25`, not `1`, so the open/closed dot still works) and
the three `HOURS` blocks.

**5. Small corrections made to the PDFs.**
"Naval Orange" is written navel, and "chantarelle" is written chanterelle. The
brunch PDF renders "Waffles" with a broken ligature; it is spelled properly
here. Nothing else was touched.

---

## Photography

The site ships with one photograph, the chef at the pass, taken from the
current Petronita's site, plus the logo. It is used twice, cropped two ways.

This is the site's one real weakness and it is not something code can fix. A
restaurant this good-looking should have eight to twelve photographs: the room
under candlelight, three or four plated courses, the bar, a cocktail, the
exterior at night. The layout already has places for them. If a shoot happens,
the slots are `.story__fig` on the home and story pages, and a gallery can be
dropped straight into the home page.

---

## Publishing

There is no build step, so any static host works. `vercel.json` is included and
sets long cache headers on `assets/` and shorter ones on `css/` and `js/`.

For Vercel: put these files at the **root** of the repository, so `index.html`
is at the top level rather than inside a folder. Import the repo at
vercel.com/new and deploy. Nothing needs configuring.

---

## Browser support

Tested in Chrome. Uses CSS grid, custom properties, `clamp()`,
`aspect-ratio` and `IntersectionObserver`, all of which have been standard for
years. Older browsers lose the layout polish and the fade-in, not the content.

Accessibility: skip link, visible focus rings, labelled icon buttons, the
mobile sheet is keyboard-operable and closes on Escape, the filter count is a
live region, decorative marks are hidden from screen readers, and the vegan
marker is a readable word rather than an emoji. It honours
`prefers-reduced-motion`. It also prints: the header, filter bar and buttons
drop away and the menu prints on white.
