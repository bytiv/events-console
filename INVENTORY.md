# DOTMENT Control Center — inventory before the redesign

Taken from `index.html` at commit `c89b4bf`, served over http and walked in a headless browser. This records substance only: copy, controls, data, behaviour and storage. Nothing here describes how anything looks.

## Data model and storage

- **Password.** `PASSWORD = '121211'`, compared exactly against the gate input.
- **Storage keys.** Data: `localStorage['dotment-control-center']` (also mirrored to `sessionStorage` and an in-memory map; reads fall back in that order). Session: cookie `dcc_session=1` (path `/`, max-age one year, SameSite Lax) plus storage flag `dotment-control-center-session = '1'`. Either one keeps the user signed in.
- **Seed version.** `v: 2`. `load()` accepts stored data only when `v === 2` and `brands` exists; otherwise it writes a fresh seed. The seed loads on first visit only.
- **Static-field sync.** On every load, `syncStatic()` copies `logo`, `team`, `name` and `link` from the fresh seed into each stored brand with the same `id`. Everything else (status, plan, access, segments, limits, caps, branding, ui) is what the browser holds.
- **Stored shape.** `{ v, ui:{view:'brands'|'manage', brand:id|null, tab:'overview'|'segments'|'access'|'branding'}, brands:[…] }`.
- **Brand shape.** `id, name, slug, link, logo, plan, status ('ACTIVE'|'TRIAL'|'SUSPENDED'), prevStatus, access{period, renews (ISO date), lapsed}, timezone, roster, events[{name,date (ISO datetime),status ('ARCHIVED'|'UPCOMING'|'LIVE'),attendees,checkedIn}], team[{name,email,role ('OWNER'|'ADMIN'|'OPERATOR')}], segments{segId:boolean}, limits{eventsPerMonth,attendeesPerEvent,operatorSeats,bigScreens}, caps{whitelabel,domain,recap,excel,ai,rehearsal,reports,templates}, branding{colors{canvas,ink,accent,card,muted}, font, placement{phones,bigscreen,join,emails}}`.
- **Account.** `ME = 'kholi@dotment.com'` (shown in the header).
- **Timezone names.** `Africa/Cairo → Cairo`, `Asia/Dubai → Dubai`, `Europe/London → London`, anything else → `Local`.
- **Plan to preset.** `Starter → STARTER`, `Growth → GROWTH`, `Enterprise → FULL` (defined, not used by the UI).

### The library (50 segments in 7 groups)

Each segment: id, name, tier, one-line description. Verbatim.

**Arrival**
- `photo-checkin` · Photo check-in · CORE · Find your own face on the wall and claim it. No app, no typing.
- `pre-event` · Pre-event page · CORE · Agenda, speakers and location, live before doors open.
- `welcome` · Welcome · CORE · The opening screen the host fires when the night starts.
- `agenda` · Agenda screen · CORE · The whole flow on every phone, and where the room is right now.
- `net-goals` · Networking goals · PLUS · Pick a goal on arrival and get nudged toward it all night.
- `guess-who` · Guess who · PLUS · One surprising fact each. The room guesses whose it is.

**Learning**
- `assessment` · Assessment · CORE · Skill self-assessment with the brand’s own domains and levels.
- `results` · Personal results · CORE · Each phone shows that person’s level and breakdown, instantly.
- `capability` · Capability pick · CORE · Everyone picks the one skill they’ll master next.
- `academy` · Learning academy ask · PLUS · One question and interest capture for a follow-up program.
- `quiz` · Quiz battle · PLUS · Timed questions, points for speed, a live leaderboard between rounds.
- `predictions` · Predictions · PLUS · Everyone predicts a number. The closest guess is crowned at the end.

**Networking**
- `scratch` · Scratch reveal · CORE · Scratch the card to reveal who you’re meeting and a starter.
- `tables` · Table grouping · CORE · The room splits into tables. Every phone shows where to go.
- `circle-topics` · Circle topics · CORE · Discussion topics per circle, editable by the brand.
- `mini-circles` · Mini circles · PLUS · Smaller breakout rounds with auto-move and arrivals.
- `speed` · Speed rounds · PLUS · Timed 1:1 rotations with a conversation prompt each round.
- `topic-tables` · Topic tables · PLUS · Anyone posts a topic, others book the open seats.
- `roulette` · Icebreaker roulette · PLUS · The screen picks two people. They ask each other a question, live.
- `bingo` · Human bingo · PLUS · A card of traits. Find the people who match. First full card wins.
- `mentor` · Mentor pool · PLUS · Raise a hand to mentor or to learn. The wall fills up live.

