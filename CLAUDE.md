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
    --ink #E8EEF6  --ink-2 #9DAFC4  --ink-3 #6B7F97
    --p1 #352A87   --p2 #106DBE     --p3 #1BA9A5   --p4 #7FD03B  --p5 #F9FB0E

**Colour rule — this is the one that keeps the page coherent:**

- **Grey** (`--ink*`, `--rule*`) is chrome. Frames, labels, rules, furniture.
- **Teal** (`--p3`) means interactive. Links, focus, buttons, panel addresses.
- **Parula** (`--p1`–`--p4`) is reserved for *data*: the waterfall ramp and
  the programme timeline's categorical segments. Never use it to decorate.
- `--p5` (yellow) is the top of the waterfall ramp only.

**Type.** Archivo (display, variable width axis — `font-stretch:112%` for the
institutional/signage feel), IBM Plex Sans (body), IBM Plex Mono (all chrome).
Chrome is mono, uppercase, 10–11px, `.14–.16em` tracking. **Body copy is never
mono.** That single rule is what stops the instrument idiom becoming
unreadable.

**Components.**

- `.rails` — the page sits inside a bezel: hairlines down both edges.
- `.panel` + `.panel-bar` — every section is a panel. The bar carries an
  address (`02`), the section name, a rule that fills the gap, and an
  optional right-hand readout (`.rt`). The bar's label *is* the section
  `<h2>`, styled small and mono; big display lines inside the body are `<h3>`.
  Bar items clamp to one line with ellipsis; `.rt` and `.sub` hide below
  760px.
- `.fields` / `.f` — `LABEL — value — STATUS` ruled rows.
- `.rows` / `.r` — ruled time/what/duration rows, used by both the programme
  and the competition phase list.
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

**Status semantics.** The `.f .s` column publishes confidence:
`CONFIRMED` (`.ok`, teal) and `PROVISIONAL` (`.prov`, green). This is how the
site stays honest while facts are still settling — flip statuses as things
confirm rather than waiting to publish. Never mark something CONFIRMED that
`CONTENT.md` has as TO CONFIRM.

**Background mesh.** A fixed full-page canvas (`#mesh`, `z-index:-1`) of
drifting nodes whose links form and break by range, with nodes occasionally
dropping off the net and rejoining. It is decorative furniture, carries no
information, and is `aria-hidden`. It must stay subordinate to the waterfall —
dim, slow, and never bright enough to compete with body text. It honours
`prefers-reduced-motion` by rendering a single static frame.

**Signature element:** the animated spectrum waterfall canvas in the hero. It
is the one bold thing on the page. Keep everything else quiet — no competing
animation beyond the background mesh. It respects `prefers-reduced-motion`;
preserve that. Its
scale bar currently declares the data synthetic. That declaration comes off
only when real captured data replaces it (queue item 1).

## Quality floor

Responsive to 360px. Visible keyboard focus (`:focus-visible` is styled).
Reduced motion respected. Semantic headings in order. Form inputs labelled.
Check all five before considering a change done.

## Task queue

Roughly in priority order. Most items need data, photographs or a decision
from Artem before they can be built; ask rather than drafting placeholder
names or inventing figures.

1. **Real spectrum capture.** The hero waterfall is synthetic and says so on
   its scale bar. Replace with 60–120 s of real 2.40–2.48 GHz PSD (~512 bins),
   captured at the venue or in the lab, quantised to 8-bit and inlined. Then
   the caption becomes a provenance line: band, location, date, receiver.
2. **Interest form.** Currently a `mailto:` link with pre-filled fields, not a
   form — chosen so no attendee details reach a third-party service before
   UNSW says which tool they want used. See `CONTENT.md` §8. The original
   form markup and CSS are recoverable from the initial commit.
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
8. **Partner logos.** Fig. 03 slot in §05, once permissions are confirmed.
   Monochrome or on a light chip, uniform optical height.
9. **Capture-the-flag arena schematic**, or a playable 30-second demonstration
   run. Must not misrepresent the real rules.
10. **Blockchain scope.** Unresolved mismatch — see `CONTENT.md` §7.

## Deployment

See `README.md`. Custom domain via `CNAME`; DNS is at Squarespace and needs
the four GitHub A records plus a www CNAME. **There is no `CNAME` file in the
repository yet** — it must contain exactly `tacedge.org` or GitHub Pages will
serve only the `github.io` address.
