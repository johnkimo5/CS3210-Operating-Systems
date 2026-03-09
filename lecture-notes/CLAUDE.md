# Claude Instructions: CS3210 Lecture Notes

When generating LaTeX lecture notes for this repository, follow the template established in `lec12_schedulers.tex` exactly. The notes below describe every structural and stylistic decision.

---

## Document Class and Packages

```latex
\documentclass[11pt]{article}

\usepackage[margin=1in]{geometry}
\usepackage{amsmath, amssymb}
\usepackage{listings}
\usepackage{xcolor}
\usepackage{hyperref}
\usepackage{enumitem}
\usepackage{titlesec}
\usepackage{parskip}
\usepackage{booktabs}
```

Do NOT add packages beyond this set unless the content strictly requires it. Additional permitted packages:

- `tcolorbox` — for coloured callout boxes (see **Callout Boxes** below).
- `tikz` — for diagrams and visuals (see **Visuals and Diagrams** below).

Do not use `mdframed` or `fancyhdr`.

---

## Code Listing Style

```latex
\lstset{
    basicstyle=\ttfamily\small,
    keywordstyle=\color{blue}\bfseries,
    commentstyle=\color{gray}\itshape,
    stringstyle=\color{red},
    frame=single,
    breaklines=true,
    showstringspaces=false,
    tabsize=4
}
```

When inserting a code block inside the document, always use:

```latex
\begin{lstlisting}[language=C, caption={Descriptive caption here}]
...
\end{lstlisting}
```

For inline code use `\texttt{symbol_or_function_name}`.

---

## Title Block

```latex
\title{\textbf{CS3210: Operating Systems} \\ Lecture N --- Topic Name}
\author{John Kim \\ Georgia Institute of Technology}
\date{Month DD, YYYY}
```

- Replace `N` with the lecture number and `Topic Name` with the lecture topic.
- Use the actual lecture date.

---

## Document Opening

```latex
\begin{document}
\maketitle
\tableofcontents
\newpage
```

Always include `\maketitle`, `\tableofcontents`, and `\newpage` in that order before any content.

---

## Section Formatting

Every top-level section must be preceded by a visual divider comment:

```latex
% ============================================================
\section{Section Title}
% ============================================================
```

Use `\subsection{}` and `\subsubsection{}` freely. Use `\subsubsection*{}` (unnumbered) for minor sub-topics that don't merit their own TOC entry.

---

## Writing Style

- Write in **dense prose**, not bare bullet lists. Bullet lists are used to enumerate items, not to substitute for explanation.
- Use `\textbf{term}` to introduce and emphasise key terms on first use.
- Use `\emph{phrase}` for secondary emphasis or to flag important qualifications.
- Every major concept should have a one-sentence plain-English explanation before any formal definition or code.
- Prefer concrete examples drawn directly from the lecture (xv6 code paths, real OS names like Linux/CFS, etc.).
- When presenting tradeoffs or comparisons, use a `booktabs` table:

```latex
\begin{center}
\begin{tabular}{ll}
\toprule
\textbf{Column A} & \textbf{Column B} \\
\midrule
row & row \\
\bottomrule
\end{tabular}
\end{center}
```

- For multi-step processes or ordered reasoning, use `\begin{enumerate}`.
- Nested lists are fine; keep nesting to at most two levels.

---

## Mathematics

- Inline math: `$expression$`
- Display math for important equations or flow diagrams:

```latex
\[
\text{shell (user)} \xrightarrow{\text{save ctx}} \text{shell (kernel)} \xrightarrow{\text{swtch}} \text{scheduler}
\]
```

---

## Callout Boxes

Use `tcolorbox` to highlight **key ideas**, **key tips**, and **key intuitions** — things worth remembering at a glance. Do not overuse them; one to three per major section is a good ceiling.

Define these three box styles in the preamble:

```latex
\usepackage{tcolorbox}

\newtcolorbox{keyidea}[1][]{colback=blue!5, colframe=blue!50!black,
    fonttitle=\bfseries, title=Key Idea, #1}

\newtcolorbox{keytip}[1][]{colback=green!5, colframe=green!50!black,
    fonttitle=\bfseries, title=Tip, #1}

\newtcolorbox{keyintuition}[1][]{colback=orange!5, colframe=orange!50!black,
    fonttitle=\bfseries, title=Intuition, #1}
```

Usage in the document body:

```latex
\begin{keyidea}
Context switching is expensive because the CPU must save and restore
the full register set, TLB entries may be flushed, and caches go cold.
\end{keyidea}
```

Keep the text inside each box to one short paragraph (2–4 sentences). These boxes should distill an insight, not replace the surrounding prose.

---

## Visuals and Diagrams

Lecture notes should include **TikZ diagrams** wherever a visual mental model would help the reader understand a concept more accurately than prose alone. Aim for at least one or two diagrams per lecture. Good candidates include:

- **Architecture diagrams** — memory layouts, address-space structure, page-table walks.
- **Flow diagrams** — system-call paths, interrupt handling sequences, scheduler transitions.
- **State machines** — process states and transitions, lock states.
- **Stack/memory illustrations** — showing how frames are laid out, where registers are saved, etc.
- **Comparison diagrams** — side-by-side visuals of two approaches (e.g. monolithic vs. microkernel).

Use `tikz` with the `arrows.meta`, `positioning`, and `shapes.geometric` libraries as needed:

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta, positioning, shapes.geometric}
```

General guidelines:

- Every diagram must have a brief caption or label (use `\begin{figure}[h] ... \caption{} ... \end{figure}`).
- Use clear, consistent colours: blue for user-space, red/orange for kernel-space, gray for hardware is a good default palette, but adapt as needed.
- Keep diagrams simple and readable — they should clarify, not overwhelm.
- When a process or transition has multiple steps, number them in the diagram.
- Prefer diagrams over long verbal descriptions of spatial relationships or multi-step flows.

---

## Mandatory Closing Sections

Every set of notes must end with these three sections, in this order:

### 1. Summary

```latex
% ============================================================
\section{Summary}
% ============================================================

\begin{itemize}
    \item \textbf{Key concept}: one-sentence takeaway.
    ...
\end{itemize}
```

The summary should be a concise bulleted list — one item per major concept covered. Lead each bullet with `\textbf{concept name}:`.

### 2. References

```latex
% ============================================================
\section{References}
% ============================================================

\begin{itemize}
    \item Author. ``Title.'' \textit{Source}. Year.
    ...
\end{itemize}
```

Include any textbooks, papers, or URLs referenced in the lecture. For URLs use `\url{}`.

### 3. Personal Notes (blank)

```latex
% ============================================================
\section*{Personal Notes}
% ============================================================
% Add your own notes below this line
```

Always include this blank section at the very end. It is for the student to fill in by hand.

---

## Compilation

After writing the `.tex` file, compile **twice** with:

```bash
pdflatex -interaction=nonstopmode -output-directory=/path/to/output file.tex
pdflatex -interaction=nonstopmode -output-directory=/path/to/output file.tex
```

Two passes are required to resolve the table of contents and cross-references. Check that the second pass produces zero lines starting with `!`. Intermediate files (`.aux`, `.log`, `.toc`, `.out`) can be discarded once the PDF is confirmed good.

---

## File Naming Convention

```
lecNN_topic_slug.tex
```

Examples: `lec12_schedulers.tex`, `lec13_waiting.tex`. Use lowercase, underscores, no spaces.