**Live interaction**
- `live-q` · Live question · CORE · Host opens a question, everyone types, answers land on the big screen.
- `spotlight` · Spotlight · CORE · One person and a prompt on the big screen.
- `wordcloud` · Word cloud · CORE · Everyone types one word. The cloud grows live.
- `qa` · Q&A with upvotes · CORE · Ask from your phone, upvote others. Top questions rise.
- `polls` · Poll pack · CORE · Multiple choice, ranking, sliders and points allocation.
- `this-that` · This or that · CORE · Rapid either-or questions. The split shows as two growing bars.
- `stand` · Stand your side · PLUS · A bold statement, agree or disagree, a meter that moves live.
- `pin` · Pin on image · PLUS · Drop a pin on a map or a 2×2 grid. Dots build up live.
- `story` · Story wall · PLUS · A prompt, two lines each. The best stories rotate with faces.
- `slides` · Slides · PLUS · Upload a deck. The host flips slides from the control room.

**Energy & games**
- `reactions` · Reaction storm · CORE · Tap emojis and they rain over the big screen.
- `spinner` · Spinner wheel picker · CORE · A wheel of attendee faces picks who answers next.
- `flash` · Flash round · PLUS · An image hits the screen. Fastest correct guess takes the round.
- `table-battle` · Table battle · PLUS · Tables compete as teams. Mini-challenges, points, a leaderboard.
- `prize` · Prize draw · PLUS · A raffle over checked-in attendees, drawn live.
- `mystery` · Mystery segment · PREMIUM · One button pulls a surprise from a shortlist the brand approved.
- `energy` · Energy meter · PLUS · One-tap mood at the start and the end. See what the night did.
- `pulse` · Feedback pulse · CORE · One emoji tap after a talk. Instant sentiment for the operator.

**Recognition**
- `ballot` · Ranked ballot + reveal · CORE · Everyone votes. The screen reveals 3rd, 2nd, 1st with drama.
- `workshops` · Workshops vote · PLUS · Pitch workshop options, the room votes, winners run.
- `awards` · Awards ceremony · PREMIUM · Awards generated from the night’s own data, revealed one by one.
- `photo-wall` · Photo wall · PLUS · Phones shoot, the big screen becomes a live mosaic of the night.
- `sponsor` · Sponsor spotlight · PREMIUM · Rotating sponsor slides between segments, logos on the join page.

**After the event**
- `commitment` · Commitment card · CORE · A shareable card with your superpower and your next skill.
- `connect` · Connect wall · CORE · Everyone you met tonight, one tap to LinkedIn.
- `email` · Email capture · CORE · One consent-clean email ask, placed anywhere in the flow.
- `recap` · Recap mode · PLUS · After the event, phones become a take-home recap of the night.
- `countdown` · Countdown break · CORE · A break screen with a timer. Phones and big screen in sync.
- `announce` · Announcements · CORE · Push a banner to every phone the moment it matters.

### Presets

- `STARTER` = every CORE segment on, everything else off.
- `GROWTH` = every CORE and PLUS segment on, PREMIUM off.
- `FULL` = everything on.
- `currentPreset(b)` returns the preset name whose set matches the brand exactly, or `null`.

### Capabilities (8)

- `whitelabel` · White-label · Remove the DOTMENT badge everywhere.
- `domain` · Custom domain · The event lives at the brand’s own address.
- `recap` · Recap mode after the event · Phones stay live as a take-home recap.
- `excel` · Excel exports · One-click exports with saved column sets.
- `ai` · AI fill for content · A first draft of every segment from one line.
- `rehearsal` · Rehearsal mode · Run the whole night with test data, then wipe.
- `reports` · Personal reports by email · A branded report to each attendee’s inbox.
- `templates` · Event templates · Save any event as a format and relaunch it.

### Limits (4)

`eventsPerMonth` · Events per month — `attendeesPerEvent` · Attendees per event — `operatorSeats` · Operator seats — `bigScreens` · Big screens per event.

### Logo placements (4)

`phones` · Phones — `bigscreen` · Big screen — `join` · Join page — `emails` · Emails.

