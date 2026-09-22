---
name: "study-notes-builder"
description: Turn a course's materials (lecture slides or notes, tutorial questions, past-year exam papers, textbook chapters) into study notes that read like a story, fill the gaps the slides leave, verify code and calculations, flag slide errors, give model answers for every past-year question, and end with exam-style practice questions and full answers. Use this whenever the user uploads course material and asks for notes, revision, exam prep, a chapter explanation, or says the lectures are hard to understand or "not explained well", even if they never say "notes". Also trigger for Chinese requests such as 做笔记, 整理笔记, 帮我复习, 讲义看不懂, 出练习题, 每一章做 note, pass year, 考古题.
---

# Study Notes Builder

> 中文说明：把课件（lecture / note、tutorial、past year）变成「像读故事一样、又能直接拿分」的笔记：每章从一个问题开始，概念作为答案出现；先完整读完材料（**past year 最先读**），**课件里的图片逐页打开看**，找出 slide 没讲清楚或讲错的地方补上；代码和数字都实际验证；每一题 past year 都有满分答法；最后附练习题和完整答案。Claude 自己判断每种资料怎么用，不汇报过程；只有资料互相矛盾或需求不清楚时才来问你。
> ⚠️ 课件里的图片（pseudocode、方框图、例子表格、一步一步的例图、讲师的标注法）必须**逐页打开看**，不能只靠文字提取；交付前要按 "slide inventory" 逐页核对，确认没有漏掉。

## Why this skill exists

Lecture slides are written to accompany a lecturer, not to teach on their own. Students who read them alone hit three walls: terms that are never explained, examples with the working missing, and statements that are vague or simply wrong. When a student hits a wall they tend to give up rather than ask. Good notes remove those walls **before** the student reaches them. The goal is never to restate the slides shorter; it is to explain what the slides skip.

The notes are written as a story, because students understand and remember ideas far better when each idea arrives as the answer to a problem they already care about. They must still be exam-useful: accurate, complete for the syllabus, and aligned with how the course is actually examined.

## Workflow

Follow these steps in order. Do not start writing notes until steps 1–4 are done. Do steps 1–4 silently: do not narrate progress, list the files you opened, or describe your plan. The only reasons to talk to the user before delivering are the clarification cases in step 3.

### 1. Scope the job and sort the materials

Work out from the request and conversation: which files or chapters, the output language, and any format preference already stated. If the user has a stored preference about language, teaching style or answer delivery, follow it silently.

Look at every file and decide what kind it is, from its name and content:

| Kind | How to recognise it | What it is for |
|---|---|---|
| **Past-year papers** | "Past year", "PYP", "QP", "Final", "Exam", a year/semester in the name; marks shown | **How the course is examined. Read it first** (step 2). |
| **Lecture slides / notes** | "Lec", "Chapter", "Topic", "Week"; teaching content | **The syllabus and the main source**: which concepts, terms and notation. |
| **Tutorial questions** | "Tutorial", "Tut", "Lab", "Exercise" | **The skills the course trains**; every question type must be taught in the notes. |
| **Textbook / reference** | book chapters, long reference PDFs | Background and extra examples; never overrides the lecture on scope or notation. |
| **Answer schemes / student handbooks** | "Answer", "Solution", "Scheme", a tutorial book filled in by students | Shows what markers expect; **verify before trusting** — these contain mistakes. |

Then map tutorial and past-year questions to chapters, so each chapter's note knows which questions belong to it.

Defaults when nothing is specified:
- One markdown file per chapter (lecture parts such as 2.1/2.2/2.3 may be merged into one chapter file), plus a `00_Exam_Guide` file when past papers exist.
- Write in the language the user writes in. For Chinese-speaking users, explain in Chinese and keep all technical terms in English (Manglish/Malay-English mix is fine if the user uses it).
- Give answers directly, not "try it first" — unless the user asks for exercises without answers.

#### Bilingual: explain in the thinking language, answer in the exam language

When the student's language differs from the exam's language, **don't translate everything** — that doubles length for little gain. Split by job:

| Job | Language |
|---|---|
| Understanding the concept | the language the student thinks in |
| Technical terms | **always the exam language** — never gloss "Content Provider" into Chinese, or the student won't recognise it on the paper |
| The sentence they'd actually write in the exam | **the exam language** |

So: body prose in the student's language, terms untranslated, and after each **major, examinable** concept (not every minor point) add one line:

> **💬 答题句 (EN)** — the one or two sentences the student would actually write on the exam paper. This is the scoring formulation, compressed — *not* a translation of the paragraph above it.

