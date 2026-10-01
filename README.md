# Examplify clone — CS1010E practice

A self-contained clone of the Examplify exam interface for practising CS1010E
mid-term questions. It reproduces the window chrome, dark header with the
countdown, FILTER rail, FLAG QUESTION pill, rounded answer rows, the
`Answers: A - E` / `Answers 1 - 3` blocks, and the Previous / Next / Finish
footer from the reference screenshots.

## Open it

Double-click **`examplify.html`**. That is the whole app — one file, no server,
no install, no network access.

If you would rather edit the source and reload, open `index.html` instead; it
loads `styles.css`, `app.js` and the `data/` files separately.

## Papers

Every source PDF has been split into its own exam. Nothing is combined.

Each paper is named exactly as it is titled inside its PDF, so a card always
matches the document it came from.

| Exam in the picker | Where it comes from |
|---|---|
| `MID-TERM PRACTICE EXAMINATION (AY 2026/2027)` | `CS1010E Mock Exam 1 (Google AI Studio).pdf` — 28 questions, 57 marks |
| `Paper A` | `CS1010E Mock Exam 2,3,4,5 (Claude Opus 5).pdf`, from the pack titled *CS1010E Programming Methodology — Midterm Practice Papers* |
| `Paper B` | same pack |
| `Paper C` | same pack |
| `Paper D` | same pack |
| `MID-TERM EXAM (Semester 1 : AY 2025/26)` | `cs1010e_2526S1_midterm.pdf` — the real past paper |
| `MID-TERM TEST (AY 2020/2021, SEM 1)` | `cs1010e_2021S1_midterm_soln.pdf` — 25 questions, all multiple choice |
| `MID-TERM TEST (AY 2021/2022, SEM 1)` | `cs1010e_2122S1_midterm_soln.pdf` — 25 questions (2 fill-in-the-blank) |
| `MID-TERM TEST (AY 2022/2023, SEM 1)` | `cs1010e_2223S1_midterm_soln.pdf` — 25 questions (2 fill-in-the-blank) |

258 questions in total. Each of the four pack papers has 31 questions worth
61 marks: 22 multiple choice (Sections A and B) and 9 fill-in-the-blank
(Section C).

The PDFs themselves are **not committed** — they are matched by `*.pdf` in
`.gitignore` and stay on the machine that owns them. Everything the app needs
is already transcribed into `data/`, so a fresh clone runs fine without them.

## What the clone does

- **Two question types.** Multiple choice rows show the letter, the option
  text and a mark on the right; fill-in-the-blank questions render `{{n}}`
  markers in the code as circled numbers (the `( ① )` in the screenshots) and
  give you one numbered input per blank, exactly like the paper.
- **Eliminating options.** The mark on the right of a choice strikes it
  through so you can rule answers out; clicking it again undoes the mark. It
  never reveals the key. Eliminated options stay selectable, and your marks
  are kept per question, surviving a reload.
- **The `Answers:` header tracks your choice.** It reads `Answers: A - E`
  until you pick one, then names the letter you selected —
  `Answers: A - E   —   selected: C` — so the choice stays visible even if
  you have scrolled or struck the option out. Fill-in questions keep the plain
  `Answers 1 - 2` header, and their rows carry no elimination mark because
  there is nothing to rule out.
- **Grading.** MCQs are right or wrong. Fill-in questions follow the paper's
  own rule: **every** blank must be correct, there is no partial credit. Text
  answers ignore whitespace and quote style, so `acc + (x,)` and `acc+(x,)`
  both count.
- **Results screen.** Score ring, marks earned, correct / incorrect / skipped
  counts, then a card per question with your answer, the correct answer and a
  short explanation. "Open this question" jumps back into the interface with
  the key overlaid in green and red.
- **Nothing locks you out.** Submitting only shows the key — **Continue this
  paper** puts you straight back into the editable paper with every answer
  intact. **Retake paper** is the only thing that clears answers, and it is
  never automatic.
- **Timer.** Counts down from 90 minutes in `MM:SS` and turns red under five
  minutes. It is a prompt, not a gate: at zero the header switches to
  `TIME OVER +MM:SS` in amber and warns you once, while the paper stays fully
  editable so you can work at your own pace.
- **A stopwatch per question.** Each question shows its own clock in the
  question header and only ticks while that question is the one on screen —
  stepping to another question, submitting, reviewing the key, or hiding the
  tab pauses it. The seconds are stored with your answers (`session.times`), so
  a paper remembers how long every question took across reloads.
- **Time chart on the results screen.** Under the score summary, *Time per
  question* shows total time, the single longest question, the average time per
  multiple-choice and per fill-in question, and a verdict on which *kind* of
  question eats the most time — plus a bar per question, coloured by type, with
  the slowest bar highlighted. Filters narrow it to MCQ or fill-in, and sort it
  by question order or longest first.
- **Compare across papers.** *Time analysis — all papers* (from the picker, or
  *Compare all papers* on any results screen) pools every paper you have sat
  into the same chart. Toggle each paper on or off, filter by question type,
  and sort by longest first or paper order.
- **Export / import attempts.** Because attempts live in this browser only, the
  picker has an **Export attempts** button that downloads every saved paper as a
  single JSON file (`examplify-attempts-YYYY-MM-DD.json`, tagged with a backup
  version), and an **Import attempts** button that reads one back through a file
  picker — nothing has to be typed or pasted anywhere. Import warns first if the
  file is not a backup or has no papers this build knows about, then offers
  **Keep newest** (only overwrite when the backup is more recent) or **Replace
  all**. This is the way to move attempts to another browser, profile or origin
  (`file://` and a local server keep separate storage), or to back them up before
  clearing site data.
