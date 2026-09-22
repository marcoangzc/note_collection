---
name: study-notes-builder
description: Turn a course's materials (lecture slides or notes, tutorial questions, past-year exam papers, textbook chapters) into story-style study notes that read like a narrative, fill the gaps the slides leave, verify code and calculations, flag slide errors, and end with exam-style practice questions and full answers. Use this whenever the user uploads course material and asks for notes, revision, exam prep, a chapter explanation, or says the lectures are hard to understand or "not explained well", even if they never say "notes". Also trigger for Chinese requests such as 做笔记, 整理笔记, 帮我复习, 讲义看不懂, 出练习题, 每一章做 note, pass year, 考古题.
---

# Study Notes Builder

> 中文说明：把课件（lecture / note、tutorial、past year）变成「像读故事一样」的笔记：每章从一个问题开始，概念一个接一个作为答案出现，读起来有前因后果。Claude 自己判断每种资料怎么用，不汇报过程；只有资料互相矛盾或需求不清楚时才来问你。
> ⚠️ 课件里的图片（pseudocode、方框图、例子表格、一步一步的例图、讲师的标注法）必须**逐页打开看**，不能只靠文字提取；交付前要按"slide inventory"逐页核对，确认没有漏掉。

## Why this skill exists

Slides are written to accompany a lecturer, not to teach on their own. A student reading them alone meets undefined terms, missing working, and vague or wrong statements, and tends to give up instead of asking. This skill produces notes that remove those obstacles, and it writes them as a story, because many students understand and remember ideas far better when each idea arrives as the answer to a problem the reader already cares about, instead of as a list of definitions.

The notes must still be exam-useful: accurate, complete for the syllabus, and aligned with how the course is actually examined.

## Workflow

Do steps 1 to 4 silently. Do not narrate progress, list the files you opened, or describe your plan to the user. The only reasons to talk to the user before delivering are the clarification cases in step 3.

### 1. Sort the materials

Look at every uploaded file and decide what kind it is, from its name and its content. Typical kinds:

| Kind | How to recognise it | What it is for |
|---|---|---|
| **Lecture slides / lecture notes** | "Lec", "Chapter", "Topic", "Week", "Note"; teaching content | **The syllabus and the main source.** Decides which concepts the notes cover and the terms and notation to use. |
| **Tutorial questions** | "Tutorial", "Tut", "Lab", "Exercise", "Practical"; questions, sometimes with answers | **The skills the course trains.** Every tutorial question type must be something the notes teach the student to do. |
| **Past-year papers** | "Past year", "PYP", "Final", "Exam", "Midterm", a year or semester in the name; exam format and marks | **How the course is examined.** Decides emphasis, which topics deserve depth, and the style of practice questions. |
| **Textbook / reference** | book chapters, long reference PDFs | Background and extra examples. Never overrides the lecture on scope or notation. |
| **Answer schemes / marking guides** | "Answer", "Solution", "Scheme" | Shows what markers expect. Verify before trusting; schemes contain mistakes too. |

Then map tutorials and past-year questions to chapters, so each chapter's note knows which questions belong to it.

Check the real file format before assuming a PDF is normal. Files named `.pdf` are sometimes zip archives of slide images plus text; try `file` and `unzip -l` when `pdftotext` fails. Use OCR or view the images for scanned material.

#### Text extraction is never enough: look at every slide that has a picture

Lecture slides put a large share of the syllabus in images: pseudocode pasted as a screenshot, schematic diagrams (agent architectures, flowcharts), tables (PEAS examples, property tables, complexity tables), step-by-step worked examples (search trees, game trees with [α, β] labels, domain tables), maps and their numbers, and the lecturer's own notation. `pdftotext` returns only a slide title or nothing for these pages, so the gap is **silent** — the extracted text looks complete, but it is not.

Do this for every lecture file, not only for exam papers:

