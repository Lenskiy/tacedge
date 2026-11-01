# CLAUDE.md — tacedge.org

Static site for the TacEdge 2026 workshop. Read `CONTENT.md` for the factual
source of truth before editing anything. Do not invent facts about the event;
if something is marked TO CONFIRM, ask rather than filling it in.

## What this is

Single-page static site (`index.html`), no build step, no dependencies, no
framework. Deployed on GitHub Pages at tacedge.org. Will migrate to
a self-hosted web server on Kamatera later (Artem, 17 October 2026; not WordPress), so keep the markup semantic and the
CSS in one block — portability matters more than cleverness here.

## Ground rules

- **Plain punctuation only in visible text** (Artem, 14 October 2026): no
  middle dots or other decorative Unicode separators. Use commas, semicolons,
  colons and plain hyphens. The footer carries no name or email address.

- **One file of code.** All CSS in the `<style>` block, all JS in the
  `<script>` block at the end. Do not split into CSS/JS assets and do not add
  a bundler.
- **Media files are content, not dependencies.** Photographs, SAR tiles,
  detector frames, captured spectrum and partner marks live in `img/` and are
  referenced normally. This is the one exception to the single-file rule; it
  survives the server migration as a media folder.
- **No dependencies** beyond the Google Fonts link already present.
- **No localStorage/sessionStorage.**
- **Accuracy over polish.** This site is read by Defence officers, academics
  and international partners. A wrong date or a misspelled name costs more
  than a plain layout does.
- **Australian English.** "Programme" for the event schedule, "organiser",
  "-ise" endings.
- **Names and titles are load-bearing.** Never adjust, abbreviate or guess a
  person's name, title or institution. Copy them exactly from `CONTENT.md`.

## Design system — dark, plain

**Redesign of 2 November 2026 — this overrides anything below or in git
history that disagrees.** Artem asked again for the page to look professional
rather than generated, and picked this direction from two mockups (a light
institutional version was rejected; dark stays). Removed as tells of generated
design: the hairlines down both page edges and between nav links; the small
mono uppercase section bars with their numbers (`01`, `02`) and filler rules;
the `T-01` tags on tutorials; corner crop marks on figures; mono type for
content (programme titles, times, hero facts); uppercase tracked labels
everywhere; the ghost-outline buttons and the "status bar" footer. IBM Plex
Mono is no longer loaded. **Do not bring any of these back.** Kept: the dark
parula palette, the hero waterfall, the background mesh (Artem, 2 November
2026), the programme's colour chips and timeline strip, and one light rule
between programme rows and between tutorials.

Earlier clean-up of 13 October 2026, still in force: no `SYS` bar, no scale
bar or frequency labels under the waterfall, no "Include in your email" list,
no aphoristic copy, and no self-referential captions about what is simulated
or pending. The waterfall is an unlabelled illustration and makes no claim to
be data. If something is not real, leave it out rather than apologising for it.

**Palette** is MATLAB's *parula* colormap, chosen because it is the colormap
you actually see in a spectrum waterfall. Defined as CSS custom properties in
`:root`:

    --bg #0B1119   --bg-2 #0E1620   --bg-3 #141F2C
    --rule #22303F --rule-2 #2E4054
    --ink #E8EEF6  --ink-2 #9DAFC4  --ink-3 #7688A0
    --p1 #352A87   --p2 #106DBE     --p3 #1BA9A5   --p4 #7FD03B  --p5 #F9FB0E

**Colour rule — this is the one that keeps the page coherent:**

- **Grey** (`--ink*`, `--rule*`) is chrome. Frames, labels, rules, furniture.
- **Teal** (`--p3`) means interactive. Links, buttons, the kicker.
- **Parula** (`--p1`–`--p4`) is reserved for *data*: the waterfall ramp and
  the programme timeline's categorical segments. Never use it to decorate.
- `--p5` (yellow) is the top of the waterfall ramp only.

**Type.** Archivo for display (`font-stretch:105%`): the `<h1>`, section
`<h2>`s, sub-headings and tutorial `<h3>`s. IBM Plex Sans for everything else,
including times, labels, captions and the footer. No monospace, no uppercase
tracked labels, nothing below 13px. Sentence case throughout.

