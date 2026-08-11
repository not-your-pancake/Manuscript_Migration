# Manuscript migration — TCC paper → Climatic Change (Springer)

## What this project is

Moving a finished, already-proofread LaTeX manuscript out of its old journal
format and into the Springer Nature template (`sn-jnl`), for submission to the
journal *Climatic Change*.

## Folder rules

- `old/` — the original manuscript. **READ-ONLY.** Never edit it, never delete it.
- `template/` — the official Springer Nature template. **READ-ONLY.**
- `new/` — the only folder you write to. It must be self-contained: `.cls` and
  `.bst` files sit next to `main.tex`.
- Never create a file or folder name containing spaces, brackets or parentheses.
  LaTeX breaks on them.

## The rule that matters most

The text in `old/` has already been checked line by line. Copy it **verbatim**.

- Do not reword, shorten, expand or improve any sentence.
- Do not correct grammar or spelling, even if it looks wrong to you.
- Do not reorder paragraphs, sections, figures or references.
- Do not change any number, unit, equation, symbol or citation key.
- Do not change the contents of the `.bib` file.

If something looks wrong, **stop and tell me**. Do not fix it yourself.

## Journal requirements (Climatic Change)

- Class option: `sn-basic` — author–year citations. **Not** a numbered style.
- Bibliography style: `sn-basic.bst`.
- In-text citations by name and year in parentheses, e.g. (Rahman 2021).
- Reference list alphabetical by first author surname. DOIs as full links.
- Decimal headings, maximum three levels deep.
- Continuous line numbering must be switched on.
- Abstract 150–250 words. 4 to 6 keywords.
- Hard limit: 10,000 words total, counting all text, references, figures and
  tables. Each figure or table from the fourth onward counts as 300 words.
- **Double-blind review.** The manuscript file must contain no author names, no
  affiliations, no emails, no ORCIDs, no acknowledgements, no funding text, and
  no repository or dashboard URLs that could identify the authors. All of that
  goes into a separate `title_page.tex`.
- Figures and tables are separated from the main text file. Captions stay in the
  main text.
- A "Statements and Declarations" section goes after the references.
- A Data Availability Statement is required.

## Build commands (run inside `new/`)

```
pdflatex main
bibtex main
pdflatex main
pdflatex main
```

## How to work

- Small steps. Compile after every section you move.
- If a compile fails, show me the real error line from the `.log` before you fix it.
- Never delete a file or comment out content just to make an error go away.
- Commit to git after every successful compile, with a short message.
- Never touch anything outside this project folder.
- When you are unsure, ask. Do not guess and continue.
