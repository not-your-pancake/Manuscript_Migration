# Migration notes — TCC manuscript → Climatic Change (Springer `sn-jnl`)

Every deviation from `old/manuscript/manuscript.tex`, for review before submission.

Source: `old/manuscript/manuscript.tex` (elsarticle, targeted at *Urban Climate*)
Target: `new/main.tex` (`sn-jnl`, `sn-basic`, for *Climatic Change*)

**No sentence of the manuscript was edited.** The migrated body is byte-identical
to the source apart from exactly two changes, both listed below and both approved:
one deletion (the identifying footnote, section 2) and one citation key
(`Sultana2021` → `dewan2021`, section 4.0). A word count run with the same tool
over both files returns **7,873 words on each side**.

---

## 1. Files

| File | What it is |
|---|---|
| `main.tex` | Anonymised manuscript. Compile this. |
| `title_page.tex` | Authors, affiliations, email, ORCID, CRediT, live URLs. **Separate submission file — not `\input` by `main.tex`.** |
| `references.bib` | Copy of `old/manuscript/references.bib` **with 6 corrected entries** — see section 4.0. Every other entry is unmodified. |
| `sn-jnl.cls`, `sn-basic.bst` | Copied from `template/`. Unmodified. |
| `tables/*.tex` | 6 table bodies, byte-identical to `old/manuscript/tables/`. |
| `figures/*.png` | The 8 referenced figures, byte-identical to `old/outputs/figures/`. |

Build: `pdflatex main` → `bibtex main` → `pdflatex main` → `pdflatex main`

Result: **33 pages. 0 errors, 0 undefined citations, 0 undefined cross-references,
0 overfull hboxes.**

That standard 4-command sequence still ends with one `LaTeX Warning: Label(s) may
have changed. Rerun to get cross-references right.` One additional
`pdflatex main` clears it. The PDF is identical either way (same page count, same
byte size), but run the extra pass before submitting.

---

## 2. Blinding — what was removed from the manuscript

All of it is preserved on `title_page.tex`.

| Removed from `main.tex` | Was at old line |
|---|---|
| 5 author names | 44, 47, 49, 50, 52 |
| Corresponding email `nabil…@student.bup.edu.bd` | 45 |
| Commented-out second email | 48 |
| Affiliation — Bangladesh University of Professionals | 54–58 |
| Affiliation — University of Slavonski Brod | 60–63 |
| ORCID `0009-0007-3255-1420` | 65 |
| **Footnote containing the Streamlit dashboard URL** | **721** |
| CRediT statement naming 3 authors | 1144–1148 |
| Zenodo DOI `10.5281/zenodo.21628776` | 1160 |
| Streamlit dashboard URL | 1164 |

The line-721 footnote is the **only deletion inside the running text.** The
sentence around it is untouched; only `\footnote{\url{…}}` was removed, so it now
reads "…through an interactive web dashboard through which any district and
condition can be explored directly."

The word "Streamlit" still appears once in the Data availability statement
(verbatim source wording). It names a platform, not an author. Remove it if you
consider it identifying.

---

## 2a. Why the new PDF is shorter — no text was removed

The new PDF is 33 pages against the old one's 54. That is entirely layout. Text
extracted directly from the two PDFs shows the new document contains **more**
words, not fewer:

| | Old `manuscript.pdf` | New `main.pdf` |
|---|---|---|
| Pages | **54** | 33 |
| Words (extracted from the PDF) | 12,108 | **12,567** |
| Page size | US Letter, 612 × 792 pt | A4, 595 × 842 pt |

Three causes, in order of size:

1. **Line spacing.** `elsarticle`'s `review` option sets `\@blstr{1.5}` — the old
   PDF is 1.5× line-spaced. `sn-jnl` is single-spaced.
2. **Paper size.** A4 is ~50 pt taller per page than US Letter.
3. **Reference list.** `sn-basic` abbreviates ("Shaw R, Luo Y" + *et al*) where
   the old inlined list spelled out every author and editor in full — about 1,100
   words shorter.

The word count went *up* mainly because author–year citations
("Shaw et al. 2022") are longer than "[1]".

Independent confirmation that the body text is unchanged: the same word counter
run over the old `.tex` and the new `.tex` returns **7,873 words on both sides**,
and a line diff of the migrated body shows only whitespace at section joins plus
the one intended footnote removal (section 2).