**Components.**

- `.rails` — the centred page column (`--wrap`), with no borders.
- `.nav` — sticky bar: wordmark left, plain text links, one solid teal
  Register button. Links other than Register hide below 820px.
- `.panel` + `.panel-bar` — each section. The bar holds the section `<h2>` as
  a plain display heading, and an optional muted right-hand note (`.rt`,
  hidden below 760px). Big display lines inside the body are `<h3>`.
- `.kicker` — short teal line above the `<h1>`, carrying the short name so the
  `<h1>` can be the full workshop title.
- `.fields` / `.f` — label, value, note rows without rules. The `.s` cell
  carries a soft-fact note (`Opens shortly`), a country or a link.
- `.sbar` — sub-heading splitting the programme into its free morning and paid
  afternoon, with a muted note beside it.
- `.tl-split` — the free/paid bracket above the timeline strip, each label
  underlined across the span it covers. Hidden below 700px, where the `.sbar`
  notes carry the same information.
- `.rows` / `.r` — programme rows, one light rule between each. Rows carry a
  3px colour chip (`.g-key`, `.g-talk`, `.g-tut`, `.g-comp`, `.g-break`) keyed
  to the timeline segment colours, so the legend means the same thing in both
  places. `.tbc` mutes a chip to 45% for an unconfirmed session, matching its
  timeline segment. Abstracts and bios sit in a collapsed `.abs` under the row.
- `.tl-bar` + `.tl-ticks` + `.legend` — the day as a single 09:00–17:00 axis.
  Segment `left`/`width` are percentages of the 480-minute span; recompute
  them if timings change or the strip will lie.
- `.tut` — a tutorial row: figure one side, text the other, alternating via
  `.rev`, stacking below 880px. `.card-bar` is a small green subject label
  above the title. Do not put the tutorials back in equal-size cards.
- `.cap` / `figcaption` — plain muted caption under a figure. It says what the
  figure is.
- `.fig` — a figure frame with no border or crop marks.
- `.logos` — partner marks on light chips, two across, small radius.
- `.note` — a callout with a 3px teal left border on `--bg-2`.
- `.btn` — solid teal button, 4px radius.
- `.status` — plain footer: place and date, no name or email.
- No shadows, no blur, no gradients. Radius only on buttons, logo chips and
  the timeline strip (3–4px).

**Voice.** Plain, factual, specific. Prefer concrete nouns, numbers and dates
to claims about significance. Avoid aphorisms, three-part rhetorical lists and
neat antitheses — they read as generated text to exactly this audience. If a
sentence could sit on any workshop's site, it is not earning its place.

**Two hard rules for this aesthetic:**

1. **Never fake a readout.** No decorative signal levels, coordinates or
   counts. The design borrows the authority of instrumentation, and one
   invented number discredits the whole page for exactly the audience that
   matters.
2. **Photographs carry all the warmth.** The chrome is deliberately cold. The
   mitigation is large, human figures — hands, gear, faces, the arena — set
   inside the plates. Until those land, the page will read austere.

**Shareable surface.** `img/favicon.svg` (plus a 32px PNG and a 180px
apple-touch-icon) is three vertical traces in teal, green and parula yellow on
the dark field — a spectrum trace, legible at 16px. `theme-color` is `--bg`.

The social card at `img/og.png` (1200×630) is **generated, not drawn**:
`img/og-source.html` loads the same Google Fonts and palette and is rendered
headless, so its typography is the site's rather than an approximation, and the
waterfall in it is a real render of the airband script.

    google-chrome --headless=new --window-size=1200,630 \
      --screenshot=img/og.png "file://$PWD/img/og-source.html"

Chrome is not installed on Artem's Mac; the Playwright copy works:
`~/Library/Caches/ms-playwright/chromium-1248/chrome-mac-x64/Google Chrome for Testing.app/Contents/MacOS/Google Chrome for Testing`.
The card was restyled with the site on 2 November 2026: no `SYS` bar, no mono.

