# DOTMENT Control Center — visual redesign prompt

You are redesigning the look of an existing, working one-page web app. The app is finished technically. Its look is not. Your job is to give it a premium, elegant, corporate visual identity that feels comfortable to the eye, with the calm spacing and restraint of an Apple product page, while keeping every function exactly as it works today. What each control does is fixed. What it looks like, and to a degree what it is called, is yours to decide.

## The app

- Repo: `/home/belal/Desktop/dotment-control-center/`
- One file does everything: `index.html`. Vanilla HTML, CSS and JS. No framework, no build step, no CDN, no external requests. Keep it that way.
- Assets in `assets/`: the licensed Gilroy family and IBM Plex Mono in `assets/fonts/`, brand logos in `assets/logos/`, the favicon files. Keep all paths relative.
- Three screens: a password gate, a Brands overview, and a Manage screen with four tabs (Overview, Segments, Access, Branding).
- State lives in the browser: one `localStorage` key for the data, a cookie plus a storage flag for sign-in. Everything the user changes survives a refresh. The seed loads on first visit only.
- Run it with `python3 -m http.server` and test over http, never `file://`.

## Sources you must read, and one you must not

1. **`/home/belal/Desktop/dotment/incircle-application/dotment/DOTMENT Foundations.html`** is the design system. It is a Claude Design bundle: the page markup is a JSON string inside `<script type="__bundler/template">`, and fonts and images are base64 inside `<script type="__bundler/manifest">`. Do two things with it, in this order:
   - **Render it and look at it.** Open it in a headless browser, take a full-page screenshot, crop every one of the 36 boards (F1 to F36) into its own image, and view each one. Do not skip this. Reading the text alone produces a page that follows the tokens but misses the compositions, which is exactly what went wrong last time.
   - Then parse the template text for the exact values: hexes, type sizes, radii, spacing, motion curve and durations, component recipes.
2. **The current `index.html`.** Serve it, sign in (password is the `PASSWORD` constant at the top of the script), and screenshot every screen, every tab, the wrong-password state, a flipped toggle, the toast, and the signed-out state. You need this only to build the inventory in step 1 below. You are not keeping its look.
3. **Do not open or use `DOTMENT Platform Deck.html`** or any other deck for design. Not for layout, not for reference, not for mood. The Foundations file and this brief are the only design inputs.

## Step 1 — strip the identity, keep the substance

Before you design anything, produce a written inventory of the app as it is and save it as `INVENTORY.md` in the repo. It must list, screen by screen:

- Every heading, label, line of copy, and caption, verbatim.
- Every button, link, tab, select, input, toggle and preset, with what it does when used.
- Every data value shown and where it comes from in the seed.
- Every behaviour: what saves, what shows a toast, what persists on refresh, what opens in a new tab, what the suspend toggle changes elsewhere, how the segment counter and group toggles update, how sign-in and sign-out work.
- The data model, storage keys, cookie name, seed version, and the sync of static fields from the seed.

Then remove the current visual identity completely. Delete every style. Keep the HTML structure only where it carries content or hooks the JavaScript uses (ids, `data-` attributes, classes the script reads). Keep the JavaScript's behaviour intact. You may restructure markup and rename classes as your design needs, as long as every function in the inventory still exists and still works. This applies to all screens including the gate. Start visually from a blank page.

**Controls are functions, not shapes.** When the inventory says "button", "toggle", "select" or "tab", it records what the thing does, not how it must look. A button may become a text link, a row you click, a card that is itself the action, an icon with a label, or anything else that reads clearly and fits the design. A toggle may become a different kind of switch. Tabs may become a segmented control, a side rail, or a scrolling page with anchors. Two rules hold: the function must be reachable and obvious, and the same function must look the same everywhere it appears.

**Names may move too.** The labels in the inventory are the current wording, not scripture. Keep the meaning, but rename anything if a shorter, warmer or more precise label fits the design better. `SIGN IN AS BRAND` can become `Open their workspace`, `MANAGE` can become `Manage` or a simple arrow, `FULL LIBRARY` can become `Everything`. Keep the DOTMENT voice: sentence case, short, human, ends with a period where it is a sentence. Do not rename things that are data, such as brand names, segment names, plan names, tier names, emails and links.

## Step 2 — the design direction

Get creative. This is the part where you have freedom. The constraints below are the frame, not the picture.

**Feel.** Premium, elegant, corporate. Quiet confidence. Comfortable to look at for a long time. Think of the spacing, hierarchy and restraint of Apple's product and account pages: large calm type, generous whitespace, few lines, few borders, soft rounded surfaces, everything aligned to a grid. Nothing loud, nothing cluttered, nothing that looks like a generic admin dashboard.