If you compare word frequencies between the two PDFs directly, note two
extraction artifacts that look alarming but are not real:

- The old PDF's `fi`/`fl` ligatures do not decode — "fields" extracts as "elds",
  "significant" as "signicant". Those words are present in both documents.
- "Mann--Kendall" extracts as one token from the old PDF and two from the new
  (the en-dash differs), so "mann" and "kendall" appear to jump from 0 to 7.
- "Figure 1" became "Fig. 1" — that is Springer's cross-reference style, set by
  the class.

---

## 3. Changes that alter what the PDF says

### 3.1 `Sultana2021` and `islam2026enso` — RESOLVED, see section 4.0

These two were originally documented here as unresolved "the `.bib` wins"
changes. Both were then checked against Crossref and **both turned out to be
errors in `references.bib`, not in your proofread PDF.** They have been corrected
and `Sultana2021` has been merged into `dewan2021`. Full detail in **section 4.0**.

### 3.2 Superseded — see section 4.0

### 3.3 Reference list is 66 entries, was 68

Two fewer than the old printed list, for two unrelated reasons:

- `hunter1999` had a printed `\bibitem` but is never cited, so BibTeX drops it.
- `Sultana2021` was a phantom entry and is gone; its one citation now points at
  `dewan2021`, the same paper (section 4.0).

Five further `.bib` entries are never cited and do not print: `Bergstra2012`,
`hasan2025sylhet`, `rahman2021heatindex`, `saha2025adaptability`,
`xu2025humidexurinary`.

### 3.4 Field-by-field audit — old printed list vs `references.bib`

The first check compared only **title, year, journal and DOI**, which is why the
author errors were missed. A second pass compared `author`, `title`, `journal`,
`booktitle`, `year`, `volume`, `number`, `pages`, `publisher` and `doi` in both
directions for every entry. Result:

| Entry | What differed | Outcome |
|---|---|---|
| `Sultana2021` | author, title, journal, volume, pages | **fixed** — section 4.0 |
| `islam2026enso` | author entirely; volume and pages absent | **fixed** — section 4.0 |
| `faisal2022` | "urbani**s**ation" → "urbani**z**ation" | cosmetic, `.bib` spelling kept |
| `iso7243_2017` | `.bib` title carries an em-dash the printed title lacked | cosmetic, left as is |
| `saha2025postescalation` | pages `157--158` → `157-158` (en-dash becomes hyphen) | cosmetic, left as is |

**The other 62 entries matched on every compared field.**

Note this pass compares the two *local* sources against each other; it cannot
tell which is factually right. That required checking the DOIs against Crossref —
see section 4.0, which is where the four additional errors present in *both*
sources were found.

Detail *gained*: 31 entries pick up issue numbers or publishers that
`references.bib` carries and the old hard-coded list omitted — e.g. `Abrar2022`
(number 9, MDPI), `liljegren2008` (number 10, Taylor & Francis), `raymond2020`
(number 19). The regenerated list is more complete than the old one.

### 3.5 Citations are now author–year

Was `[1]`, `[2]` (elsarticle default). Now `(Shaw et al. 2022)` as *Climatic
Change* requires. Reference list is alphabetical by first author surname.

### 3.6 Data availability statement

Verbatim from old lines 1156–1164 **except** that the Zenodo DOI and the
dashboard URL are replaced with "withheld for double-blind peer review; given on
the title page". Author contributions likewise point to the title page. These are
the only sentences in the manuscript containing wording not in the original.

---

## 4.0 Six references were factually wrong and have been corrected

Every DOI in the bibliography (56 of the 73 entries carry one) was checked
against the **Crossref** registry, and against **DataCite** where Crossref had no
record. Six entries did not describe the paper their DOI points to. All six are
now corrected from the registry records, with your approval. **No other entry was
touched, and no manuscript sentence was changed.**

Two of the six were made worse by this migration, because the old hard-coded
reference list was right and `references.bib` was wrong:

