# University of Liechtenstein — Academic Paper Template (LaTeX)

A clean, modular LaTeX template for academic papers at the **Liechtenstein
Business School**, built to the *Guidelines for Writing Academic Papers*,
**Version 6.1** (valid from 01/09/2024).
Official guidelines: https://www.uni.li/en/university/services-and-support/legal-resources?id=255.67 (Study Services / LBS).

---

## What you edit vs. what is automated

You only ever touch files in the **root**, in **`chapters/`**, and the
**`tables/`** / **`figures/`** asset folders:

| File / folder | What it holds |
|---------------|---------------|
| `config.tex` | Your metadata (title, name, dates, assessors…) and the on/off switches. |
| `acronyms.tex` | Your abbreviations (defined once, listed automatically). |
| `references.bib` | Your bibliography database. |
| `chapters/abstract.tex` | Abstract + keywords. |
| `chapters/body/` | **Your body chapters — auto-included** (see below). |
| `chapters/list_of_aids.tex` | AI-tool usage table. |
| `chapters/appendix.tex` | Appendix (A-numbered floats). |
| `chapters/declaration.tex` | Declaration of Authorship. |
| `tables/` | Table `.tex` files, `\input` where you discuss them. |
| `figures/` | Figure images (pdf/png/jpg), included with `\includegraphics`. |
| `main.tex` | Orchestrator — you normally don't edit it at all. |

Everything in **`template_system/`** (geometry, fonts, headers, title page,
captions, the auto-loader) is handled for you — **do not edit it**.

```
main.tex                 Orchestrator (rarely touched)
config.tex               Metadata + switches
acronyms.tex             Abbreviations
references.bib           Bibliography
README.md
chapters/
  abstract.tex           Abstract + keywords
  body/                  BODY CHAPTERS — auto-included in order
    01.tex               Introduction
    02.tex               Literature Review (inputs a table)
    03.tex               Analysis (includes a figure)
  list_of_aids.tex       AI-tool usage table
  appendix.tex           Appendix (A-numbered floats)
  declaration.tex        Declaration of Authorship
tables/
  table_01_return_summary_statistics.tex
figures/
  figure_05_asset_price_return_panels.pdf  (+ figure_03, figure_04)
template_system/         INTERNAL — do not modify
  typography.sty         Fonts (Times New Roman), headings, footnotes
  packages.sty           Packages, captions, hyperref, glossaries, biblatex
  layout.sty             Geometry, header/footer, ToC, lists, auto-loader
  titlepage.sty          Official title page
  assets/unili_logo.jpg  University logo
```

## Adding body chapters, tables and figures

**Body chapters** live in `chapters/body/`, numbered `01.tex`, `02.tex`,
`03.tex`, … They are pulled in **automatically, in order** — to add a
chapter, just create the next number (e.g. `04.tex`) and it appears; no edit
to `main.tex` is needed. The heading is whatever you put in `\chapter{...}`
at the top of the file. The loader stops at the first missing number, so keep
the numbering contiguous; it needs no shell-escape and works on Overleaf.

**Tables** go in `tables/<name>.tex` (a `table` environment with `\caption`,
`\label`, the data, and a `\source{}` / `\apanote{}` line). `\input` it where
you discuss it and reference it dynamically:

```latex
\input{tables/table_01_return_summary_statistics}
% ... refer to it as \tabref{tab:return_summary_statistics}
```

**Figures**: drop the image into `figures/` (already on the graphics path)
and include it in a `figure` float:

```latex
\begin{figure}[H]
\caption{Asset price and return panels}
\label{fig:asset_price_return_panels}
\centering
\includegraphics[width=\textwidth]{figure_05_asset_price_return_panels}
\source{Own illustration based on the underlying market data.}
\end{figure}
```

---

## How to build

This template uses the modern stack: **biblatex + biber** for the APA 7
bibliography and **glossaries + makeglossaries** for abbreviations. Run the
full sequence once, then `pdflatex` alone for minor edits:

```
pdflatex main
biber main
makeglossaries main
pdflatex main
pdflatex main
```

In Overleaf, set the compiler to **pdfLaTeX** and the bibliography backend to
**Biber** (Menu → Settings); Overleaf runs the sequence automatically.

> Requires a full TeX distribution (TeX Live / MiKTeX) including `newtx`,
> `biblatex`, `biber`, `glossaries`, `koma-script`, `caption`, `geometry`.

---

## The two main switches (`config.tex`)

```latex
\digitaltrue    % digital submission: continuous, no blank pages,
                % abridged title top-RIGHT.
\digitalfalse   % printed: two-sided, blank page after the title page so
                % the body starts on a right-hand page, abridged title
                % top-OUTER corner, roman front matter → arabic body.

\pluralfalse    % single author  (I / my / me)
\pluraltrue     % group paper    (We / our / us)  → Declaration adapts.
```

---

## Benchmark rules (from the guidelines)

- **Abstract:** approx. **120–170 words**; summarises purpose, argument and
  outcome. Comes after the title page, before the table of contents.
- **Keywords:** **4 to 6**, separated by **semicolons** (`;`).
- **Length** of the paper is measured in *characters including spaces* of the
  text section only (abstract, lists, tables, appendices excluded); the exact
  figure is set in the module description.
- **Layout:** A4 portrait, justified, automatic hyphenation, **1.2** line
  spacing. Margins left/right **2.5 cm**, top **3.0 cm**, bottom **2.0 cm**;
  header/footer **1.25 cm**. Font colour black.
- **Fonts:** Times New Roman — headings **12 pt** bold/italic, body **11 pt**
  (6 pt before each paragraph), footnotes **9 pt**.
- **Structure:** decimal numbering; a heading needs at least two sub-headings
  (e.g. 2.1.1 *and* 2.1.2). The table of contents shows three decimal levels
  (1, 1.1, 1.1.1).

### Citations — page numbers are mandatory

The **precise page number** must be given for references to specific
statements — for **both direct and indirect** quotations.

```latex
\parencite[42]{key}        % indirect quotation, p. 42  → (Author, 2024, p. 42)
\textcite[42]{key}         % narrative: Author (2024, p. 42)
\parencite[42--43]{key}    % page range
\parencite{key}            % whole work → page number may be omitted
```

Use **only one citation style** throughout (this template ships APA 7;
Chicago/CMOS is the alternative for Tax-department papers). Avoid secondary
citations — always obtain the primary source.

### Tables & figures

- Each table/figure must be **referred to and discussed** in the text. Use the
  dynamic helpers `\tabref{label}` / `\figref{label}` / `\secref{label}` —
  **never** phrases like “the table below” or “as the following figure shows”.
- The **caption** sits above the asset and is the only text that appears in the
  List of Figures / List of Tables.
- The **source** goes on its own line *underneath* the asset via
  `\source{...}`; it is deliberately excluded from the lists. Use `\apanote{...}`
  for an APA explanatory note.

### Abbreviations

Define each acronym once in `acronyms.tex`. First use of `\gls{key}` prints the
full term with the short form in parentheses; later uses print the short form
only. The List of Abbreviations is sorted alphabetically. Everyday
abbreviations (“e.g.”, “cf.”, “etc.”) are **omitted**.

### List of Aids (AI tools)

Every AI tool used must be named with its product name and a short description
of how it was used (`chapters/03_list_of_aids.tex`). Omitting a tool you used is
treated as an act of deception under the guidelines.