Also give each chapter a bilingual term table (`English | 中文 | 一句话说明`), and write practice questions **and their answers** in the exam's language, with a one-line note in the student's language under answers where it helps. If the user explicitly wants full parallel text, do it, but say in one line that it roughly doubles reading time.

### 2. Read everything first — past paper first, pictures included

Read all the material for the requested chapters before writing anything.

**Read the past exam paper first.** It tells you the question style, mark allocation, command verbs ("examine", "justify", "differentiate", "illustrate"), and whether the paper is built around a case study. This reshapes the notes: which topics deserve length, how practice questions are phrased, and which concepts need a ready-made model answer. Map every question to the chapter that answers it and carry that mapping into each chapter's overview.

Check the real file format before assuming it is a normal PDF. Files named `.pdf` are sometimes zip archives of slide images plus text; try `file` and `unzip -l` when `pdftotext` fails.

| Material | How to read |
|---|---|
| Text-based PDF | `pdftotext -layout`, or PyMuPDF `page.get_text()` per page |
| Zip of slide images + txt | unzip, concatenate the txt files in numeric order |
| Images / scanned PDF (past papers are often scanned) | render pages and view them; OCR (`tesseract`) if available |
| PPTX / DOCX | use the pptx / docx skills to extract text |

#### Text extraction is never enough: look at every slide that has a picture

Slides put a large share of the syllabus in images: pseudocode pasted as a screenshot, schematic diagrams (architectures, lifecycles, flowcharts), tables (examples, comparisons, complexity), step-by-step worked examples (search trees, game trees with `[α, β]` labels, domain tables), maps with their numbers, and the lecturer's own notation. Text extraction returns only a title or nothing for these pages, so the gap is **silent** — the extracted text looks complete but is not.

Do this for **every lecture file**, not only for exam papers:

1. **List candidate pages mechanically.** With PyMuPDF (`pip install pymupdf` if `pdftoppm` is missing):
   ```python
   import pymupdf
   d = pymupdf.open(path)
   pages = [i+1 for i, p in enumerate(d) if p.get_images() or len(p.get_drawings()) > 20]
   low_text = [i+1 for i, p in enumerate(d) if len(p.get_text().split()) < 15]
   ```
   Also flag: slides whose text reads like garbled word soup (a boxes-and-arrows diagram flattened into repeated nouns — it looks like content and is not), and **slides whose text is just labels such as "Activity A / Intent / Android System / Start"** — these are vector-drawn diagrams that the image test misses.
2. **Render and actually view them.** Tile 4 pages into one contact sheet (2×2, ~72–90 dpi, page label on each tile) and view the sheets one by one. Re-render single pages at 110–150 dpi when numbers, boards or small labels must be read exactly.
3. **Record what each picture contains** in the slide inventory (step 3): pseudocode, diagram, table, worked example, notation, or decoration (stock photo, meme, QR code).
4. **Verify numbers taken from pictures** by recomputing them, exactly like any other worked example.

If a figure genuinely cannot be inspected, say so in the note at that point and again in the final message — never guess it.

### 3. Find the gaps, build the inventory, clarify only when it matters

While reading, keep private lists per chapter:

- **Slide inventory** — one line per slide: page number, topic, and what is on it (text only / pseudocode / diagram / table / worked example / decoration). This is the checklist for step 6.
- **Lecturer's conventions** — the exact way the lecturer draws and annotates things (e.g. `[α, β]` beside every node, `f = g + h` under every node, expansion-order numbers, table layouts). Students are marked against these, so the notes reproduce them, not a textbook variant.
- **Gaps** — anything a self-studying student would trip on:
  - unexplained jargon; missing "why"; truncated or partial code;
  - **questions posed on the slide with no answer** (usually tutorial/exam-like — answer every one);
  - figures with no explanation; internal contradictions (slide 12 vs slide 30);
  - wrong or oversimplified statements (verify before calling them wrong); absolute claims with real exceptions;
  - demo output that looks like a bug but is not; concepts that quietly depend on an earlier chapter.
- **Exam focus** — which topics the papers test, how often, for how many marks, in what form.
- **Tutorial skills** — each distinct type of tutorial question.

A topic thin in the slides but heavy in past papers needs the most explanation; sections the slides already explain well can be short.

