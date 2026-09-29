# CLAUDE.md — tacedge.org

Static site for the TacEdge 2026 workshop. Read `CONTENT.md` for the factual
source of truth before editing anything. Do not invent facts about the event;
if something is marked TO CONFIRM, ask rather than filling it in.

## What this is

Single-page static site (`index.html`), no build step, no dependencies, no
framework. Deployed on GitHub Pages at tacedge.org. Will migrate to
WordPress on a self-hosted server later, so keep the markup semantic and the
CSS in one block — portability matters more than cleverness here.

## Ground rules

- **One file of code.** All CSS in the `<style>` block, all JS in the
  `<script>` block at the end. Do not split into CSS/JS assets and do not add
  a bundler.
- **Media files are content, not dependencies.** Photographs, SAR tiles,
  detector frames, captured spectrum and partner marks live in `img/` and are
  referenced normally. This is the one exception to the single-file rule; it
  survives the WordPress migration as a media folder.
- **No dependencies** beyond the Google Fonts link already present.
- **No localStorage/sessionStorage.**
- **Accuracy over polish.** This site is read by Defence officers, academics
  and international partners. A wrong date or a misspelled name costs more
  than a plain layout does.
- **Australian English.** "Programme" for the event schedule, "organiser",
  "-ise" endings.
- **Names and titles are load-bearing.** Never adjust, abbreviate or guess a
  person's name, title or institution. Copy them exactly from `CONTENT.md`.

## Design system — instrument panel

The page is built to read as a piece of test equipment: ruled, addressed,
tabular, and honest about its own state. Extend this idiom rather than
replacing it.

**Palette** is MATLAB's *parula* colormap, chosen because it is the colormap
you actually see in a spectrum waterfall. Defined as CSS custom properties in
`:root`:

    --bg #0B1119   --bg-2 #0E1620   --bg-3 #141F2C
    --rule #22303F --rule-2 #2E4054
    --ink #E8EEF6  --ink-2 #9DAFC4  --ink-3 #7688A0
    --p1 #352A87   --p2 #106DBE     --p3 #1BA9A5   --p4 #7FD03B  --p5 #F9FB0E

**Colour rule — this is the one that keeps the page coherent:**

- **Grey** (`--ink*`, `--rule*`) is chrome. Frames, labels, rules, furniture.
- **Teal** (`--p3`) means interactive. Links, focus, buttons, panel addresses.
- **Parula** (`--p1`–`--p4`) is reserved for *data*: the waterfall ramp and
  the programme timeline's categorical segments. Never use it to decorate.
- `--p5` (yellow) is the top of the waterfall ramp only.

**Type.** Archivo (display, variable width axis — `font-stretch:112%` for the
institutional/signage feel), IBM Plex Sans (body), IBM Plex Mono (all chrome).
Chrome is mono, `.14–.16em` tracking, and **never below 11px** — that is a
legibility floor, not a style value, and it holds even in the footer. **Body
copy is never mono**, which is what stops the instrument idiom becoming
unreadable.

**Caps are for labels, not sentences.** Short chrome labels are uppercase.
A readout that is actually a sentence — the programme's provisional note, the
waterfall's provenance line — stays in sentence case with looser tracking. An
all-caps string of 40+ characters is a sentence wearing a label's clothes.

**Components.**

- `.rails` — the page sits inside a bezel: hairlines down both edges.
- `.panel` + `.panel-bar` — every section is a panel. The bar carries an
  address (`02`), the section name, a rule that fills the gap, and an
  optional right-hand readout (`.rt`). The bar's label *is* the section
  `<h2>`, styled small and mono; big display lines inside the body are `<h3>`.
  Bar items clamp to one line with ellipsis; `.rt` and `.sub` hide below
  760px.
- `.kicker` — small tracked teal line above the `<h1>`, carrying the short
  name so the `<h1>` can be the full workshop title.
- `.fields` / `.f` — `LABEL — value — readout` ruled rows. The `.s` cell
  carries a soft-fact badge (`Opens shortly`) or a country.
- `.sbar` — sub-bar dividing a panel body into named blocks: bold label, rule,
  right-hand readout. Used to split the programme into its free morning and
  paid afternoon.
- `.tl-split` — the free/paid bracket sitting above the timeline strip, with
  each label underlined across the span it covers. Hidden below 700px, where
  the `.sbar` labels carry the same information.
- `.logos` — partner mark row, greyscaled to a uniform 46px optical height.
  Written and commented out in §04; see `img/README.md` for the file list.
- `.rows` / `.r` — ruled time/what/duration rows. Programme rows carry a 3px
  colour chip (`.g-key`, `.g-talk`, `.g-tut`, `.g-comp`, `.g-break`) keyed to
  the timeline segment colours, so the legend means the same thing in both
  places. `.tbc` mutes a chip to 45% for an unconfirmed session, matching the
  same treatment on its timeline segment.
- `.tl-bar` + `.tl-ticks` + `.legend` — the day as a single 09:00–17:00 axis.
  Segment `left`/`width` are percentages of the 480-minute span; recompute
  them if timings change or the strip will lie.
- `.fig` + `figcaption` — figure plates with corner crop marks, for
  photographs and data figures. Caption them as plates (`Fig. 01 · …`).
- `.status` — the footer is a status bar.
- Squared corners throughout (`border-radius:0`), ghost buttons, no shadows,
  no blur, no gradients.

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

**Regenerate it whenever the hero facts change** — the card repeats the title,
date, venue and admission line, and nothing warns you when it goes stale.

**Status column.** `.f` rows have a third cell (`.s`) for a right-aligned
readout. It carried `CONFIRMED`/`PROVISIONAL` badges in the hero until every
fact firmed up; those were removed as noise once everything read CONFIRMED. It
now carries the country in the organisers list. If a fact goes soft again,
bring the badge back for that row only — never mark something CONFIRMED that
`CONTENT.md` has as TO CONFIRM.

**Background mesh.** A fixed full-page canvas (`#mesh`, `z-index:-1`) of
drifting nodes whose links form and break by range, with nodes occasionally
dropping off the net and rejoining. It is decorative furniture, carries no
information, and is `aria-hidden`. It must stay subordinate to the waterfall —
dim, and never bright enough to compete with body text. It honours
`prefers-reduced-motion` by rendering a single static frame.

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
6. **Tutorial micro-demos.** One working figure per tutorial card, each built
   from real data: a spectrum explorer that labels emitters on hover, a
   SAR↔optical wipe over Canberra (Sentinel-1 GRD + Sentinel-2, Copernicus
   attribution), and a detector frame with a live confidence threshold
   (needs the raw boxes as JSON, not a video). Tutorial 2's copy is grounded
   in the course SAR exercise in `Books/FST/source-materials/lectures/
   discussion_forums/Discussion_1.docx` — Sentinel-1 GRD dual-pol through
   calibration, multi-look speckle reduction and terrain correction, then read
   against optical imagery.
7. **Registration and travel info.** Venue address detail, getting there,
   nearby accommodation, and visa guidance for international attendees.
8. **Partner logos.** The `.logos` row is written and commented out in §04.
   Drop the six files listed in `img/README.md` into `img/` and delete the
   comment markers. Confirm each partner is happy to be shown first.
9. **Capture-the-flag.** The dedicated panel was removed on 29 September 2026
   because the session is no longer confirmed; it survives as one programme
   row at 16:00 with a muted chip and a `To be confirmed` readout. If it is
   confirmed, the panel comes back from git history (`b26f2cc`), along with
   the arena schematic and the promise to publish rules and API docs.
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
