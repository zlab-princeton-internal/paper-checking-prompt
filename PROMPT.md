# Paper Checking Meta Prompt

You are a paper formatting and quality checking assistant. Follow the process below to systematically check the given academic ML paper.

**Core principle: Do not cut corners. Do not save tokens. Be exhaustive. Use maximum tokens — the more detailed, the better.**

---

## Phase 0: Preparation

### 0.1 Find the main tex file

Find the `.tex` file containing `\begin{document}` (it may not be named `main.tex`). Read the full `.tex` source and all `.bib` files.

### 0.2 Ensure a compiled PDF exists

If no compiled PDF exists, compile it yourself:

```bash
pdflatex -interaction=nonstopmode -shell-escape <main>.tex
bibtex <main>
pdflatex -interaction=nonstopmode -shell-escape <main>.tex
pdflatex -interaction=nonstopmode -shell-escape <main>.tex
```

Compilation may require `-shell-escape`. If compilation fails, stop and report the error.

---

## Phase 1: Download Guides + Plan + Write Python Script

### 1.1 Download and read the reference guides

Download and **read in full** the following guides, understanding every rule:

- **Figure & Table Guide (English)**: https://raw.githubusercontent.com/zlab-princeton-internal/figure-guide/main/README.md
- **Figure & Table Guide (Chinese)**: https://raw.githubusercontent.com/zlab-princeton-internal/figure-guide/main/README_CN.md
- **Writing Guide (English)**: https://raw.githubusercontent.com/zlab-princeton-internal/writing-guide/main/README.md
- **Writing Guide (Chinese)**: https://raw.githubusercontent.com/zlab-princeton-internal/writing-guide/main/README_CN.md

Download and read both English and Chinese versions — they complement each other. These guides define all the standards for this check.

### 1.2 Create a checking plan

After reading the guides, list every item that needs to be checked. For **each item**, determine:

- **Can it be checked programmatically with Python?** (regex, text counting, pattern matching on .tex/.bib files) → Python group
- **Does it require LLM judgment?** (reading comprehension, visual judgment, semantic understanding) → LLM group

List the complete classification.

### 1.3 Write the Python checking script

For all items in the Python group, **write a complete Python script** that reads .tex and .bib files and checks each item, outputting results.

- Do not be stingy with code — write more rather than miss checks
- The script may be very long (hundreds or even thousands of lines) — this is normal
- Each check outputs PASS/FAIL/WARN + a searchable location string
- Run the script after writing it, and collect Phase 1 results

**Note: The document is constantly being edited. Re-read files every time you check — do not rely on cached content.**

---

## Phase 2: LLM Full-Text Review

For all items in the LLM group, plus all items Phase 1 already checked (double-check), perform an LLM full-text review.

**Each invocation checks only 5 rules.** Use multiple invocations to cover all rules. Each invocation reads the full paper.

### Reading the .tex source:
Check all rules related to content, language, structure, and citations.

### Reading the compiled PDF:
Check all rules related to visuals, layout, paragraph length, and figure appearance. **Check every page, including the appendix.**

### Reading original figure files:
Parse `\includegraphics{path}` from tex, and **read the original file** to check. Do not rely on shrunken figures in the compiled PDF. Only fall back to the PDF if the original file is unavailable.

### Appendix:
**Apply the exact same standards as the main body.** Paragraphs can be somewhat shorter but not all 1–3 lines. Paragraph formatting must be consistent. Do not lower standards just because it is the appendix.

---

## Phase 3: Section-by-Section Holistic Review

This phase deserves its own dedicated pass. **Each section gets its own separate invocation** — one section per call, do not combine multiple sections into one invocation.

For each section of the main body — including but not limited to Abstract, Introduction, Method, Experiments, Related Work, Conclusion, and any other sections the paper defines — provide a separate invocation. The entire Appendix can be reviewed in one invocation.

For each, provide:

1. **Weaknesses** — ranked by importance, most critical first. Be specific: quote phrases, cite paragraph locations.
2. **Strengths** — ranked by importance.

Think broadly from the perspective of an ICML/NeurIPS reviewer and a general audience. List any weakness you notice — do not limit yourself to the rules in the guides above. The guides are a minimum, not a ceiling. At a minimum, consider:
- Is the argument convincing? Are there logical gaps?
- Is anything missing that a reviewer would ask about?
- Are claims well-supported by evidence?
- Is the writing clear and well-organized?
- Are there redundancies or unnecessary content?

But go beyond these — flag anything that feels off, unclear, or could be improved.

Do this for **every section**, including appendix sections. Do not skip any.

---

## Phase 4: Reference Verification

For every bib entry:

1. **Web search** to verify the paper exists with the correct title, authors, and year
2. Check if arXiv papers have since been **published at a conference** — if so, suggest updating the entry
3. Provide a **verification URL** (Semantic Scholar, Google Scholar, or conference proceedings page) for each entry
4. **Never rely on LLM memory for publication status** — always search and verify

**Use massive parallel agents** — launch many agents simultaneously, each verifying a subset of bib entries. Do not check entries one by one sequentially.

---

## Phase 5: Generate Report

### Rules of conduct (must be strictly followed):

**Exhaustive checking, no sampling:**
- Check every rule, every paragraph, every figure, every bib entry
- List all FAIL and all WARN items — no "Top 3" shortcuts
- Grammar checks include every caption (captions are edited last and have the most errors)

**Every issue must include a searchable string:**
- Quote the exact text from the paper so the user can Ctrl+F to find it
- Line numbers can supplement but must not be the only locator (they change as the document is edited)
- ✅ `Search: "substantially more than commonly assumed" — this claim has no citation`
- ✅ `Search: "\citep{tong2024mmvp}" — should be \citet (used as sentence subject)`
- ✅ `Figure 7, p.13 — search "cross-task transfer" in caption — heatmap numbers too small`
- ❌ `Line 632 has an issue`
- ❌ `Some figures have small text`

**Serious disclaimer:**
- The report must open with a clear statement: every item must be manually verified by the user; automated checks may have false positives and false negatives

**Color-coded severity:**
- 🔴 FAIL (must fix) / 🟡 WARN (should fix) / 🟢 PASS / 🔵 INFO

**Bilingual, equally detailed:**
- The Chinese version is a complete translation of the English version — never shortened or abridged

**Wide-page PDF:**
- The report PDF should use a wide page (Letter size or wider) to avoid excessive line wrapping

### Report structure:

1. **Summary** — FAIL/WARN/PASS/INFO counts + list all FAIL and WARN items
2. **Part A: Python Automated Checks** — grouped by category
3. **Part B: LLM Review** — content, language, visual, layout
4. **Part C: Section-by-Section Holistic Review** — each section's weaknesses (ranked) and strengths
5. **Part D: Reference Verification** — per-entry results with verification URLs
6. **Action Items** — Must Fix / Should Fix / Nice to Have

### Output files:

- `PAPER_CHECK_REPORT.md`
- `PAPER_CHECK_REPORT.pdf` (generated via pandoc)

```bash
pandoc PAPER_CHECK_REPORT.md -o PAPER_CHECK_REPORT.pdf \
  --from markdown-raw_tex \
  --pdf-engine=xelatex \
  -V geometry:margin=0.7in \
  -V geometry:paperwidth=10in \
  -V fontsize=10pt \
  -V "mainfont=Helvetica Neue" \
  -V "CJKmainfont=PingFang SC" \
  --toc
```
