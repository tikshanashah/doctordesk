# The Doctor's Desk

CoverYou's editorial, practice intelligence and utility platform for doctors in India.

> **What matters without the effort of finding it.**

Written and curated by **Tikshana Shah**.

---

## What's in here

| File | What it is |
|---|---|
| `index.html` | **The current build.** Single-file site — HTML, CSS and JS bundled, no build step, no dependencies beyond Google Fonts. Open it in a browser. |
| `THE_DOCTORS_DESK_MASTER_README (1).md` | The master brief. Strategy, editorial voice, content jobs, sourcing discipline, information architecture. Authoritative on everything except visual identity (see below). |
| `From_the_Archive_*.docx` | 5 finished, sourced Archive pieces. |
| `Small_History_*.docx` | 5 finished, sourced Small History pieces. |
| `WHAT'S MOVING (1).docx` | 10 finished, sourced current pieces. |
| `the_doctor_s_desk (7).html` | The previous build, kept for reference. Superseded by `index.html`. |

All 20 articles from the .docx files are wired into `index.html`, with their references and SEO keywords intact.

---

## Design system

The master brief's §90–93 (visual identity, palette, typography) are **superseded**. Both the
README's CoverYou palette and the previous build's coral + glassmorphism system were dropped in
favour of a digest content model in a modern minimal newspaper-editorial dress.

**Concept — section inks on newsprint.** Warm newsprint stock, blue-black letterpress ink, and
a distinct ink per editorial stream. Colour is a section-front device: it tells you which part
of the Desk you are in, rather than decorating the page. Each stream's ink flows through its
section bar, its numbers, its buttons, its plates and its reader panel.

```
--paper    #F4F1E8   warm newsprint
--paper-2  #EDE8DC   recessed bands
--sheet    #FBFAF5   reading surface
--ink      #17232A   blue-black letterpress

--rounds   #A63528   Making Rounds — current affairs      5.88:1
--tools    #1B6353   Tools — practical                    6.29:1
--library  #8A5A1E   From the Archive — sepia             5.22:1
--history  #2F4F63   Small History — slate                7.69:1
--sand     #DCC9A2   on the dark Casebook ground          9.86:1
```

Streams are applied with `data-ink="rounds|tools|library|history|case"`, which sets
`--ink-accent` for everything inside. The reader picks up its article's stream ink on open.
All five pass WCAG AA on their grounds.

**Type.** [Newsreader](https://fonts.google.com/specimen/Newsreader) (variable, optical size
6–72) for display *and* body — a face built for news reading, whose optical size axis lets
display sizes tighten properly. [Archivo](https://fonts.google.com/specimen/Archivo) (width axis
75–100) for the functional layer, condensed at label sizes the way newspaper kickers are.

**Rules are hierarchy.** 6px coloured bar opens a section, 3px press rules divide, hairlines
separate entries. Thickness carries meaning; nothing is a border for decoration's sake.

**Plates are typographic by design.** The Library's lead images are colour fields carrying the
piece's own closing thought. A photograph, when one loads, fades in over the top. This is not a
failure state — remote images that hang never fire `onerror`, so a fallback triggered by failure
cannot be relied on.

**Breakpoints** are explicit column counts at 1100 / 980 / 940 / 820 / 560 / 400 — never
`auto-fit`, which produced five columns at 1440 and four-plus-an-orphan at 1024. Note that media
queries evaluate against the viewport *including* the scrollbar, so a 768px tablet reports 768.

**Kickers are serif italic**, not tracked-out caps eyebrows. **Numbers appear only in the issue
digest**, because that is the one place the content is genuinely a sequence.

**The issue leads with one story.** Entry 01 carries a 60px headline against 25px for the rest —
a 2.4× ratio — and closes on a rule in the section ink, so the eye lands somewhere before it
starts scanning. Without it, six items of equal rank read as a list rather than a front page.
The lead numeral and its rail scale down at 820 and 400, or it overruns the narrowed column.

**The front is a split.** The Desk says what it is on the left; on the right it leads with an
actual story carrying a plate. Before this, every image sat below the Casebook — the page opened
with five screens of type before the first picture. There are now three visual anchors spaced
down the page: the lead plate at the top, the dark Casebook in the middle, the Archive feature at
the bottom. The lead story appears in the hero **or** the numbered digest, never both.

**Motion.** The masthead line sets itself word by word, each rising out of its own mask, then the
deck and search follow — one orchestrated opening, after which the page is still. A section
announces itself by drawing its own 6px rule as it scrolls into view (`.sec-head::before`,
`scaleX`), which reuses the existing rule vocabulary rather than adding a new effect. Beyond
that it is interaction only: every control takes a press, hovering a row draws a 2px accent
hairline in from the left in that section's ink, the reader arrives on a long soft curve, and a
hairline tracks scroll position. Headlines use `text-wrap:balance`, decks `text-wrap:pretty`.
All of it is disabled under `prefers-reduced-motion`, which also draws every section rule
immediately so nothing is left invisible.