1. **List the pages that contain images or vector drawings.** With PyMuPDF (`pip install pymupdf` if `pdftoppm` is missing):
   ```python
   import pymupdf
   d = pymupdf.open(path)
   pages = [i+1 for i, p in enumerate(d) if p.get_images() or len(p.get_drawings()) > 20]
   ```
   Also flag any page whose extracted text is under ~15 words: it is almost certainly a picture.
2. **Render those pages and actually view them.** To keep it fast, tile 4 pages into one contact sheet (2×2, about 72–90 dpi, page label on each tile) and view the sheets one by one. Re-render single pages at 110–150 dpi when numbers, chess boards, or small labels must be read exactly.
3. **Record what each picture contains** in the private per-chapter list (step 2): pseudocode, diagram, table, worked example, notation. Each one must either appear in the note or be consciously skipped as decoration (stock photos, movie posters, cartoons).
4. **Verify numbers taken from pictures** by recomputing them (e.g. an 8-queens h-value grid, an A* f = g + h table, a Sudoku deduction), exactly like any other worked example.

If a file's pictures cannot be rendered or viewed, say so to the user before writing — do not quietly write the chapter from text alone.

### 2. Read everything, then find the gaps and the exam focus

Read all the material for the requested scope before writing anything.

While reading, keep private lists per chapter:

- **Gaps**: jargon with no example, missing "why", truncated code, questions on the slides with no answer, unexplained figures, contradictions, wrong or oversimplified statements, demo output that looks like a bug, dependencies on earlier chapters.
- **Exam focus**: which topics appear in past-year papers, how often, for how many marks, and in what form (define, explain, compare, trace, calculate, write code, apply to a scenario). Note recurring question patterns.
- **Tutorial skills**: each distinct type of tutorial question.
- **Slide inventory**: for every slide page, one line — page number, topic, and what is on it (text only / pseudocode / diagram / table / worked example). This inventory is the checklist used in step 6 to prove nothing was dropped.
- **Lecturer's conventions**: the exact way the lecturer draws and annotates things (e.g. `[α, β]` beside every node, `f = g + h` under every node, expansion order numbers, table layouts for forward checking). Students are marked against these conventions, so the notes must reproduce them, not a textbook variant.

Treat tutorial "handbooks" filled in by students as unverified answers: use them to learn the question types, but recompute every answer.

A topic that is thin in the slides but heavy in past-year papers needs the most explanation. A topic that never appears in exams can be shorter, but still covered if it is in the lectures.

### 3. Clarify only when it matters

Ask the user before writing when one of these happens and the answer would change what you write:

- **Materials disagree**: the slide says one thing, the tutorial answer or past-year scheme says another, or two slides contradict each other and you cannot settle it by verification.
- **Scope is unclear**: past-year papers test a topic the lectures do not cover (maybe a missing lecture file, maybe a changed syllabus), or it is unclear which chapters the user wants.
- **A file cannot be read** or seems to be missing (for example Lecture 4 is absent between 3 and 5).
- **The request itself is ambiguous**: e.g. the output language cannot be told from the conversation, or the user's instruction conflicts with something the materials require.

How to ask: collect all open questions and ask them together in one short message, each with the options you see and which one you would pick by default. Do not ask about things you can settle by checking, running code, or reasonable judgement. For small uncertainties that do not change the structure, make the sensible choice, write the note, and mark the point inside the note (for example "⚠️ The tutorial answer gives 12, but recomputing gives 13; the working is below.").

### 4. Verify before writing

Never put an unchecked claim into a note the student will memorise.

- Programming: run the code and use real output. Test corrected versions of buggy slide or tutorial code.
- Maths, algorithms, finance, science: recompute every number in worked examples, tutorial solutions you include, and your own practice answers.
- Theory subjects: check definitions against the lecture material, and label anything added from outside the course as extra, so the student knows what is exam-safe.
- Past-year answers: work them out yourself; do not copy a scheme's answer without checking it.
- If something cannot be verified, say so where it appears. A visible uncertainty is better than a confident error.