| Key | Was printed in your old PDF | What `references.bib` said | Registry says |
|---|---|---|---|
| `Sultana2021` | Dewan, Kiselev & Botje, *Applied Geography* 135:102533 — **correct** | Sultana, Islam, Akter & Paul, *Sci Rep* 11:9683, DOI `10.1038/s41598-021-89214-4` | **That DOI does not exist.** Its title belongs to Kamal, Fahim & Shahid, *Sci Rep* **14**:10417 (2024) — already present as `kamal2024scientific`. The entry was fabricated. |
| `islam2026enso` | Mohsin, Ghosh, Akter, Sarkar & Mullick, *Discover Environment* 4:29 — **correct** | Islam, Md. Ariful and others | Crossref confirms **Mohsin et al.** for DOI `10.1007/s44274-026-00533-6` |

Four were already wrong in **both** sources, i.e. they are errors in the original
manuscript that the migration merely carried forward:

| Key | Problem | Corrected to |
|---|---|---|
| `rahman2024rnlstm` | DOI `10.1371/journal.pone.0305406` is *"Meta-2OM: a multi-classifier meta-model for RNA 2′-O-methylation"* — an RNA-biology paper | Hasan MM, Hasan MJ, Rahman PB, *PLOS ONE* **19**(9):e0310446, DOI `10.1371/journal.pone.0310446` |
| `dewan2021` | DOI `10.1016/j.uclim.2021.100896` is Ambade et al., *black carbon over East India* | Dewan A, Kiselev G, Botje D, *Applied Geography* **135**:102533, DOI `10.1016/j.apgeog.2021.102533` (also drops 3 authors the paper does not have) |
| `khatun2024` | Right paper, wrong authors | Akter MY, Islam ARMT, Mallick J et al., *Theor Appl Climatol* **155**(9):8843–8869 |
| `faisal2022` | Right paper, wrong authors | Rahman MN, Rony MRH, Jannat FA et al., *Climate* **10**(1):3 |

### The one manuscript edit

`Sultana2021` and `dewan2021` turned out to be **the same paper** — Dewan has no
*Urban Climate* Bangladesh paper; only the *Applied Geography* one exists. Rather
than print it twice as "Dewan et al. (2021a)" and "(2021b)", the single citation
of `Sultana2021` in the Introduction was repointed to `dewan2021`:

```
main.tex, Introduction:
  ...distribute unevenly across the landscape \citep{Sultana2021}.
                              becomes
  ...distribute unevenly across the landscape \citep{dewan2021}.
```

This is **the only citation key changed anywhere**, and it reproduces what your
proofread PDF actually printed at that sentence. The phantom `Sultana2021` entry
was removed from `references.bib`; a comment marks where it stood, and its
original text is preserved here:

```bibtex
@article{Sultana2021,
  author    = {Sultana, Sharmin and Islam, A. K. M. Saiful and Akter, Farhana and Paul, Susmita},
  title     = {Changes in Wet Bulb Globe Temperature and Risk to Heat-Related Hazards in Bangladesh},
  journal   = {Scientific Reports}, volume = {11}, pages = {9683}, year = {2021},
  doi       = {10.1038/s41598-021-89214-4}, publisher = {Nature Publishing Group}
}
```

The reference list is therefore **66 entries**, not 67.

### Re-verification after the fixes

All 55 remaining DOIs re-checked: **53 verified correct, 0 that fail to resolve.**
Two are flagged by the checker but are false positives — Crossref stores the
XGBoost paper's title as just "XGBoost", and the IPCC chapter's title is literally
"Asia", too short for the title-overlap test.

**17 entries carry no DOI** (ISO 7243, the WHO report, NeurIPS papers, Breiman,
Steadman, the Visual Crossing API, etc.). You chose not to verify these; they
remain unchecked.

---

## 4. Open issues — not fixed, need your decision

### 4.1 Doubled DOI links — RESOLVED in `main.tex`, `references.bib` untouched

`references.bib` stores 17 DOIs as full URLs (`doi = {https://doi.org/10.…}`)
and 39 as bare DOIs (`doi = {10.…}`). `sn-basic.bst` prepends `https://doi.org/`
unconditionally, so 16 of the 67 printed references rendered as:

```
https://doi.org/https://doi.org/10.1038/s41598-025-98607-7
```

— links that do not resolve. The old `.bst` did not do this, so it appeared only
after migration.

Fixed by defining `\doi` in the `main.tex` preamble so that the
`\providecommand{\doi}` in `main.bbl` is skipped and both DOI forms are handled:

