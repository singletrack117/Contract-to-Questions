# A&GEL Contract-to-Questions 📄

A single-file browser tool, styled in the A&GEL brand identity, that turns a
contract into a clean **question list** — one paragraph per clause. Open
`index.html` in any modern browser. No install, no server, no build step.

The goal is to strip out every carriage return *except* the ones that separate
one clause from the next. Each clause becomes a single paragraph that starts
with its clause number, with the sentences inside separated only by `.` — no
line breaks. This makes the text easy to paste into a downstream
question-generation step.

## Example

Input:

```
1. General: This clause is the start.
1.1 This is the subclause which should be treated as clause 1.
2. Scope of agreement: this clause is the second clause
```

Output (default "top-level" grouping):

```
1. General: This clause is the start. 1.1 This is the subclause which should be treated as clause 1.
2. Scope of agreement: this clause is the second clause
```

Sub-clause `1.1` is folded into clause 1, and the two clauses stay separated by
a single carriage return.

## Features

- **Upload a contract**: Word (`.docx`), PDF (`.pdf`), email (`.eml`), or plain
  text (`.txt`, `.md`) — drag-and-drop or click to browse — or just paste the text.
- **Clause flattening**: multi-line clauses are joined into one paragraph,
  keeping the leading clause number.
- **Sub-clause grouping**: choose how deep the numbering splits into separate
  clauses:
  - **Top-level only** (default) — `1.1`, `1.1.1`, … all fold into clause `1`.
  - **2nd-level** — split down to `1.1`; deeper items (`1.1.1`, …) fold into it.
  - **3rd-level** — split down to `1.1.1`; deeper items fold into it.
  - **Every numbered item is its own clause** — no grouping.
- **Heading detection**: recognises `1.`, `2)`, `10.` style numbering as well as
  `Clause 3`, `Section 2.1`, `Article 5` headings.
- **Live stats**: clauses detected, line breaks removed, words, and characters.
- **Preview cards** plus a copyable flattened-text box; **Copy** to clipboard or
  **Download .txt**.
- **Load example** button to see the conversion on a small sample.

## Usage

1. Open `index.html` in a browser (or serve the folder: `python3 -m http.server`).
2. Drop in a contract file or paste the text.
3. Pick a clause-grouping option, then click **Convert to Question List**.
4. **Copy** or **Download .txt** the flattened result.

## Notes

- Everything runs locally in your browser — nothing is uploaded to a server.
- Word (`.docx`) clause numbers are read straight from the document's XML
  (via JSZip), so Word's automatic list numbering — including style-based and
  multi-level numbering — is reconstructed rather than lost. PDF parsing uses
  pdf.js; a mammoth.js pass is kept as a fallback.
- Word and PDF parsing use on-demand CDN libraries (JSZip, mammoth.js, pdf.js)
  and degrade gracefully with a clear message if those cannot load; paste the text
  as a fallback.