- **Answers are saved** to this browser's `localStorage` after every keystroke,
  so you can close the tab mid-paper and resume later, or reload by accident
  without losing anything. The picker shows "In progress — n of 31 answered" or
  your score for a finished paper (times included). Export the lot from the picker
  if you want a copy that git and a browser wipe cannot take away.
- **Mid-term scope labels.** The three past papers (`2021S1`, `2122S1`,
  `2223S1`) are tagged against the lectures 1–5 scope sheet. A question that
  touches anything off-syllabus (lists, dicts, sets, files, `turtle`, `math`,
  …) carries a small pill at the top right of its header — `OUT OF SCOPE`,
  the level, and a one-line reason — and the same pill on its results card.
  In-scope questions are left clean. Hovering the pill shows the full reason.
- **Filter**, **flag**, **jump-to-question**, and keyboard shortcuts
  (`←` / `→` to move, `F` to flag, `Esc` to close menus). The FILTER rail has a
  **MID-TERM SCOPE** group that narrows the paper to *in scope*, *out of scope*,
  or any single level: **totally** (the question is only about the off-syllabus
  item), **crucially** (you must understand that item to answer), or **slightly**
  (the item is incidental — the tested idea is in the lectures).

## Layout

```
examplify.html          built single-file app — open this
index.html              same app, multi-file version
styles.css
app.js
build.js                inlines index.html + css + js -> examplify.html
data/registry.js        registerExam() helper
data/exam-mock1.js      one file per paper
data/exam-paper-a.js
data/exam-paper-b.js
data/exam-paper-c.js
data/exam-paper-d.js
data/exam-midterm.js
tools/extract-pdfs.py   re-extract the PDFs to text (needs pymupdf)
tools/validate-data.js  sanity-check the question data
```

## Rebuilding and checking

After editing `index.html`, `styles.css`, `app.js` or anything in `data/`:

```bash
node build.js              # rebuild examplify.html
node tools/validate-data.js   # verify every paper's data
```

`validate-data.js` checks that question numbers run 1..n without gaps, that
every MCQ's key is one of its own options, that the `{{n}}` markers in each
stem line up one-for-one with the declared blanks, and that the section ranges
cover every question.

To regenerate the extracted text from the PDFs:

```bash
pip install pymupdf
python tools/extract-pdfs.py    # writes tools/text/*.txt
```

## Adding a paper

Create `data/exam-mine.js`, then add a `<script src="data/exam-mine.js"></script>`
line to `index.html` before `app.js`, and rebuild. The shape is:

```js
registerExam({
  id: 'mine',
  title: 'MID-TERM PRACTICE EXAMINATION (AY 2027/2028)',  // name the card
  headerName: 'MID-TERM PRACTICE EXAMINATION (AY 2027/2028)',  // shown in the header
  source: 'my-paper.pdf',
  duration: 90,                                 // minutes
  sections: [{ name: 'Section A', from: 1, to: 12, marks: 1 }],
  questions: [
    {
      n: 1, marks: 1, type: 'mcq',
      stem: [ { p: 'What is returned?' }, { code: '>>> 2 ** 3 ** 2 % 5' } ],
      options: [ { l: 'A', t: '4' }, { l: 'B', t: '512' } ],
      answer: 'C',
      explanation: '** is right-associative, so 2 ** (3 ** 2) = 512 and 512 % 5 = 2.'
    },
    {
      n: 2, marks: 3, type: 'fib',
      stem: [
        { p: 'Complete `rev(t)`, which reverses a tuple.' },
        { code: 'def rev(t):\n    if not t:\n        return ()\n    return {{1}} + {{2}}' }
      ],
      blanks: [ { n: 1, answer: 'rev(t[1:])' }, { n: 2, answer: '(t[0],)' } ],
      explanation: 'The head must land at the end, so the recursive call goes first.'
    }
  ]
});
```

Text inside backticks in a `p` block renders as inline code. Use `{{1}}`,
`{{2}}` … inside a `code` block to place a numbered blank chip. A question with
a single blank and no marker gets one free-standing answer box, which is what
the "Answer: ____" items use. Add `freeform: true` to a blank when any
non-empty answer should be accepted.

## Notes on the keys

- The four pack papers and Mock Exam 1 ship with their own answer keys, which
  are used verbatim.
- `cs1010e_2526S1_midterm.pdf` is the question paper only, so every answer for
  that paper was worked out by hand. Two items are worth knowing about:
  - **Mid-Term Q8** asks which expression returns `(1, 3, 5, 7, 2, 4)`-style
    concatenation; the correct result is not listed, so the key is **E**.
    Likewise **Q12** returns 3, which is not listed, so the key is **E**.
  - **Mid-Term Q21** (`r = x % y % z`) is ambiguous: `r < y` and `r < z` are
    both always true, so the item has three valid options. **E** is recorded as
    the key and the explanation says why.
- **Mock Exam 1 Q6** relies on a slice that does not produce any listed
  option; the paper's stated key **A** is kept and flagged in the
  explanation.
- The three added past papers (`2021S1`, `2122S1`, `2223S1`) were transcribed
  from PDFs whose "Answer" column is a placeholder — it reads **A** for every
  single question in both the 2122 and 2223 files, and the 2021 file has no
  answer column at all. Every key for these three papers was therefore worked
  out from the question, and each explanation shows the reasoning. The
  fill-in-the-blank answers are the ones printed in the PDFs.
- **AY 2021/2022 Q23** is flawed in the source: the code actually tests for an
  ascending (non-decreasing) list, which none of the options states. **A** is
  recorded as the key and the explanation says so.
- **Scope tags** (the `scope: { level, reason }` field) exist only on the three
  added past papers, which is where the lectures 1–5 scope sheet applies. The
  other six papers are practice / future papers and are left untagged, so the
  scope filters naturally show every question of those as *in scope*.