### Seed brands

**InCircle** — `id incircle`, slug `incircle`, link `https://2ndmeetup.incircle.community/admin`, logo `assets/logos/incircle.png`, plan Enterprise, status ACTIVE, access Yearly renews 2027-08-01, timezone Africa/Cairo, roster 93. Events: Community Night #1 · 2026-08-04T19:00 · ARCHIVED · 93 attendees · 71 checked in; Community Night #2 · 2026-08-25T19:00 · ARCHIVED · 93 · 78. Team: Mohamed Elkholi · kholi@incircle.community · OWNER; Nour Hassan · nour@incircle.community · ADMIN; Omar Farid · omar@incircle.community · OPERATOR; Mariam Adel · mariam@incircle.community · OPERATOR. Segments on (20): photo-checkin, pre-event, welcome, assessment, results, capability, academy, scratch, tables, circle-topics, mini-circles, mentor, live-q, spotlight, ballot, workshops, commitment, connect, email, recap. Limits 8 / 500 / 5 / 3. Caps: all eight on. Branding: canvas #F0F8FC, ink #1A1A1A, accent #9EBBF1, card #FFFFFF, muted #8A9093; font Gilroy; placement all four on.

**SNBC Bank** — `id snbc`, slug `snbc`, link `https://snbc.dotment.com`, logo `assets/logos/snbc.png`, plan Growth, status ACTIVE, access Monthly renews 2026-10-01, timezone Africa/Cairo, roster 140. Events: Leaders Offsite · 2026-09-17T18:30 · UPCOMING · 140 · 0. Team: Dina Mansour · dina.mansour@snbc.example · OWNER; Karim Sabry · karim.sabry@snbc.example · ADMIN; Salma Ezz · salma.ezz@snbc.example · OPERATOR. Segments: GROWTH preset minus bingo and slides, plus awards (46 on). Limits 4 / 250 / 3 / 2. Caps: whitelabel off, domain off, the other six on. Branding: canvas #F4F1EA, ink #141414, accent #1F4E79, card #FFFFFF, muted #7A7F84; font Brand upload; placement phones, bigscreen, join on; emails off.

**HSA** — `id hsa`, slug `hsa`, link `https://hsa.dotment.com`, logo `assets/logos/hsa.png`, plan Starter, status TRIAL, access Per agreement renews 2026-11-30, timezone Asia/Dubai, roster 60. Events: Team Day · 2026-10-08T19:00 · UPCOMING · 60 · 0. Team: Layla Haddad · layla@hsa.example · OWNER; Ravi Menon · ravi@hsa.example · OPERATOR. Segments: STARTER preset minus reactions and wordcloud, plus quiz and speed (25 on). Limits 2 / 100 / 1 / 1. Caps: recap, excel, rehearsal on; whitelabel, domain, ai, reports, templates off. Branding: canvas #FFFFFF, ink #0B0B0B, accent #2E6F73, card #F6F6F6, muted #8C9196; font Gilroy; placement phones, bigscreen on; join, emails off.

### Formatting helpers

- Day: `AUG 4TH`, `AUG 25TH` (month caps + ordinal caps). Day with year: `AUG 1ST 2027`.
- Time: `7:00 PM` (12-hour).
- Event line: `AUG 25TH · 7:00 PM · Cairo Time`.
- Access line: `Yearly · renews AUG 1ST 2027`; if `lapsed` then `Yearly · lapsed`; if no renew date, just the period.
- Segment count: number of segment ids true on the brand.
- Quarter range: first day of the current quarter to the last day of its third month (`YYYY-MM-31`).

## Document

- Title: `DOTMENT · Platform Control Center`. Meta description: `DOTMENT's console above every brand workspace.` Theme colour `#000000`. Favicons: `assets/favicon.svg`, `assets/favicon-32.png`, apple touch `assets/favicon-180.png`.
- Fonts: Gilroy 300/400/500/600/700/800 plus italics 300/500/600/700 from `assets/fonts/`; IBM Plex Mono 400/500.
- Three top-level regions: `#gate` (section), `#app` (header, `main > #main`, footer), `#toast`.
- Decoration only: `#burst` holds a generated halftone-burst SVG on the gate.

## Screen 1 — the gate (`#gate`)

Shown when not signed in. Full screen.

