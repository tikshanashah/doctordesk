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

**Concept — two-ink press.** The Desk is set as if printed in two inks on clinical form stock.
One ink carries the page. The spot colour is reserved for the case/legal rail and cautions, and
is never used as decoration.

```
--stock   #EDF1F0   pale cool grey-green, the tint of duplicate case-sheet paper
--sheet   #F9FBFA   the reading surface
--ink     #152A35   blue-black writing ink, chromatic rather than a tinted black
--ink-soft#4E626C   decks and meta
--spot    #8E2B22   madder — case rail and cautions only
```

**Type.** [Newsreader](https://fonts.google.com/specimen/Newsreader) (variable, optical size
6–72) for display *and* body — a face built for news reading, whose optical size axis lets
display sizes tighten properly. [Archivo](https://fonts.google.com/specimen/Archivo) (width axis
75–100) for the functional layer, condensed at label sizes the way newspaper kickers are.

**Rules are hierarchy.** 3px press rule under the masthead, 1px section rule, hairlines between
entries. Thickness carries meaning; nothing is a border for decoration's sake.

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
