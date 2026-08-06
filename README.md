# A&GEL Checklist-to-Questions 📋

A single-file browser tool, styled in the A&GEL brand identity, that flattens a
**checklist** — a disclosure schedule, a requirements list, or any numbered
document — into a clean **list of questions**. Every requirement becomes one
line, tagged with its **level** (top → 5th), so the list can be grouped, folded,
or compared line-by-line against your precedents. Open `index.html` in any
modern browser. No install, no server, no build step.

## What it does

Each line of the source is classified into a hierarchy level by the marker it
starts with, deepest last:

| Level | Marker family            | Example                                   |
|:-----:|--------------------------|-------------------------------------------|
| 1     | `PART` / `SECTION` heading | `PART 1 OF THE 5TH SCHEDULE SFR: FRONT COVER` |
| 2     | integer item             | `1.`  `2)`                                 |
| 3     | lettered item            | `(a)`  `(b)`                               |
| 4     | roman item               | `(i)`  `(ii)`                              |
| 5     | upper-letter item        | `(A)`  `(B)`                              |

(A plain numeric contract that uses `1 / 1.1 / 1.1.1` is handled too — the
shallowest marker present becomes level 1, so dotted numbering maps straight
onto levels.)

You then choose how deep to split:

- **Top level only** — everything folds under its PART / top heading.
- **Down to 2nd / 3rd / 4th / 5th level** — split to that level; anything deeper
  folds into its parent item.
- **Every line a level** — each line becomes its own question (the full list).

## Example

Input:

```
PART 1 OF THE 5TH SCHEDULE SFR: FRONT COVER
1. On the front cover of the prospectus, provide:
(a) the date of registration of the prospectus;
(b) the following statements:
(i) "This document is important...";
(ii) "A copy of this prospectus has been lodged...";
(c) the name of the corporation.
```

Output (grouped **down to 3rd level** — the roman `(i)/(ii)` fold into `(b)`):

```
PART 1 OF THE 5TH SCHEDULE SFR: FRONT COVER
1. On the front cover of the prospectus, provide:
(a) the date of registration of the prospectus;
(b) the following statements: (i) "This document is important..."; (ii) "A copy of this prospectus has been lodged...";
(c) the name of the corporation.
```

## Word checklists laid out as tables

Disclosure schedules are usually Word tables with columns such as

```
[ №  |  DISCLOSURE REQUIREMENT  |  SECTION  |  PAGE(S)  |  REMARKS ]
```

The tool reads these **column-aware**: it keeps only the number column and the
*Disclosure Requirement* column — the questions — and drops the cross-reference
columns. Word's automatic `(a) / (i) / (A)` list numbering is reconstructed from
the document's XML (via JSZip), and full-width rows are treated as `PART`
headings. The result is one clean, correctly-levelled question per requirement,
with none of the "Cover Page / Paragraph 7 / -" reference noise.

## Features

- **Upload a checklist**: Word (`.docx`), PDF (`.pdf`), email (`.eml`), or plain
  text (`.txt`, `.md`) — drag-and-drop or click to browse — or just paste the text.
- **Level detection** for `PART`, `1.`, `(a)`, `(i)`, `(A)` and `1.1.1` markers,
  including disambiguation of a lettered list that is interrupted by a roman
  sub-list and then resumes (`(a) (b) (i) (ii) (c) (d)`).
- **Group by level**: top → 5th, or every line a level.
- **Live stats**: questions produced, distinct levels, words, and characters.
- **Preview cards** — each question shown with its level chip, marker, and an
  indent that mirrors its depth — plus a copyable flattened-text box; **Copy** to
  clipboard or **Download .txt**.
- **Load example** button to see the conversion on a small disclosure sample.

## Usage

1. Open `index.html` in a browser (or serve the folder: `python3 -m http.server`).
2. Drop in a checklist file or paste the text.
3. Pick a grouping level, then click **Flatten to Questions**.
4. **Copy** or **Download .txt** the flattened result.

## Notes

- Everything runs locally in your browser — nothing is uploaded to a server.
- Word (`.docx`) parsing reads clause numbers straight from the document's XML
  (via JSZip), reconstructing Word's automatic and multi-level list numbering
  rather than losing it. PDF parsing uses pdf.js; a mammoth.js pass is kept as a
  fallback.
- Word and PDF parsing use on-demand CDN libraries (JSZip, mammoth.js, pdf.js)
  and degrade gracefully with a clear message if those cannot load; paste the
  text as a fallback.
</content>
</invoke>