**Copy, verbatim**
- Eyebrow: `Platform Control Center`
- Heading: `Nice to see you.` (period is the accent)
- Lede: `The console above every workspace.`
- Field label: `Password`
- Field placeholder: `Your console password`
- Error note: `That's not it. Try again.`
- Button: `Enter`
- Rail, bottom left: `Dotment.com` · bottom right: `Dotment` (wordmark)

**Controls**
- `#pw` — password input, `autocomplete=current-password`, spellcheck off. Typing clears the error state.
- Submit button `Enter` (form `#gateForm`, `novalidate`). Enter key submits too.

**Behaviour**
- Correct password: `signIn()` writes the cookie and the storage flag, clears the error, cross-fades gate → app, routes to the stored `ui` view (brands by default), scrolls to top.
- Wrong password: input gets the error state, the note shows, the input's text is selected for retyping. No toast.
- On show, the input is emptied and focused.
- Reload while signed out: the gate again. Reload while signed in: straight to the app, on whatever view and tab was last stored.

## Screen 2 — Brands (`ui.view = 'brands'`)

**Header (shared with Manage)**
- Wordmark `Dotment` — link, returns to Brands (`data-go="brands"`).
- Account email: `kholi@dotment.com`.
- Button `Sign out` (`#signOut`).

**Copy, verbatim**
- Heading: `Every brand, one screen.`
- Subtitle: `Grant access, switch segments on, and watch the nights run.`
- Stat captions: `Brands`, `Events this quarter`, `Attendees checked in`, `Segments in the library`.
- Brand card row labels: `Plan`, `Access`, `Segments`.
- Brand card buttons: `Open`, `Manage`.

**Data shown**
- `Brands` = number of brands (3).
- `Events this quarter` = events across all brands whose date falls inside the current calendar quarter (3 on 2026-09-05).
- `Attendees checked in` = sum of `checkedIn` across every event (149).
- `Segments in the library` = 50.
- Per card: logo image (or, when a brand has no logo, the first letter of its name), name, status (`ACTIVE` / `TRIAL` / `SUSPENDED`), plan, access period word only (`Yearly`, `Monthly`, `Per agreement`), `on / 50` segment count.

**Controls**
- `Open` — anchor to the brand's `link`, `target=_blank rel=noopener`.
- `Manage` — `data-manage=id`, goes to Manage for that brand on the Overview tab.
- Wordmark, `Sign out` as above.

**Behaviour**
- Cards are listed in seed order.
- `Sign out` clears the cookie and the flag, cross-fades app → gate, empties and focuses the password input. The stored `ui` is not changed, so the next sign-in lands where the user left off.

**Footer (shared)**
- Left: `©2026 · Dotment.com`. Right: `With pure love·` (the middle dot is the accent).

## Screen 3 — Manage (`ui.view = 'manage'`, `ui.brand = id`)

**Frame**
- Crumb: `Brands` (link, `data-go="brands"`) · `{brand name}`.
- Masthead: logo, heading `{brand name}.` (accent period), status line `#mastStatus` (dot + `ACTIVE` / `TRIAL` / `SUSPENDED`).
- Primary button `Sign in as brand` (`#signInAs`): opens the brand's `link` in a new tab (`noopener`) and toasts `Signed in as {name}. Opening their workspace.`
- Tabs (`data-tab`): `Overview`, `Segments`, `Access`, `Branding`. Clicking a tab stores it in `ui.tab`, re-renders, scrolls to top. The tab survives refresh.

### Tab: Overview

- Card `Facts`, key-value rows: `Workspace` → slug; `Link` → `{slug}.dotment.com`; `Timezone` → IANA name (e.g. `Africa/Cairo`); `Plan` → plan; `Access` → access line (e.g. `Yearly · renews AUG 1ST 2027`); `Roster` → `{roster} people`.
- Card `Events`, table columns `Event`, `When`, `Status`, `Attendees`, `Checked in`. Rows sorted newest first. `When` = event line with city time. `Status` chip = `ARCHIVED` / `UPCOMING` / `LIVE`. `Checked in` shows the number for ARCHIVED events and `—` otherwise.
- Card `Team`, table columns `Name`, `Email`, `Role`. Role chip = `OWNER` / `ADMIN` / `OPERATOR`. Rows in seed order.
- Nothing on this tab is editable.

### Tab: Segments