### 5. Write the story

#### Output language

Decide this explicitly; do not drift into the language of the slides. In order: what the user asks for; a stored language preference; the language of the user's own messages. If still unclear, it is a step 3 question. For Chinese-speaking users, narrate in Chinese and keep technical terms, code and standard notation in English (a Malay-English mix is fine if the user uses it). Every explanation, table header, callout and answer is in the chosen language.

#### What "story style" means

The note should be something the student wants to keep reading. The engine of a story is a problem that needs solving, so each chapter is told as a chain of problems and solutions:

1. **Open with a situation, not a definition.** Put the reader in a concrete scene where the chapter's first problem appears: a shop with a million products and a slow search, a library, a queue at a bank, a group project where two people overwrite each other's work.
2. **Introduce each concept as the answer to the current problem.** The name comes after the reader already feels why it is needed. "This is exactly what a hash table solves" lands better than "A hash table is…".
3. **Let each solution create the next problem.** Chain the sections with "but then…" moments: the fast lookup causes collisions; collisions slow things down; that is why a load factor exists. This chain is the plot, and it mirrors the order of the lecture.
4. **Use one running example through the chapter.** The same shop, the same student record, the same small graph. Returning to familiar things makes new ideas cheaper to understand.
5. **Keep the characters small and the tone natural.** A simple recurring character or setting is welcome if it helps; do not force jokes, melodrama or childish tone. Sound like a good senior student explaining over coffee.
6. **Stop the story to show the working.** Traces, tables, code and diagrams stay exactly as precise as in a technical note. The narrative leads into them ("let's watch what the loop actually does") and picks up after them ("so the answer is 9, and notice why…"). Never replace working with narration.
7. **End each chapter by closing the loop.** Return to the opening problem and show how the chapter's ideas solved it, then hand off to the next chapter's problem.

Contrast, for the same idea:

> Dry: "Collision: two different keys hash to the same bucket. Java handles collisions using chaining."

> Story: "Our 16 lockers worked perfectly until 21 arrived. 21 % 16 is 5, and locker 5 already holds the number 5. Two different items, one locker: this is a **collision**. Java's answer is surprisingly simple: don't choose, just hang the second item behind the first, like a short chain of hooks. That's **chaining**. But imagine what happens when a chain gets ten items long…"

Both are accurate; the second makes the reader want the next sentence.

#### Chapter structure

```
# <Course code> — Chapter N: <Title>
(short blockquote: the problem this chapter solves, in one or two lines)

## Scene: <the opening situation>

## <Part 1: first problem → first concept>
   narrative, then worked example / trace / code, then "⚠️ easy to get wrong" if needed

## <Part 2: the problem Part 1 created → next concept>
   ...

## Closing the loop
   back to the opening scene; how the ideas fit together; hook to the next chapter

## ⚠️ Where the slides mislead (only if real issues were found)
   table: slide | what it says | more accurate understanding
   + exam advice: if a question quotes the slide, answer as the slide does

## Exam radar (only if past-year papers were provided)
   which parts of this chapter the papers test, how often, in what form,
   and one or two past-year style questions worked in full

## Cheat sheet
   compact table for last-minute revision (this part is not a story)

## Practice (answers included)
   A. MCQ   B. Short answer   C. Calculation / trace   D. Thinking
   Mirror tutorial and past-year question styles where they exist.
   Answers follow directly, each with a brief explanation.
```

Keep the cheat sheet, practice and answers in a plain, scannable format. Stories are for understanding; revision needs speed.

#### Other writing rules

