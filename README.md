# Tanu & Dhiraj — Wedding Invitation (Tanu's side)

A premium, cinematic, mobile-first Indian wedding invitation representing
Tanu's family and their ceremonies. This is a separate, independent project
— built from the same design and technical foundation as the original
Dhiraj & Tanu invitation, but with its own content, event schedule and
contact list. The original project is untouched; this is its own complete,
self-contained folder.

## Running it

This is a static site — no build step, no server-side code, no database.

**Locally:** open `index.html` directly in a mobile browser, or in desktop
Chrome/Safari with device-toolbar/responsive mode turned on (it is designed
for phone screens, roughly 320px–430px wide).

**Deploying:** upload the whole folder as-is to any static host (Netlify,
Vercel, GitHub Pages, S3 + CloudFront, or a plain web server) and point it
at `index.html`. No environment variables, API keys or backend are
required — everything the page needs ships inside this folder.

## Project structure

```
index.html    Markup for all 8 pages (scenes) + shared inline SVG artwork
style.css     All visual design: palette, typography, layout, transitions
script.js     Navigation engine, swipe handling, transitions, music, and
              all the generic [data-event]/[data-field]/[data-text]
              rendering that pulls its content from config.js
config.js     ALL editable text: names, dates, times, venues, contacts —
              the only file you should need to touch to update content
assets/       The couple's actual photographs
*.mp3         Background music
```

## Editing content

Open `config.js`. Every name, date, time, venue and phone number the
invitation displays lives there, in plain English and Hindi pairs. The
markup and script read from it generically, so changing a value in
`config.js` is enough — you do not need to touch `index.html` or
`script.js` for a text change.

## The eight pages

1. **Language** — Hindi / English. Choosing one is also the invitation's
   one opening gesture: that same tap starts the background music
   (required by iOS/Safari — audio can only start from a real tap) and
   takes you straight into the invitation.
2. **Couple reveal** — the couple's photograph, then **Tanu** and her
   parents, then **Dhiraj** and his parents. A true full-screen scene: it
   fits every supported phone (320×568 and up) with no scrolling.
3. **Haldi** — Sunday, 29 November 2026, 3:00 PM, at home in Jansath.
4. **Bhoj** — Monday, 30 November 2026, 12:00 PM, at home in Jansath. A
   new page, built on the same restrained single-motif template as Haldi's
   simpler sibling pages, using the invitation's existing diya (lamp) motif
   rather than a literal food illustration.
5. **Mehndi & Sangeet** — Monday, 30 November 2026, at home in Jansath. No
   time was supplied for this event, so none is shown.
6. **Shubh Vivah** — Wednesday, 2 December 2026, 5:00 PM, Tejashvi Palace.
   Date, time, venue, address, the Google Maps link and the Barat
   departure line are all unchanged from the original invitation; only the
   couple-name heading now reads **Tanu & Dhiraj**.
7. **Our Little Stars** — Arnika · Anik.
8. **Final page** — "View Venue" (opens the supplied Google Maps link),
   "Add to Calendar" (downloads a .ics file), and contact numbers for
   Santosh, Tanu and Kuldeep, each a tappable `tel:` link.

Each of the seven page-to-page transitions reuses one of the original
invitation's existing cinematic choreographies — nothing new was designed
for the transitions, only repointed to the new running order (Manas Path
has been removed; the page order is now language → couple → Haldi → Bhoj →
Mehndi & Sangeet → Shubh Vivah → stars → final).

## Notes on what stayed the same as the original project

- **Swipe = command.** A vertical drag of at least 90px, with the vertical
  movement at least 1.5× the horizontal, is read as one navigation command.
  The page itself never moves, drags or rubber-bands under your finger;
  only a full-screen transition overlay animates, and it does so above the
  stationary current and destination scenes. Pages never skip, and input
  is locked while a transition is running.
- **Tall pages** (ones whose content is taller than the screen) scroll
  their own content internally; the page frame itself still never moves.
  The couple-reveal page is the one page that is guaranteed non-scrollable
  at every supported size.
- **Music** is a single persistent, looping track. It starts once, from
  the language-selection tap, and is never restarted by navigation. Mute
  pauses the visual icon state only — the track keeps playing in the
  background, so position is never lost.
- Real couple photographs are used throughout — nothing here is
  AI-generated or stock imagery.

## Testing performed

Automated touch-event tests (Playwright, mobile Chromium) were run at
320×568, 375×667, 390×844 and 430×932, in both English and Hindi,
covering: the direct language → couple opening with no intermediate
screen; full forward and backward navigation through all 8 pages; that no
scene ever visibly moves during a swipe; rejection of too-short, diagonal
and horizontal gestures; the couple-reveal page's fit and non-scrolling
behaviour; Haldi/Bhoj/Mehndi content and dates; the Tanu-first name order
on both the couple-reveal and Shubh Vivah pages; the two children's names
on the stars page; the three contacts and their `tel:` links in the
correct order; the Maps and Calendar buttons; and continuous, never-
restarted music playback across every transition.

This was not tested on a physical phone — only in a simulated mobile
browser environment — so real-device touch feel, haptics, and iOS
Safari's specific audio/viewport quirks are unverified.
