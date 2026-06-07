# QUIZ-APP FINAL PROJECT STRUCTURE

## Purpose

This document defines the final folder and file structure for the Quiz App MVP.

## Final Structure

```txt
project-root/
|-- docs/
|   |-- PRD.md
|   |-- architecture.md
|   |-- roadmap.md
|   `-- FINAL_PROJECT_STRUCTURE.md
|-- src/
|   |-- components/
|   |   |-- QuizSelector.jsx
|   |   |-- QuizSelector.css
|   |   |-- QuestionCard.jsx
|   |   |-- QuestionCard.css
|   |   |-- AnswerOption.jsx
|   |   |-- AnswerOption.css
|   |   |-- FeedbackMessage.jsx
|   |   |-- FeedbackMessage.css
|   |   |-- ExplanationBox.jsx
|   |   |-- ExplanationBox.css
|   |   |-- TimerBar.jsx
|   |   |-- TimerBar.css
|   |   |-- ExitQuizModal.jsx
|   |   |-- ExitQuizModal.css
|   |   |-- ResultsCard.jsx
|   |   `-- ResultsCard.css
|   |-- screens/
|   |   |-- HomeScreen.jsx
|   |   |-- HomeScreen.css
|   |   |-- QuizScreen.jsx
|   |   |-- QuizScreen.css
|   |   |-- ResultsScreen.jsx
|   |   `-- ResultsScreen.css
|   |-- services/
|   |   `-- quizApi.js
|   |-- data/
|   |   `-- quizTopics.js
|   |-- utils/
|   |   `-- shuffleArray.js
|   |-- styles/
|   |   |-- globals.css
|   |   `-- variables.css
|   |-- App.jsx
|   `-- main.jsx
|-- .env.example
|-- .gitignore
|-- eslint.config.js
|-- index.html
|-- package.json
|-- package-lock.json
`-- vite.config.js
```

## File Responsibilities

- `src/App.jsx`: App-level controller for screen state and shared quiz session state.
- `src/main.jsx`: React entry point.
- `src/screens/*`: Top-level screens (`home`, `quiz`, `results`).
- `src/components/*`: Reusable UI blocks for quiz flow.
- `src/services/quizApi.js`: API integration and question formatting.
- `src/data/quizTopics.js`: Quiz categories/topics list.
- `src/utils/shuffleArray.js`: Utility for randomization.
- `src/styles/variables.css`: Shared design tokens.
- `src/styles/globals.css`: Reset and app-wide base styles.

## Notes

- Navigation is state-based in `App.jsx` (no router for MVP).
- Keep component styles in component-level `.css` files.
- Keep screen layout styles in screen-level `.css` files.
- Keep only reusable tokens in `variables.css`.