- Keep the lecturer's terms and notation, and point out where they differ from common usage.
- Analogies must be accurate; say where an analogy stops working if that matters.
- Include every tutorial question type somewhere as a worked example, so the student has seen each skill done once before attempting the tutorial.
- Reproduce every **pseudocode** block from the slides (in a code fence, with short `←` comments explaining the key lines), every **schematic diagram** the exam could ask the student to draw (as an ASCII diagram plus a short "how it decides / how it works" paragraph), every **table** of examples, and every **step-by-step worked example** in the lecture, step by step. A note that explains a concept but omits the lecture's own diagram or example is incomplete.
- When the exam asks the student to draw something (landscape, tree, constraint graph, agent diagram), draw it in the same layout as the lecture slide and list the marking points. Generate long ASCII diagrams with a small script on a character grid so the lines stay aligned.
- Use ASCII diagrams in code fences; produce SVG only if asked or if ASCII cannot show the idea.
- No HTML tags like `<details>` in markdown.
- Length follows the gaps and the exam weight, not a target. Do not pad the story.

### 6. Review before delivering

Re-read each note as a student meeting the subject for the first time:

- Does every concept arrive after its problem, and does the chain of problems follow the lecture order?
- Is every term defined at first use, and every worked example fully worked?
- Are all tutorial skills and past-year question types covered?
- Do the practice answers match a recomputation?
- Are unverified claims labelled?
- **Coverage audit against the slide inventory**: walk through the inventory from step 2 slide by slide and tick where each item appears in the note. Every pseudocode, diagram, table and worked example must be ticked or explicitly marked as decoration. Any unticked item is added before delivering. Spot-check at least one image-heavy page per lecture file by viewing it again next to the note.

### 7. Deliver

- Save each chapter as its own `.md` in `/mnt/user-data/outputs/`, named `<COURSE>_Ch<N>_<Topic>.md`.
- Call `present_files` on the finished files. Only say a file is available after that call has happened.
- The closing message is short and only contains what the user needs to act on: which chapters are included, any issues they should know about (slide errors worth noticing, conflicts you resolved by assumption, figures you could not inspect), and one line offering a natural next step. Do not recap the process or list every file you read.
- Be exact about coverage. If any slide pictures were not viewed, the closing message must say which files/pages, instead of implying the notes are complete.

## Adapting to the subject

- **Programming / algorithms**: running example can be a small program or dataset that grows through the chapter; run every snippet.
- **Maths / statistics / physics**: the problem-first chain works well ("we can measure this, but not that; how do we get it?"); full worked solutions, units and edge cases checked.
- **Accounting / finance / economics**: follow one small business through the chapter; show every entry or formula line by line; recompute totals.
- **Law / business / humanities / biology**: tell it through a case or scenario; practice leans on application ("what happens if…") and compare-and-contrast.
- **Languages**: a situation where the pattern is needed, correct and incorrect examples, practice that asks the student to produce sentences.

## Optional requests

- **Exam sprint**: shorter story, bigger Exam radar and practice sections.
- **Pure reference**: drop the story and use a compact technical format.
- **Bilingual**: main language narration with English key terms in brackets, or a second file in the other language.
- **Interactive**: walk through one hard chapter conversationally instead of as a document.
- **Quiz / flashcards** from a finished note.

## Pitfalls

- Reporting progress or listing files instead of just doing the work.
- Asking questions that verification or judgement could settle; or, the opposite, silently guessing when materials genuinely conflict.
- Letting the story replace the working, or making the tone childish.
- Writing notes in English only because the slides are English.
- Copying slide text or past-year answers verbatim instead of explaining.
- Calling a slide "wrong" without verifying; many statements are simplifications, so label them as such.
- Claiming a file, output or verification that did not actually happen.
- **Writing notes from `pdftotext` output alone.** Slide pseudocode, agent diagrams, example tables and step-by-step figures are usually images and vanish silently from extracted text. Only the exam papers getting viewed as images, while the lecture slides do not, is the classic form of this mistake.
- Replacing the lecture's own examples with invented ones (e.g. a new map instead of the lecture's Romania map), or the lecturer's notation with a textbook variant. Add new examples only on top of the lecture's.