**Ask the user before writing only when** the answer would change what you write:
- materials disagree and verification cannot settle it;
- scope is unclear (papers test a topic the lectures don't cover, or which chapters is unclear);
- a file cannot be read or seems missing (Lecture 4 absent between 3 and 5);
- the request itself is ambiguous (e.g. output language).

Collect all open questions into one short message, each with the options and your default. For small uncertainties, make the sensible choice and mark it inside the note (e.g. "⚠️ The tutorial answer gives 12, but recomputing gives 13; working below.").

### 4. Verify before you write

Never put an unchecked claim into a note the student will memorise.

- **Programming**: run the code and paste real output; test corrected versions of buggy slide code. If no compiler/runtime is available, say so and label the snippets as unexecuted.
- **Maths, algorithms, finance, science**: recompute every number in worked examples, tutorial solutions you include, and your own practice answers — including numbers read from slide images.
- **Theory subjects**: cross-check definitions against the source; label anything added from outside as "extra / not in the slides".
- **Past-year answers**: work them out yourself; never copy a scheme's or a student handbook's answer without checking. If a question itself is flawed (e.g. an unsolvable puzzle), find what can still be answered and flag the flaw.
- If something cannot be verified, say so plainly. A visible uncertainty beats a confident error.

### 5. Write the notes as a story

#### Story style

1. **Open each chapter with a situation, not a definition** — a concrete scene where the chapter's first problem appears (a timetable that won't fit, a bus that needs the shortest route, a user who loses a half-typed message when a call comes in). Where a past paper has a case study, the case app can be the running scene.
2. **Introduce each concept as the answer to the current problem**; the name comes after the reader feels why it is needed.
3. **Let each solution create the next problem** ("but then…") — this chain is the plot and follows the lecture order.
4. **Use one running example through the chapter.**
5. **Tone**: a good senior student explaining over coffee; no forced jokes or childish tone.
6. **Stop the story to show the working** — traces, tables, code and diagrams stay fully precise; never replace working with narration.
7. **Close the loop** — return to the opening scene and hand off to the next chapter.

Keep the cheat sheet, practice and answers plain and scannable: stories are for understanding, revision needs speed.

#### Chapter structure

```
# <Course code> — Chapter N: <Title>
(one-line blockquote: what the slides get wrong or skip, and what this note adds)

## 0. One-sentence overview
   + a small map/table of the chapter's topics with slide numbers
   + 🎯 which exam questions this chapter answers (if a past paper was read)
   + prerequisites: one line of refresher each

## Scene (short opening situation)

## 1..k. Concept sections (one per topic, in slide order)
   - plain-language explanation, arriving as the answer to a problem
   - accurate everyday analogy when the concept is abstract
   - worked example with a step-by-step trace table
   - the lecture's own pseudocode, diagram, table or example, reproduced
   - "💬 答题句 (EN)" for major examinable concepts (if bilingual)
   - "✅ 满分答法 / model answer" for each past-paper question this section answers
   - "⚠️ common confusion" where the gap list says students trip
   - answers to the slide's own "Question?" slides

## Closing the loop

## ⚠️ Where the slides mislead (only real issues)
   table: slide | what it says | more accurate understanding
   + one line of exam strategy: if a question quotes the slide, answer as the slide does

## Term table (if bilingual)   English | 中文 | one-line gloss
## Cheat sheet
## Practice (answers included)
   A. MCQ (5)  B. Short answer (3)  C. Application / trace (3)  D. Thinking (2)
   In the exam's language, imitating the past paper's format (case-study style if the paper uses one).
## Slide index   (note section -> slide numbers)
## Links to other chapters
```

#### Model answers for past papers

Give every past-paper question a **✅ 满分答法 / model answer** block inside the chapter that covers it, written at the mark allocation (a 6-mark "TWO …" question gets two developed points of ~3 marks each; a 15-mark "THREE methods" gets three ~5-mark points: name + principle + how/code + benefit). When the paper is built on a case study, **apply every answer to the case app** — definitions alone rarely earn "apply/justify" marks. Draw a table when the question says "in a table format", and a labelled diagram when it says "using a diagram". By the end, every question on every paper must be answerable from the notes.

#### Other writing rules

- **Explain why, not only what**; **trace, don't assert**; one idea per section in lecture order.
- Keep the lecturer's terms and notation, and note where they differ from common usage.
- Reproduce every **pseudocode** block from the slides (code fence, short `←` comments on key lines), every **diagram the exam could ask the student to draw** (ASCII + a short "how it works" paragraph), every **table** of examples and every **step-by-step worked example**. A note that explains a concept but omits the lecture's own diagram or example is incomplete.
- When the exam asks the student to draw something, draw it in the **same layout as the lecture slide** and list the marking points. Generate long ASCII diagrams with a small script on a character grid so lines stay aligned.
- Include every tutorial question type somewhere as a worked example.
- Analogies must be accurate; say where they stop working if that matters.
- Base some practice questions on the slide's own examples.
- **No HTML tags** such as `<details>` in markdown files.
- Length follows the gap list and exam weight, not a target. Do not pad.