- Heading: `What {brand name} can run.`
- Counter `#segCounter`: `{n} of 50 segments on`.
- Preset row: caption `Presets`, buttons `Starter`, `Growth`, `Full library` (`data-preset` = `STARTER` / `GROWTH` / `FULL`). The one matching the brand's exact set is marked current; if none matches, none is.
- One section per group (`data-group=name`), header: group name, `{on} / {total} on` (`data-groupcount`), group toggle (`data-grouptog=name`, `aria-label="All of {group}"`), on only when every segment in the group is on.
- One row per segment (`.seg`, `data-seg=id`): name, description, tier chip `CORE` / `PLUS` / `PREMIUM`, toggle (`data-segtog=id`, `aria-label` = name). Rows that are off carry class `off`.

**Behaviour**
- Segment toggle: flips that id, updates its own switch and row state, saves, toasts `Saved`, then refreshes the counter, the current preset marker, every group count and every group toggle in place (no re-render).
- Group toggle: sets every segment in the group to the opposite of the toggle's current state, saves, toasts, re-renders the Segments tab.
- Preset: replaces the whole segment map with the preset set, saves, toasts, re-renders the Segments tab.
- All of it persists: reload shows the same switches, counter and preset marker.

### Tab: Access

- Card `Access period`:
  - Label `Period`, select `#accPeriod` (`data-access="period"`) with options `Monthly`, `Yearly`, `Per agreement`.
  - Label `Renews on`, date input `#accRenew` (`data-access="renews"`).
  - Helper `#renewHelp` under the date: the access line.
  - Sub-panel `Suspend access` — `Locks the workspace and every event in it. Nothing is deleted.` — toggle `#suspendTog` (`aria-label="Suspend access"`), on when status is `SUSPENDED`.
- Card `Limits`: labels `Events per month`, `Attendees per event`, `Operator seats`, `Big screens per event`; number inputs `#lim-{key}` (`data-limit=key`, min 0, step 1, numeric keyboard).
- Card `Capabilities`: eight rows, each name + description + toggle (`data-cap=key`, `aria-label` = name).

**Behaviour**
- Changing period or date saves and toasts, and rewrites the helper line. Setting a date also clears `lapsed`.
- Changing a limit: parsed as an integer, anything invalid or negative becomes 0 and is written back into the field, then saves and toasts.
- Suspend on: remembers the previous status in `prevStatus`, sets status `SUSPENDED`, updates the masthead status line and the helper line, saves and toasts. Suspend off: restores `prevStatus` (or `ACTIVE`), clears `prevStatus` and `lapsed`, updates the same places, saves and toasts. The new status also shows on the Brands cards after navigation.
- Capability toggle: flips the key, updates the switch, saves and toasts.
- All of it persists on reload.

### Tab: Branding

- Left card: logo, `{brand name}`, caption `Logo`. Label `Font`, select `#brandFont` (`data-branding="font"`) with options `Gilroy`, `Brand upload`, `Inter`. Heading `Logo placement`, four rows `Phones`, `Big screen`, `Join page`, `Emails`, each with a toggle (`data-place=key`).
- Right card `Palette`: five swatches with captions `Canvas`, `Ink`, `Accent`, `Card`, `Muted` and the hex in upper case. Caption: `Set by the brand in their studio.`

**Behaviour**
- Font change saves and toasts. Placement toggle flips the key, updates the switch, saves and toasts. Both persist.
- Palette is read-only.

## Shared behaviour

- **Toast** (`#toast`, `role=status`, `aria-live=polite`): text `Saved` after every save, or the sign-in-as message. Shows for 3 seconds; repeating the same message while visible only extends the timer.
- **Navigation** (`go`): every view change writes `ui` to storage first, then renders, then scrolls to top. Refreshing anywhere reopens the same screen and tab.
- **Screen swap**: gate ↔ app cross-fade of about 450 ms; the outgoing screen is hidden after 220 ms and the incoming one is revealed.
- **Reduced motion**: all transitions collapse to near zero.
- **Logos**: `assets/logos/incircle.png`, `assets/logos/snbc.png`, `assets/logos/hsa.png`, shown as images at natural aspect ratio, height-constrained, no plate.
- **New tabs**: only the brand `Open` links and `Sign in as brand`.
- **No external requests**: fonts, logos and favicons are relative paths; no scripts or styles are fetched.
