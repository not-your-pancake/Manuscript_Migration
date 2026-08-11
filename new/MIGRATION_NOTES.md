# Migration notes — TCC manuscript → Climatic Change (Springer `sn-jnl`)

Every deviation from `old/manuscript/manuscript.tex`, for review before submission.

Source: `old/manuscript/manuscript.tex` (elsarticle, targeted at *Urban Climate*)
Target: `new/main.tex` (`sn-jnl`, `sn-basic`, for *Climatic Change*)

**The manuscript text was not edited.** The migrated body is byte-identical to
the source apart from one deletion listed under "Blinding" below. A word count
run with the same tool over both files returns **7,873 words on each side**.

---

## 1. Files

| File | What it is |
|---|---|
| `main.tex` | Anonymised manuscript. Compile this. |
| `title_page.tex` | Authors, affiliations, email, ORCID, CRediT, live URLs. **Separate submission file — not `\input` by `main.tex`.** |
| `references.bib` | Byte-identical copy of `old/manuscript/references.bib`. Unmodified. |
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

## 3. Changes that alter what the PDF says

### 3.1 `Sultana2021` now cites a different paper — **please confirm**

The old manuscript's reference list was hard-coded, not generated from the
`.bib`, and the two disagreed:

| | Author / title / venue |
|---|---|
| Old printed list (`\bibitem`) | Dewan, Kiselev, Botje — *Diurnal and seasonal trends… surface urban heat islands in large Bangladesh cities*, **Applied Geography** 135, 102533, DOI `10.1016/j.apgeog.2021.102533` |
| `references.bib` (now used) | Sultana, Islam, Akter, Paul — *Changes in Wet Bulb Globe Temperature and Risk to Heat-Related Hazards in Bangladesh*, **Scientific Reports** 11, 9683, DOI `10.1038/s41598-021-89214-4` |

Per instruction the `.bib` was treated as authoritative and left unedited, so the
citation at old line 139 (unplanned urbanisation intensifying heat stress) now
resolves to Sultana et al.

Two further points for the record:

- The old printed list contained the **Dewan paper twice** — once correctly under
  key `dewan2021` (*Urban Climate* 38, 100896) and once under `Sultana2021` with
  an Applied Geography DOI that appears **nowhere in `references.bib`**.
- The `.bib` entry for `Sultana2021` has a title nearly identical to
  `kamal2024scientific` but different authors, year and DOI. Worth confirming
  that `10.1038/s41598-021-89214-4` resolves to the Sultana paper.

### 3.2 Reference list is 67 entries, was 68

`hunter1999` had a printed `\bibitem` but is never cited, so BibTeX drops it.
Six further `.bib` entries are never cited and do not print: `Bergstra2012`,
`hasan2025sylhet`, `hunter1999`, `rahman2021heatindex`, `saha2025adaptability`,
`xu2025humidexurinary`.

### 3.3 `faisal2022` spelling

Printed list said "urbanisation"; `.bib` says "urbanization". The `.bib` wins.

### 3.4 Citations are now author–year

Was `[1]`, `[2]` (elsarticle default). Now `(Shaw et al. 2022)` as *Climatic
Change* requires. Reference list is alphabetical by first author surname.

### 3.5 Data availability statement

Verbatim from old lines 1156–1164 **except** that the Zenodo DOI and the
dashboard URL are replaced with "withheld for double-blind peer review; given on
the title page". Author contributions likewise point to the title page. These are
the only sentences in the manuscript containing wording not in the original.

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