#### Visuals: draw only where structure earns it, but draw it properly

Draw a diagram when a concept has states and transitions, layers or containment, ordered flow, two or more comparison dimensions, or a slide figure whose content text extraction lost. Don't draw for definitions or flat lists.

- **Markdown (default)**: ASCII art in code fences, under ~70–90 characters wide.
- **Published HTML artifact**: hand-drawn inline SVG using theme CSS variables and a `viewBox` with no fixed width. Worth it when a chapter has 3+ genuinely structural concepts, or the user asks for a link.
- Never deliver through an inline chat visualiser — the note must be a file or link the student keeps.

Label every diagram with a one-line caption saying **what to notice in it**.

### 6. Review before delivering

Re-read each note as a student who has never seen the course:

- Does every concept arrive after its problem, following the lecture order?
- Is every term defined at first use, and every worked example fully worked?
- Do the practice answers match a recomputation?
- Did you flag every slide problem you found, and only real ones? Are unverified claims labelled?
- **Past-paper coverage**: list every sub-question of every paper and point to its model answer; add any that are missing.
- **Coverage audit against the slide inventory**: walk the inventory slide by slide and tick where each pseudocode, diagram, table and worked example appears in the note (or mark it as decoration). Add any unticked item. Spot-check at least one image-heavy page per lecture file by viewing it again next to the note.

**If the output is HTML, run the checker before publishing — every time:**

```bash
python3 scripts/check_notes_html.py <file>.html
```

It catches failures that are silent in HTML: malformed `</strong` missing `>`, unbalanced tags, duplicate ids, dangling `href="#…"` / `url(#…)` references, empty `<svg>` placeholders. Fix everything and re-run until clean.

### 7. Deliver honestly

Pick the format:

1. **Markdown `.md` files (default)** — one per chapter, named `<COURSE>_Ch<N>_<Topic>.md`, plus `<COURSE>_00_Exam_Guide.md` (paper format, question→chapter map, marks per chapter, one-day study plan, answering tips, slide errors, limitations). Save in a `Notes_<COURSE>` folder next to the materials unless the user names a location, then send/present the files.
2. **Published HTML artifact** — when the Visuals rule calls for real diagrams, or the user asks for a link.
3. **One master artifact for all chapters** — read `references/multi-chapter-artifact.md` first (id namespacing, incremental append, sticky nav).
4. **PDF / DOCX** — only for printing, submitting or offline reading.

Only say a file or link exists **after** it has actually been sent or published.

The closing message is short: what each note covers, the most important slide errors, what was verified (and what could not be — e.g. no compiler), any figures not inspected (name the files/pages; never imply full coverage otherwise), and one line offering a natural next step.

## Adapting to different subjects

- **Programming / algorithms** — run every snippet; include complexity; correct buggy slide code and explain the bug.
- **Maths / statistics / physics** — full worked solutions line by line; check units and edge cases; recompute all numbers.
- **Accounting / finance / economics** — one running company/dataset; show entries or formulas line by line; recompute totals.
- **Law / business / humanities / biology** — cross-check against the source; cases and scenarios as worked examples; practice leans on application and compare-and-contrast.
- **Case-study exams** (common in app-development, IS and management courses) — every model answer names the case app and applies the concept to its features.
- **Language courses** — patterns with correct/incorrect examples; practice that requires producing sentences.

## Optional adjustments

- **Exam sprint**: shorter story, bigger model-answer and practice sections, likely-questions list.
- **Pure reference**: drop the story, compact technical format.
- **One combined cheat sheet** organised by exam question type.
- **Flashcards / quiz** from a finished note; **interactive** walk-through of one hard chapter.
- **Different language or level**.

## Common pitfalls to avoid

- Starting to write after reading only one chapter; **skipping the past exam paper**.
- **Writing notes from text extraction alone.** Slide pseudocode, diagrams, example tables and step-by-step figures are usually images (or vector drawings) and vanish silently from extracted text. Viewing only the exam papers as images while not viewing the lecture slides is the classic form of this mistake.
- Replacing the lecture's own examples with invented ones, or the lecturer's notation with a textbook variant. Add new examples only on top of the lecture's.
- Repeating the slides in fewer words; copying long passages verbatim.
- Letting the story replace the working, or a childish tone.
- Translating technical terms for a bilingual student.
- Trusting student-filled tutorial answers or answer schemes without recomputing.
- Declaring a slide "wrong" without checking — many statements are simplifications; label them as such.
- Asking questions that verification could settle — or silently guessing when materials genuinely conflict.
- Publishing HTML without running the checker.
- Claiming a file, output, verification or full coverage that did not actually happen.
