# DevQuiz — Layout & UI Pattern Specification

**Source:** Figma file `Quiz-App`, page "UI Design" that contains Home, Quiz, and Results frames plus the Exit Confirmation modal. Link: https://www.figma.com/design/bjSH4xpgWptKjggpjRC2y6/Quiz-App?node-id=4-31&p=f&t=zNJawqOzk5e19gVk-0

**Scope:** Layout structure and UI component patterns, which control type is used for each element and where it sits in the screen.

**Out of scope:** Final styling, colors, typography, spacing, **and responsiveness**. This document describes the layout and pattern decisions.

---

## 1. Global Layout Framework

Every screen is a single centered content panel on a background canvas, with a small decorative triangular motif at opposing corners.

- **Content panel:** one centered card holding all screen content. Children stack in a single vertical column, center-aligned, with consistent gaps between sections.
- **Header:** the `< devquiz >` logo appears at the top of every screen. The Quiz screen extends the header with a timer, progress bar, question counter, and exit control (see §3).
- **Body:** a single column. The only multi-element grids are the topic selector (Home) and the answer options (Quiz).

---

## 2. Home Screen

Order: header (logo + copy) → topic selector → "Start Quiz" button.

### 2.1 Topic Selector — **Decision: selectable cards in a grid**

Cards, not a dropdown, radio list, or plain button row. Each topic is a card containing a topic icon, the topic name, and a metadata line ("N questions · M min").

- **Selection model:** single-select. The chosen card gets a distinct selected treatment plus a check badge pinned to its top-right corner.
- **Affordance:** the whole card is the target; a hover state exists.

**Rationale:** cards carry per-topic metadata (question count, duration) inline and give a large target — a dropdown or button row can't do that as cleanly.

**Implementation:** each card is a `<label>` wrapping a visually hidden `<input type="radio">`. The radio group provides native single-select semantics and keyboard navigation; the card visual state is driven by the `:checked` pseudo-class on the input.

### 2.2 Header / Copy

Centered column: logo, a display heading, and a one-line subtitle/instruction.

### 2.3 Start Quiz Button — **Placement: below the grid, centered**

A single primary button ("START QUIZ" + arrow), centered beneath the topic grid. It is the only primary action on the screen and is enabled once a topic is selected.

---

## 3. Quiz Screen

Order: quiz header → question → answer options → feedback → Next button.

### 3.1 Timer — **Placement: header, top-right**

A clock icon + countdown, right-aligned on the same row as the logo.

### 3.2 Progress Bar — **Placement: directly below the header row, full width**

A full-width pill track with a fill segment indicating progress through the question set.

### 3.3 Question Counter — **Placement: row below the progress bar, left**

"QUESTION X OF 10", left-aligned, opposite the Exit control.

### 3.4 Question Presentation — **Decision: text block in the main container, not a bordered sub-card**

The question is a heading-level text block sitting directly in the panel. It is *not* wrapped in its own card or bordered box, keeping visual weight on the answers.

### 3.5 Answer Options — **Decision: lettered option tiles in a grid**

Each option is a bordered, rounded tile containing a letter chip (A/B/C/D in its own boxed token) plus the answer text.

- **Selection model:** single-select; selecting an option reveals feedback.
- **States:** default, hover, selected-correct, selected-incorrect. On an incorrect pick, the correct option is also highlighted.

**Implementation:** same pattern as the topic selector — each tile is a `<label>` wrapping a visually hidden `<input type="radio">`. After validation, a `data-state` attribute is set on each `<label>` to drive the post-validation visual states via CSS:

- `data-state="correct"` — the option the user picked and it is right
- `data-state="incorrect"` — the option the user picked and it is wrong
- `data-state="reveal"` — the correct answer, shown when the user picked wrong
- No attribute — untouched option

All inputs receive `disabled` after validation to lock the group.

### 3.6 Feedback & Explanation — **Placement: below the answer grid, above the Next button**

After answering, an in-flow feedback block appears (it pushes the Next button down; it is not an overlay):
1. **Status row:** an icon (correct/incorrect) + a status label ("CORRECT!" / the incorrect equivalent).
2. **Explanation:** a dashed-border box with a "Why:" prefix followed by a one-sentence explanation.

### 3.7 Next Question Button — **Placement: bottom of the body, centered**

A single primary button ("NEXT QUESTION" + arrow), centered below the feedback block. Appears once an answer is submitted; on the final question it advances to Results.

### 3.8 Exit Quiz Button — **Placement: question-counter row, right**

A low-emphasis text control ("EXIT QUIZ" + close glyph), right-aligned opposite the counter. Opens the Exit Confirmation modal (§5).

---

## 4. Results Screen

Order: header (logo) → title → score gauge → result copy → Return Home button.

### 4.1 Results Summary Layout — **Decision: centered single column with a circular score gauge**

1. **Title** — "Quiz complete", centered.
2. **Score gauge** — a circular progress ring with the score as a fraction in its center ("7/10") above a "SCORE" label.
3. **Result copy** — a short headline ("Great job!") plus a one–two line encouragement paragraph.
4. **Return Home button.**

### 4.2 Return Home Button — **Placement: bottom of the body, centered**

A single primary button ("RETURN HOME" + arrow); the only action on the screen, routing back to Home.

---

## 5. Exit Confirmation Modal

- **Trigger:** "EXIT QUIZ" on the Quiz screen.
- **Type:** centered modal dialog over the screen; underlying screen is inert while open.
- **Structure (single centered column):**
  1. **Logo.**
  2. **Message:** title "Are you sure you want to quit?" + body "Progress will be lost."
  3. **Buttons** — two full-width buttons **stacked vertically**:
     - **Primary / dismiss (filled, top):** "CONTINUE QUIZ →" — closes the modal and resumes the quiz. Emphasized action.
     - **Secondary / confirm (outlined, bottom):** "EXIT QUIZ →" — abandons the quiz and returns Home. De-emphasized.
- **Emphasis:** the safe action (continue) is dominant; the destructive action (exit) is de-emphasized and outlined. Buttons are stacked, not side by side.

---