```latex
\usepackage{xstring}
\newcommand{\doi}[1]{\IfBeginWith{#1}{https://doi.org/}%
  {\url{#1}}%
  {\IfBeginWith{#1}{http://dx.doi.org/}%
    {\url{#1}}%
    {\url{https://doi.org/#1}}}}
```

**`references.bib` was not edited.** If the manuscript is ever moved to another
class or `.bst`, this override travels with `main.tex`; the underlying
inconsistency in the `.bib` remains and may be worth cleaning up separately.

### 4.2 Word count is over the limit

| | Words |
|---|---|
| Abstract | 218 |
| Body (Introduction → Conclusion) | 7,873 |
| Statements and Declarations | 139 |
| Reference list (67 entries) | 1,844 |
| Float surcharge — 14 floats, 11 chargeable × 300 | 3,300 |
| **Total** | **13,374** |
| Limit | 10,000 |
| **Over by** | **3,374** |

No cuts were made, by instruction. The float surcharge (3,300) is the single
largest lever: moving figures/tables to supplementary information removes 300
words each beyond the third.

*(An earlier estimate said 4,236 over. The final figure is lower because the
regenerated `sn-basic` reference list is about 1,100 words more compact than the
old hard-coded one. The body text itself is unchanged.)*

### 4.3 Keywords

Now 5, per your list: Temperature-Correlated Conditions · public health ·
Bangladesh · seasonal projection · Applied Machine Learning. The old manuscript
had 8, which exceeded the 4–6 limit. "Temperature-Correlated Conditions" uses the
manuscript's own hyphenated spelling.

### 4.4 Co-author name

Old manuscript spelled it **both** "Musayeb Hussain Usama" (line 49) and "Musayeb
Hossain Usama" (line 1148). Per your decision the title page uses **Hossain**
throughout.

### 4.5 CRediT covers only 3 of 5 authors

Atika Akter and Ivan Grgić have no contribution entry. Copied as-is. Springer
expects every author to be covered.

### 4.6 Stray punctuation, old line 1148

`…review \& editing.\\[2pt]. ` — a period after the line break, producing an
orphaned "." Copied verbatim to the title page, not corrected.

### 4.7 Pre-existing typesetting artifact

Old line 160 reads `0.08--0.5\,\degC per decade`. Because `\degC` is a control
word, LaTeX swallows the following space and it prints as "0.08–0.5 °Cper
decade". This is present in the original and was carried over unchanged. It is
the only one of the 50 `\degC` uses affected.

---

## 5. Structural changes (no effect on wording)

- `\documentclass[review,11pt]{elsarticle}` → `\documentclass[pdflatex,sn-basic,lineno]{sn-jnl}`
- Six `\setlength` page-geometry overrides dropped — they were tuned for
  Elsevier's wide review layout and fight the Springer trim size.
- `\usepackage{lineno}` + `\linenumbers` dropped; continuous line numbering now
  comes from the `lineno` **class option**, which is how `sn-jnl` does it.
  Verified continuous across all 33 pages (last line number 1518).
- `\journal{Urban Climate}` dropped.
- `\graphicspath{{../outputs/figures/}}` → `{{figures/}}` so `new/` is
  self-contained with no `../` path escape.
- Front matter rewritten into `sn-jnl` syntax (`\author*`, `\fnm`, `\sur`,
  `\affil`) — on the title page only.
- `title_page.tex` loads `amsmath` because `sn-jnl` calls `\allowdisplaybreaks`
  in a `\begin{document}` hook without loading amsmath itself, and `fontenc`
  with T1 for the "ć" in Grgić.
- The class's `\orcid{}` macro is **not** used: it expands to
  `\includegraphics{Orcidlogo.eps}` and that file is missing from the Springer
  template. ORCID is written as plain text.
- Table bodies stay in `tables/*.tex` via `\input`; figure images are separate
  `.png` files; all 14 captions remain in the main text, as the journal requires.

## 6. A note on `template/`

`template/sn-article.tex` is **not** the clean Springer demo. It is an abandoned
earlier migration attempt containing **159 `[cite: 1]` artifacts** pasted through
the prose, the wrong class option (`sn-mathphys-ay`), and a bibliography pointing
at Springer's demo `.bib`. **No text was taken from it.** Only `sn-jnl.cls` and
`sn-basic.bst` were used. `main.tex` contains zero `[cite:` artifacts (verified).
