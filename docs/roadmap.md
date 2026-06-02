# QUIZ APP - ROADMAP

## PHASE 1 — PROJECT SETUP

- Create a React project using Vite
- Clean default Vite files
- Confirm the app runs locally
- Create .env.example file
- Configure QuizAPI environment variable

---

## PHASE 2 — PROJECT STRUCTURE

- Create folder structure
- Create screen components
  - HomeScreen.jsx
  - QuizScreen.jsx
  - ResultsScreen.jsx
- Create reusable UI components
- Create API service file
- Create quiz topics data file
- Create utility files
- Create styling structure

---

## PHASE 3 — BASIC UI

Build simple functional screens without focusing on final styling.

### Home Screen

- Display app title
- Display welcome message
- Display topic selector
- Display Start Quiz button

### Quiz Screen

- Display quiz title
- Display question indicator
- Display question text
- Display answer options
- Display Next Question button
- Display Exit Quiz button
- Display timer placeholder

### Results Screen

- Display score
- Display feedback message
- Display Return Home button

---

## PHASE 4 — APP STATE & NAVIGATION

- Create shared state in App.jsx
  - currentScreen
  - selectedTopic
  - totalQuestions
  - score
  - quizStatus
- Implement state-based navigation
- Implement `startQuiz()`
- Implement `finishQuiz()`
- Implement `resetQuiz()`

---

## PHASE 5 — QUESTION SOURCE & API RESEARCH

- Confirm available quiz topics in QuizAPI
- Check if QuizAPI provides enough questions per topic
- Check if each question includes:
  - 4 answer options
  - Correct answer
  - Explanation
- Define the fixed number of questions per quiz
- Determine whether QuizAPI questions can be used directly
- Verify whether custom QuizAPI questions support explanations
- Decide the explanation strategy for the MVP
- Identify any required data transformations

## PHASE 6 — API INTEGRATION

- Configure QuizAPI integration
- Create `quizApi.js` service
- Fetch questions by topic
- Normalize API response
- Validate question structure
- Handle loading state
- Handle API errors

---

## PHASE 7 — QUIZ SESSION SETUP

- Load questions into QuizScreen
- Store questions in local state
- Set totalQuestions
- Display first question
- Implement currentQuestionIndex
- Display question indicator

---

## PHASE 8 — QUESTION & ANSWER RANDOMIZATION

- Randomize question order
- Randomize answer order
- Ensure answer validation uses values instead of indexes
- Keep randomized order fixed during the session

---

## PHASE 9 — ANSWER VALIDATION & FEEDBACK

- Implement answer selection
- Implement immediate validation
- Lock answers after validation
- Display correct answer state
- Display incorrect answer state
- Update score
- Display feedback messages
- Display explanations
- Enable Next Question after validation

---

## PHASE 10 — QUESTION PROGRESSION

- Implement Next Question functionality
- Reset question state between questions
- Detect final question
- Complete quiz when all questions are answered
- Redirect to Results screen

---

## PHASE 11 — GLOBAL TIMER

- Implement countdown timer
- Display remaining time
- Display timer progress bar
- Handle timer expiration
- Redirect to Results screen when time reaches zero

---

## PHASE 12 — EXIT QUIZ FLOW

- Create Exit Quiz modal
- Implement Continue Quiz action
- Implement Exit Quiz action
- Ensure timer continues running while modal is open
- Reset quiz state when exiting
- Return user to Home screen

---

## PHASE 13 — RESULTS SCREEN

- Display score
- Display score as correct answers / total questions
- Display performance message
- Handle completed quiz state
- Handle expired quiz state
- Implement Return Home functionality

---

## PHASE 14 — FINAL UI & STYLING

- Create CSS variables
- Apply color palette
- Apply typography
- Improve layout and spacing
- Style buttons
- Style answer states
- Style feedback messages
- Style timer and progress bar
- Style modal
- Improve responsive design
- Ensure visual consistency

---

## PHASE 15 — TESTING & REFINEMENT

- Test full quiz flow
- Test API integration
- Test score calculation
- Test question randomization
- Test answer randomization
- Test timer behavior
- Test exit flow
- Test error handling
- Fix bugs and edge cases
- Refactor code where needed