**Search re-weights the desk.** Typing doesn't open a dropdown — the whole page responds.
Matches stay lit, everything else drops to 14% and desaturates, folds force open so a match
can't hide inside one, and a section dims entirely when it has nothing to offer. This is §14's
"search as a core product layer" made literal. Two things to know if you touch it: content
elements are addressed by `data-open` **or** `data-filter` (the Archive's "also" items promote
rather than open, so they carry the latter), and `:not()` must be repeated per branch —
`"[data-open],[data-filter]:not(.nomatch)"` applies the negation only to the second selector,
which silently breaks the section-empty check.

A card-based modernisation was tried and reverted at 78d5521 — rounded surfaces and shadows cost
the flat typographic structure that was doing the work. Keep changes inside that structure.

## Homepage density

The homepage shows **22 items**, down from 34. Nothing was deleted; what came off the surface
went behind a fold or stayed reachable through search.

- **The "on your desk today" strip is gone.** All four of its items repeated content shown
  further down the same page — two Tools, the Casebook lead and the Archive lead — so a reader
  met each of them twice before reaching the digest. It added navigation, not content.
- **The issue shows a lead plus four**, with the remaining five current pieces folded.
- **The two Library series no longer share a layout.** They were rendering as the same module
  twice — two colour plates, two leads, two stacks of three items each carrying a full deck — so
  the eye had to parse the same pattern back to back.

**From the Archive is the resurfacing slot** (§65, §125), not a second grid. One story is
featured with the full-width plate, a large title and its deck; the other three are titles and
read-times only. Clicking one promotes it into the feature via the View Transitions API, so the
shelf is something you browse rather than a fixed list.

**Small History is an index.** "How did this become normal" is a listing question, so it reads as
a two-column list of titles and read-times — no plate, no decks. Two series, two textures.

`SHELF_ORDER` sets the Library order explicitly rather than inheriting it from `DESK`, because
two pairs tell the same story twice: `stethoscope-invention` twins `stethoscope-history` (both
Laënnec, 1816, the rolled paper tube), and `paternalism` twins `homevisit`. The Small History
half of each pair is placed last so it falls into the fold — the pair is never on screen
together. **If you reorder those shelves, keep the twins apart.** The proper fix is still to
merge or differentiate them (§84); this only stops them colliding visually.

---

## Content model

Everything lives in one `DESK` array in `index.html`, following a reduced form of the master
brief's §105 schema:

```js
{
  id, type, series, title, deck, keyword, read,
  body: [["lead"|"p"|"h"|"pull"|"list"|"dates"|"thought"|"note", value], ...],
  refs: [[label, url], ...],
  related: [id, ...],
  image, caption,
  author: "Tikshana Shah",
  editor: "Tikshana Shah"
}
```

`type` is one of `current` · `archive` · `history` · `tool` · `note` · `case`.

The render layer reads only from this array. When the Desk moves to an app, this is the one thing
that gets replaced by an API response — the rendering, search and reader all carry over.

Search indexes titles, decks, SEO keywords, series and full body text, with title and keyword
matches weighted.

---

## Editorial and legal discipline

Applied from the master brief and worth preserving in any future work:

- **The Casebook contains no case citations.** Every entry is a de-identified *pattern* drawn from
  recurring disputes. Nothing states what a court held. The previous build asserted judicial
  holdings without sources — that has been removed. Verified judgments with citations and links to
  the underlying text are the next stage of this section (§53, §57, §87).
- **The Consent Checklist Builder is a concept card, not a tool.** The interface is easy; the
  content library underneath is the product, and clinical risk content cannot be written from
  general knowledge. It ships as a described capability until the library is source-validated
  (§35, §88).
- **No overclaiming.** The previous build's "attach it to the consent form to prove a tailored
  discussion took place" and the template shelf's "legally sound" / "liability release clauses"
  language has been removed. Tools help structure and record a conversation. They do not provide
  legal protection, guarantee defensibility or reduce liability (§33, §89).
- **No signup, no email capture, no gating.** The Desk earns a bookmark before it asks for
  anything (§15).

---

## Still to do

1. **Case Intelligence needs real judgments.** Curate Indian judgments, verify case name, court,
   date, citation and exact holding against the underlying text, and link to it. Then the Casebook
   can state what courts actually said.
2. **Consent library.** Build the procedure × modifier content library from professional and
   clinical guidance, with review dates, before the builder goes live.
3. **Template shelf.** Draft and review the first five templates against current NMC and
   applicable state requirements.
4. **Images.** Most plates are Google Drive thumbnail links, which are fragile and rate-limited.
   Move to hosted, rights-cleared assets and record source, creator, collection and rights status
   for each (§96). Every image currently degrades to a typographic fallback if it fails to load.
5. **Rights check.** The Mike Savad photograph used in the previous build was marked "commercial
   usage to be confirmed" and has been dropped from this build rather than carried forward.
6. **SEO cannibalisation.** `stethoscope-invention` (Archive) and `stethoscope-history` (Small
   History) target overlapping intent, as do `paternalism` and `homevisit`. Differentiate or merge
   (§84).

---

## Running it

Open `index.html` in a browser. That's it — no build, no server, no dependencies.