**Search data (added 2 November 2026).** The `<head>` carries a schema.org
`EducationEvent` block (JSON-LD): date and times, venue address, the six
speakers, and the three Eventbrite tickets with prices. It is data, not
script, so it does not break the one-script-block rule. **Update it whenever
a speaker, price, time or ticket changes** — Google shows it in search
results, and nothing on the page warns you when it disagrees. `robots.txt`
and `sitemap.xml` sit at the root; bump the sitemap's `lastmod` on
substantive changes.

**Regenerate it whenever the hero facts change** — the card repeats the title,
date, venue and admission line, and nothing warns you when it goes stale.

**Status column.** `.f` rows have a third cell (`.s`) for a right-aligned
readout. It carried `CONFIRMED`/`PROVISIONAL` badges in the hero until every
fact firmed up; those were removed as noise once everything read CONFIRMED. It
now carries the country in the organisers list. If a fact goes soft again,
add a plain note to that row only ("To be confirmed"), in sentence case —
never mark something confirmed that `CONTENT.md` has as TO CONFIRM.

**Background mesh.** A fixed full-page canvas (`#mesh`, `z-index:-1`) of
drifting nodes whose links form and break by range, with nodes occasionally
dropping off the net and rejoining. It is decorative furniture, carries no
information, and is `aria-hidden`. It must stay subordinate to the waterfall —
dim, and never bright enough to compete with body text. It honours
`prefers-reduced-motion` by rendering a single static frame.

**Waterfall model, rewritten 13 October 2026.** The paragraph below describes
the first version. The script now models a narrow slice of the airband
(about 500 kHz on a wide screen, 400 Hz a bin) in decibels: averaged thermal
noise, AM carriers with mirrored speech sidebands, call-and-readback
exchanges between a ground station and aircraft of differing strength and
carrier offset, one continuous broadcast, two kinds of data burst, and the
receiver's own centre spike, spurs and band-edge roll-off. Artem decided
later the same day to **keep this simulated waterfall** rather than replace
it with a recording, so queue item 1 (real spectrum capture) is closed.

**Signature element:** the animated spectrum waterfall canvas in the hero. It
shows the VHF airband, 118–137 MHz: a quiet noise floor, one continuous
information broadcast, and short keyed AM exchanges between a tower and its
traffic, each channel with its own traffic rate and speech-modulated level.
Channel positions are illustrative spacing, **not** an operational frequency
list, and the scale bar says simulated. An earlier version modelled the
2.4 GHz ISM band; airband replaced it because it reads far better — a dark
floor with a few crisp traces, rather than three wide Wi-Fi blocks filling
the frame. It
is the one bold thing on the page. Keep everything else quiet — no competing
animation beyond the background mesh. It respects `prefers-reduced-motion`;
preserve that. The scale bar's "simulated" declaration comes off only when
real captured data replaces it (queue item 1).

## Quality floor

Responsive to 360px. Visible keyboard focus (`:focus-visible` is styled).
Reduced motion respected. Semantic headings in order. Form inputs labelled.
Check all five before considering a change done.

## Task queue

Roughly in priority order. Most items need data, photographs or a decision
from Artem before they can be built; ask rather than drafting placeholder
names or inventing figures.

1. **Real spectrum capture.** The hero waterfall is simulated and says so on
   its scale bar. Replace with a real airband capture — an RTL-SDR anywhere
   near Canberra Airport will do it:

       rtl_power -f 118M:137M:8k -i 1 -e 180 -g 40 airband.csv

   Quantise to 8-bit and inline it; the scale bar then becomes a provenance
   line: band, location, date, receiver. Airband is a far easier capture than
   the 2.4 GHz band the earlier version simulated.
2. **Eventbrite links.** Registration is settled: two separate tickets on
   UNSW's Eventbrite — free morning, paid afternoon capped at 40. Both rows in
   §05 currently read `Opens shortly`; replace each with its own link and
   button when Artem supplies them, and retire the `mailto:` rather than
   keeping both. See `CONTENT.md` §8.
3. **Session leads.** Add tutorial and session presenters — students and
   colleagues running the sessions. Names under each tutorial card is the
   lightest touch; a separate "Presenters" section is warranted if there are
   more than about six people. See `CONTENT.md` §5. Several are students —
   ask each of them directly before publishing a name or photo.
4. **Talk sessions.** Currently "three talks" with no detail. Whether these
   are reviewed papers or invited talks is unresolved (see `CONTENT.md` §7) —
   this determines whether a call-for-papers section is needed.
