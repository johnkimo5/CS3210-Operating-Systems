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

Do NOT add packages beyond this set unless the content strictly requires it (e.g. `tikz` for a diagram). Do not use `mdframed`, `fancyhdr`, or coloured callout boxes.

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
