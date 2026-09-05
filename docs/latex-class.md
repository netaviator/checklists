# `checklist.cls` — Documentation

The **`checklist`** class is a LaTeX document class designed to create structured, customizable checklists for aviation or other technical domains.
It provides a clean layout, checklist environments, and metadata handling for official-looking documents.

---

## 📦 Usage

1. Save the class file as `checklist.cls` in the working directory or your local TeX tree.
2. Use it in your document preamble:

```latex
\documentclass[a4double,legal_disclaimer]{checklist}
```

---

## ⚙️ Class Options

The class provides several **options**:

| Option                                              | Default    | Description                                                                |
|------------------------------------------------------|------------|-----------------------------------------------------------------------------|
| `a4double` / `a5double` / `a6double` / `a6single`     | `a4double` | Defines paper size & layout. Pass one of these directly as a class option (not `papersize=...`). |
| `legal_disclaimer`                                    | `false`    | If set, adds a disclaimer box on the title page promting to read the POH.  |
| Any other option                                      | –          | Passed directly to the `article` base class.                                |

### Paper size mapping

* `a4double` → A4, two-column
* `a5double` → A5, two-column
* `a6double` → A6, two-column
* `a6single` → A6, single-column

---

## 📝 Metadata Commands

These macros define document-level metadata for title page and headers/footers:

```latex
\checklistName{Cessna 172 Checklist}
\checklistDescription{How to fly a brick of aerospace grade aluminum?}
\checklistRevision{1.2}
\checklistDateIssue{2025-09-12}
\checklistLogo{logo.pdf} % optional
```

* **Name** → Main title of the checklist
* **Description** → Subtitle
* **Revision** → Revision number displayed in footer
* **Date Issue** → Publication date displayed in footer
* **Logo** → Optional graphic included on title page

---

## 📄 Title Page

Use `\maketitle` to create the title page.
If `legal_disclaimer` is enabled, a bilingual disclaimer is added:

> *This document is for reference only and does not replace the official Pilot Operating Handbook.*

---

## 🧾 Checklist Environment

The core feature is the `checklist` environment:

```latex
\begin{checklist}{ENGINE START}
  \checkitem{Brakes}{SET}
  \checkitem{Mixture}{RICH}
  \notes{Ensure clear prop area}
  \decision{If no start: Troubleshoot}
\end{checklist}
```

### Syntax

```latex
\begin{checklist}{<Title>}[<tcolorbox options>]
  ... checklist content ...
\end{checklist}
```

* `<Title>` → Displayed in uppercase in box title
* `[<tcolorbox options>]` → Customize colors and layout

#### Example (custom color):

```latex
\begin{checklist}{Starlight Launch}[colback=green!5,colframe=black]
  \checkitem{Main Switch}{ON}
  \checkitem{Fuel Pump}{ON}
\end{checklist}
```

---

## ✅ Checklist Commands

Available macros inside `checklist`:

| Command             | Example                                                    | Description                                    |
| ------------------- | ---------------------------------------------------------- | ---------------------------------------------- |
| `\checkitem{X}{Y}`  | `\checkitem{Brakes}{SET}`                                  | Creates a checklist entry with dots and result |
| `\notes{Text}`      | `\notes{Verify visually}`                                  | Adds small indented note                       |
| `\decision{X}`      | `\decision{ABORT}`                                         | Bold decision step                             |
| `\step{X}`          | `\step{Cross-check instruments}`                           | Indented sub-step with vertical bar            |
| `\lowersection{X}`  | `\lowersection{After Takeoff}`                             | Creates a lower section inside same checklist  |
| `\detailitem{X}{Y}` | `\detailitem{Engine Fire}{Follow emergency procedures...}` | Boxed detailed item                            |

---

## ⚠️ Decision Box

`\decisionbox{X}` draws a standalone bold callout box (yellow background, black frame) for a
warning, caution, or decision point. Unlike `\decision`, it is not tied to a `checklist`
environment and can be used anywhere in the document body.

```latex
\decisionbox{If engine fire persists: LAND IMMEDIATELY}
```

---

## 📚 Chapters

Checklists are organized into **chapters**:

```latex
\normalchapter
\section{Before Start}
...

\abnormalchapter
\section{Engine Roughness}
...

\emergencychapter
\section{Engine Fire in Flight}
...
```

### Available chapter macros

* `\normalchapter` → Normal procedures
* `\abnormalchapter` → Abnormal procedures
* `\emergencychapter` → Emergency procedures

Each resets the section counter and prints a dedicated chapter page.

---

## 🔢 Sectioning

Sections are defined with `\section{<Title>}`.

* Each section starts on a new page.
* Headers and footers update dynamically with chapter type and section title.

Example:

```latex
\section{Taxi}
\begin{checklist}{Taxi Procedure}
  \checkitem{Brakes}{Check}
  \checkitem{Steering}{Free}
\end{checklist}
```

---

## 📑 Headers & Footers

* **Header Left** → Checklist name + current chapter type
* **Header Right** → Section number + section title
* **Footer Left** → Revision number (hidden if `\checklistRevision` was never set)
* **Footer Center** → Page X of Y
* **Footer Right** → Issue date

---

## 🎨 Styling

* Defaults: `colback=white`, `colframe=black`, square edges
* Fully customizable via `tcolorbox` options passed in `checklist`.
* Pass `noborder` to remove the box border entirely (keeps the fill color and title).

Example:

```latex
\begin{checklist}{Power Check}[colback=yellow!10,colframe=red!70!black]
  \checkitem{Throttle}{2000 RPM}
  \checkitem{Magnetos}{CHECK}
\end{checklist}

\begin{checklist}{Borderless Notes}[noborder]
  \checkitem{Flaps}{UP}
\end{checklist}
```

---

## 📖 Example Document

```latex
\documentclass[a4double,legal_disclaimer]{checklist}

\checklistName{Diamond DA20-A1 Checklist}
\checklistDescription{Normal, Abnormal, Emergency}
\checklistRevision{1.0}
\checklistDateIssue{2025-09-12}
\checklistLogo{logo.pdf}

\begin{document}
\maketitle

\normalchapter
\section{Preflight}
\begin{checklist}{Exterior Inspection}[colback=blue!5,colframe=black]
  \checkitem{Aircraft Papers}{CHECKED}
  \checkitem{Fuel}{CHECK}
  \notes{Drain fuel samples from both tanks}
\end{checklist}

\emergencychapter
\section{Engine Fire}
\begin{checklist}{Engine Fire in Flight}[colback=red!10,colframe=black]
  \decision{LAND IMMEDIATELY}
  \step{Fuel Shutoff Valve: OFF}
  \step{Master Switch: OFF}
\end{checklist}

\end{document}
```
