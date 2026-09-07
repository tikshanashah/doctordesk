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

**Concept — section inks, contemporary editorial.** Near-white ground, a high-contrast display
serif, and a distinct ink per editorial stream. Colour is a section-front device: it tells you
which part of the Desk you are in rather than decorating the page. Each stream's ink flows
through its rules, numerals, buttons, plates and reader panel.

```
--paper    #FAF8F5   ground
--paper-2  #F1EDE7   recessed
--surface  #FFFFFF   raised cards
--ink      #15191E

--rounds   #BE3A2B   Making Rounds — current affairs      5.48:1
--tools    #0F6B57   Tools — practical                    6.44:1
--library  #9C5B0C   From the Archive — amber             4.92:1
--history  #33507E   Small History — indigo               7.65:1
--sand     #E9D7B0   on the dark Casebook ground         12.45:1
```

Streams are applied with `data-ink="rounds|tools|library|history|case"`, which sets
`--ink-accent` for everything inside. The reader picks up its article's stream ink on open.
All pass WCAG AA on their grounds, including white-on-accent buttons.

**Type.** [Fraunces](https://fonts.google.com/specimen/Fraunces) (variable — `opsz`, `SOFT`,
`WONK`) for display only, tightly tracked. [Archivo](https://fonts.google.com/specimen/Archivo)
for everything you actually read. Sans body is the single thing that separates this from a
broadsheet pastiche — an earlier pass set body copy in a news serif and read as a facsimile of an
old newspaper rather than a contemporary publication.

**Depth over rules.** Cards on a near-white ground with a two-step radius scale (14px for list
items, 24px for panels) and two shadow levels. Radius tracks hierarchy rather than being applied
uniformly.

**Motion.** One page-load sequence on the hero, then interaction-driven only: cards lift on
hover, every button takes a `scale(.985)` press, the reader slides in on a quint ease, and
sections rise once as they enter the viewport — grouped and staggered by row, so a section
arrives as one gesture rather than element by element. A scroll-progress hairline tracks the
page. The whole motion layer is skipped under `prefers-reduced-motion`.

**Plates are typographic by design.** The Library's lead images are colour fields carrying the
piece's own closing thought. A photograph, when one loads, fades in over the top. This is not a
failure state — remote images that hang never fire `onerror`, so a fallback triggered by failure
cannot be relied on.

**Breakpoints** are explicit column counts at 1100 / 980 / 940 / 820 / 560 / 400 — never
`auto-fit`, which produced five columns at 1440 and four-plus-an-orphan at 1024. Note that media
queries evaluate against the viewport *including* the scrollbar, so a 768px tablet reports 768.

**Kickers are serif italic**, not tracked-out caps eyebrows. **Numbers appear only in the issue
digest**, because that is the one place the content is genuinely a sequence.

**Motion** is interaction-only — modal open, search filter, library swap via the View Transitions
API. Nothing animates on scroll. `prefers-reduced-motion` is respected.

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