**Colour.** Brighter than the current build. Grey is not the theme and must not be the canvas. Build on white and light, airy surfaces. The four DOTMENT hexes stay sacred: `#000000`, `#FFFFFF`, `#E3E4E5` (allowed only as a quiet hairline or subtle surface, never the page background), and Dot Cyan `#75E2FF`, used exactly as is and never tinted or shaded. Cyan is the single accent: dots, periods, the answer, the primary action on dark. Muted ink and hairlines come from Foundations F1 and F27. No generic UI blue, no red, no new brand hues. Error and the one destructive confirm use `#EB937E`.

**Gradients.** Wanted, and permitted by Foundations F35, which the owner added for exactly this. Use its named backgrounds (Paper Fade, Cyan Dawn, Cyan Air, Deep Field, Night Glow, Flood Fade) or compose new ones under the same law: backgrounds only, built from the existing anchors (`#FFFFFF`, `#E3E4E5`, `#000000`, `#75E2FF` and its alpha), no new hues, cyan alpha capped at 45 percent on light and 30 percent on dark, one gradient per composition, type on the calm end. Make them move, slowly. Ambient loops of 6 to 12 seconds with the brand ease, desynced, so the page breathes and relaxes rather than animates. Text, pills, buttons, logos, icons and the dot itself stay flat on top. Respect `prefers-reduced-motion` by freezing the loop and keeping only opacity fades.

**Type.** Gilroy, from `assets/fonts/`, nothing else for text. IBM Plex Mono for values, times, emails and codes. Use the Foundations scale and weights (F3) but let the hierarchy be bold: one big statement per screen, then quiet. Sentence case headlines with a cyan period. Tracked light caps for eyebrows. Warm, short copy that ends with a period.

**The dot.** Every layout earns one circle (F10 to F12). One Dynamic Dot owns each composition: a Hero Dot cropping an edge, a Halftone Burst, a ringed Face Dot, or the Period Dot. Never two competing heroes. Use it to make each screen recognisably DOTMENT without adding noise.

**Surfaces and depth.** Flat, or nearly so. Cards with radius 24, pills and dots at 999, inputs at 12. No card borders on tinted surfaces, a single hairline when white sits on white. Shadows only for floating overlays. Hover swaps surfaces or reveals a cyan dot, it never lifts. No glass, no blur, no glow.

**Spacing.** The 8 base scale from F5, but err generous: 24 inside cards, 32 between components, 96 to 128 for sections, 128 for a hero to breathe. Content column at 1200 max, 12 columns, cards snapping to 3, 4 or 6.

**Motion and transitions.** Seamless everywhere, but never too much. One curve for the whole brand: `cubic-bezier(0.45, 0, 0.15, 1)`. UI response 150 to 200ms, screen and tab changes 400 to 600ms, hero moments up to 900ms. Stagger list and card entries at 60ms per item, maximum six items. Screens cross-fade into each other, tabs cross-fade their panels, toasts slide in and settle. Nothing bounces, nothing springs, nothing spins, no confetti, no parallax.

**Components.** Follow the Foundations recipes for buttons (F18), inputs and toggles (F19), chips and pills (F20), cards (F21), tables and key-value rows (F22), navigation and footer (F23), overlays and toast (F24), stats and progress (F25). You may push them toward the softer, airier feel this brief asks for, but a reader of the Foundations board should recognise every one of them.

## Step 3 — what to keep unchanged

- Every behaviour in the inventory. Content may move, merge, or change shape, and controls may take any form and any wording that fits, as long as the function survives and stays easy to find. When in doubt about a function, keep it.
- The password constant, the seed data, the storage keys, the seed version and the static-field sync. Brand names, segment names and descriptions, plan and tier names, team names and emails, and links are data and stay as they are. A browser that already holds data must keep its toggles and settings after your change.
- Brand logos shown as they are, without a circle plate around them.
- The favicon, the page title, the account email in the header, the brand links.
- No text on screen that says demo, prototype, placeholder, lorem or TODO. It must look deployed.
- No framework, no CDN, no build, no external requests, relative asset paths, no console errors or warnings.

## Step 4 — verify, then report

Serve over http in a headless browser and walk it: gate, wrong password, right password, Brands, open a brand, every tab, flip a toggle, reload and confirm the toggle and the counter held, change a limit and reload, suspend and unsuspend, sign out, reload and confirm the gate. Screenshot every screen and every tab at 1440 wide and at a phone width. Confirm zero console errors.

Commit the work in the repo with a clear message. Do not deploy.

Report back with: the screenshots, a short note per screen naming which Foundations devices it uses, a list of every control whose form or label you changed with the old and new version side by side, and anything you could not verify.

## What good looks like

Someone who has seen the Foundations board should look at any screen and know it is DOTMENT within a second, and someone who has never seen it should find the page beautiful, calm, and expensive. It should feel like a console a bank would trust, not a dashboard template. If a choice is between adding something and taking something away, take it away.