5. **Photographs.** Fig. 01 slot in §01 (wide, arena or foyer demonstrations)
   and Fig. 02 in §04. Environmental shots — hands, gear, the arena in build —
   not studio headshot grids. Slots are commented out in the markup.
6. **Tutorial figures — placed 13 October 2026, from Artem's book**
   (`Books/FST`, *Fundamentals of Surveillance Technologies*). T-01:
   `img/tut-sdr-fm-band.jpg` (book Fig. 11.2). T-02:
   `img/tut-sar-gibraltar.jpg` (Fig. 12.1). T-03:
   `img/tut-detection-ships.jpg` (Fig. 17.22), which replaced the simulated
   threshold widget. Artem chose to show them without credit lines; the
   sources and licence terms are in the book's `PERMISSIONS-AND-CREDITS.md`.
   The notes below are the history. **Real data only** remains the rule. T-01 and T-02 carried simulated
   figures and Artem rejected both on 30 September 2026: at this level of
   workshop a synthetic waterfall and a synthetic speckle field read as
   artificial and undercut the expertise the page is claiming. Both rows are
   now text-only (`.tut.solo`) until real captures exist. **Do not put a
   simulated figure back.** The bar for this section is evidence.

   - **T-01** awaiting a real capture from Artem's SDR. Note a standard
     RTL-SDR tops out near 1.7 GHz and cannot reach 2.4 GHz Wi-Fi; that needs
     a HackRF, ADALM-Pluto or USRP.
   - **T-02** awaiting a real Sentinel-1 scene against optical. Copernicus
     product downloads need an account (the catalogue is public, downloads
     return 401). The strongest source is Artem's own processed output from
     `Books/FST/source-materials/lectures/discussion_forums/Discussion_1.docx`.
   - **T-03** still carries a draggable confidence threshold over a
     deterministic score distribution. It is statistics rather than invented
     imagery, which is why it survived, but it should give way to real
     detector output — including ensemble agreement — when a frame exists.
7. **Travel — built as §05.** Venue street address, airport, accommodation
   area, the adjacent conference, and a link to the Home Affairs visa finder
   with an offer of invitation letters. Outstanding: the building and room
   (TO CONFIRM with Steph), and whether Artem will actually issue invitation
   letters, which the page now offers on his behalf.
8. **Partner logos — live in §04.** Three institutional chips: UNSW Canberra,
   KICS and Taiwan Tech. Marks sit on light chips rather than reversed out,
   and both axes are capped so a wide lockup and a tall crest balance. See
   `img/README.md`.
9. **Capture-the-flag.** A short `#competition` panel (§04) came back on
   13 October 2026 at Artem's request: one paragraph and
   `img/arena-plato-pod.jpg`, copied from platopod.com. That picture shows a
   surveillance game, not capture the flag, so its caption describes the
   arena and does not claim to show the competition. The panel bar and the
   programme row still read "to be confirmed". Earlier history: The dedicated panel was removed on 29 September 2026
   because the session is no longer confirmed; it survives as one programme
   row at 16:00 with a muted chip and a `To be confirmed` readout. If it is
   confirmed, the panel comes back from git history (`b26f2cc`), along with
   the arena schematic and the promise to publish rules and API docs — but
   rewrite it first: the format changed on 13 October 2026 (eight teams of
   two, knockout, early rounds simulated, final on the Plato Pod arena) and
   the old panel describes the superseded one. The row links to platopod.com
   and carries the general idea only; see `CONTENT.md` §4.
10. **Blockchain scope.** Unresolved mismatch — see `CONTENT.md` §7.

## Deployment

Live at https://tacedge.org from `Lenskiy/tacedge`, GitHub Pages on `main`
at root, HTTPS enforced. `CNAME` contains `tacedge.org`; DNS is at Squarespace
(Google Cloud nameservers) with the four GitHub A records on the apex and a
`www` CNAME to `lenskiy.github.io.`.

Note: changing the custom domain through the GitHub API makes GitHub commit
to the repository (it writes or deletes the `CNAME` file), so pull afterwards
or your local branch will be behind. `CONTENT.md` is gitignored and must stay
that way — the repository is public.
