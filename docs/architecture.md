# QUIZ APP - ARCHITECTURE

## OVERVIEW

The Quiz App is a frontend-only React application built with Vite. Users select a programming topic, complete a timed quiz, receive immediate answer validation, view feedback and explanations, and then see a final results summary.

MVP exclusions:

- No backend
- No database
- No authentication
- No local storage
- No React Router

Core architectural decisions:

- `App.jsx` owns app-level navigation and shared quiz session state
- `QuizScreen.jsx` owns the active quiz session UI and local interaction state
- React state is the only state management solution
- API access is isolated in `src/services/quizApi.js`
- Questions and answer options are randomized in the UI layer

## APP STRUCTURE

## APP STRUCTURE

```txt
project-root/
├── src/
│   ├── components/
│   │   ├── QuizSelector.jsx
│   │   ├── QuestionCard.jsx
│   │   ├── AnswerOption.jsx
│   │   ├── FeedbackMessage.jsx
│   │   ├── ExplanationBox.jsx
│   │   ├── TimerBar.jsx
│   │   ├── ExitQuizModal.jsx
│   │   └── ResultsCard.jsx
│   ├── screens/
│   │   ├── HomeScreen.jsx
│   │   ├── QuizScreen.jsx
│   │   └── ResultsScreen.jsx
│   ├── services/
│   │   └── quizApi.js
│   ├── data/
│   │   └── quizTopics.js
│   ├── utils/
│   │   └── shuffleArray.js
│   ├── styles/
│   │   ├── globals.css
│   │   └── variables.css
│   ├── App.jsx
│   └── main.jsx
├── .env
├── .env.example
└── .gitignore
```

### Components

- `QuizSelector.jsx`: topic selection UI
- `QuestionCard.jsx`: current question container and related content
- `AnswerOption.jsx`: individual answer option UI
- `FeedbackMessage.jsx`: correct/incorrect feedback after validation
- `ExplanationBox.jsx`: question explanation after validation
- `TimerBar.jsx`: countdown display and progress bar
- `ExitQuizModal.jsx`: exit confirmation dialog
- `ResultsCard.jsx`: final results summary

### Screens

- `HomeScreen.jsx`: Initial screen where users select a topic and start the quiz.
- `QuizScreen.jsx`: Main quiz screen handling questions, answers, timer, and quiz progression.
- `ResultsScreen.jsx`: Final screen displaying the user's score and feedback.

### Services

- `quizApi.js`: Handles fetching and formatting quiz data from QuizAPI.

### Data

- `quizTopics.js`: Stores the available quiz categories.

### Utilities

- `shuffleArray.js`: Randomizes question and answer order.

### Root Files

- `App.jsx`: Main application controller responsible for shared state and screen navigation.
- `main.jsx`: Application entry point that renders the React application.

### App Controller

`App.jsx` is the main wrapper and controller. It:

- Controls screen navigation with `currentScreen`
- Stores the selected quiz topic
- Stores the total number of questions for the active quiz
- Stores the final score
- Stores overall quiz status
- Exposes shared actions:
  - `startQuiz`
  - `finishQuiz`
  - `resetQuiz`

Navigation is state-based rather than route-based. Valid screen values are:

- `home`
- `quiz`
- `results`

## STYLING ARCHITECTURE

### Global Styles

- `globals.css`
  - Contains global reset rules, typography defaults, body styles, and shared layout rules applied across the application.

- `variables.css`
  - Contains reusable CSS custom properties such as colors, spacing, font sizes, border radius values, shadows, and other design tokens.

### Component Styles

Each component should have its own CSS file located next to the corresponding component file.

Examples:

- `QuizSelector.css`
- `QuestionCard.css`
- `AnswerOption.css`
- `FeedbackMessage.css`
- `ExplanationBox.css`
- `TimerBar.css`
- `ExitQuizModal.css`
- `ResultsCard.css`

These files contain styles specific to their component only.

### Screen Styles

Each screen should have its own CSS file.

Examples:

- `HomeScreen.css`
- `QuizScreen.css`
- `ResultsScreen.css`

These files are responsible for screen-level layout and positioning.


## CSS ORGANIZATION RULES

- Keep global styles inside `globals.css`.
- Keep reusable design values inside `variables.css`.
- Keep component-specific styles inside the component's CSS file.
- Keep screen-specific layout styles inside the screen's CSS file.
- Avoid large centralized CSS files containing styles for unrelated components.
- Avoid styling a component from another component's CSS file.
- Reuse CSS variables whenever possible instead of hardcoding values.
- Use clear and consistent class names.
- Keep styles modular and easy to maintain.


## DESIGN SYSTEM USAGE

All components and screens should use the shared design tokens defined in `variables.css`.

Examples include:

- Colors
- Typography
- Spacing
- Border radius
- Shadows
- Transitions

This ensures visual consistency across the application and simplifies future design updates.

## STATE MANAGEMENT

### App.jsx State

`App.jsx` owns shared state used across screens:

### `currentScreen`

- Controls the active screen
- Values: `home`, `quiz`, `results`

### `selectedTopic`

- Stores the topic selected on the Home screen
- Used to fetch quiz questions and display quiz context

### `totalQuestions`

- Stores the number of normalized questions loaded for the current quiz
- Used on the Results screen

### `score`

- Stores the number of correct answers
- Only correct answers increase the score

### `quizStatus`

- Stores the overall quiz lifecycle
- Values:
  - `idle`: quiz has not started
  - `active`: quiz is running
  - `completed`: all questions were answered before time expired
  - `expired`: the global timer reached zero
  - `cancelled`: the user exited the quiz

### QuizScreen.jsx State

`QuizScreen.jsx` owns active-session state:

### `questions`

- Stores normalized quiz questions for the active session
- Questions are randomized once after loading and remain fixed for that session

### `currentQuestionIndex`

- Tracks which question is currently displayed

### `selectedAnswer`

- Stores the answer value selected for the current question

### `hasAnswered`

- Indicates whether the current question has been answered
- Used to lock answers, show feedback, show explanation, and enable the Next Question button

### `remainingTime`

- Stores the global countdown timer value for the full quiz
- Drives the timer display, progress bar, and expiration behavior

### `isExitModalOpen`

- Controls the exit confirmation modal
- The global timer continues running while this modal is open

### `isLoading`

- Tracks question loading state

### `error`

- Stores loading or API errors

## SCREEN RESPONSIBILITIES

### HomeScreen.jsx

- Displays the app title or logo, welcome text, topic selector, and Start Quiz button
- Lets the user choose a topic
- Passes the selected topic to `App.jsx`
- Triggers `startQuiz()` and moves the user to the quiz screen

### QuizScreen.jsx

- Displays the quiz title or app title, timer, progress bar, question indicator, current question, answer options, feedback, explanation, Next Question button, Exit Quiz button, and exit modal
- Receives `selectedTopic` from `App.jsx`
- Fetches, normalizes, and randomizes quiz questions
- Reports `totalQuestions` to `App.jsx`
- Manages question progression, answer validation, scoring updates, timer behavior, and exit confirmation
- Redirects to Results when the quiz is completed or expired

### ResultsScreen.jsx

- Displays the final score and a result feedback message
- Receives `score`, `totalQuestions`, and `quizStatus`
- Lets the user return Home and reset the quiz

## DATA FLOW

### Ownership

- `App.jsx` owns cross-screen state and navigation
- `QuizScreen.jsx` owns in-progress quiz interaction state
- Presentational components receive data and callbacks via props
- `quizApi.js` fetches and formats API data before the UI uses it

### Flow Between Layers

1. `HomeScreen.jsx` updates `selectedTopic` through `App.jsx`.
2. `App.jsx` switches `currentScreen` from `home` to `quiz`.
3. `QuizScreen.jsx` requests questions through `quizApi.js`.
4. `quizApi.js` fetches, validates, and formats the API response.
5. `QuizScreen.jsx` stores randomized questions locally and sends `totalQuestions` upward to `App.jsx`.
6. Quiz interactions update local quiz state; score and final quiz status update shared state in `App.jsx`.
7. `App.jsx` switches to `results` when the quiz finishes or expires.
8. `ResultsScreen.jsx` reads final shared state and offers reset navigation back to `home`.

## QUIZ FLOW

### Session Start

1. The user lands on `HomeScreen.jsx`.
2. The user selects a topic and starts the quiz.
3. `App.jsx` stores `selectedTopic`, sets `quizStatus` to active, and switches `currentScreen` to `quiz`.
4. `QuizScreen.jsx` fetches questions for the selected topic.
5. Questions are normalized, randomized, stored in `questions`, and counted in `totalQuestions`.
6. The global timer starts only after questions load successfully.
7. The first question appears with Next Question disabled.

### Answer Validation

Answer validation is immediate. There is no Submit Answer button in the MVP.

When the user selects an answer:

1. Store the selected answer in `selectedAnswer`.
2. Set `hasAnswered` to `true`.
3. Compare the selected answer value with `currentQuestion.correctAnswer`.
4. If correct, increment `score`.
5. Show visual feedback:
   - Correct selection turns green
   - Incorrect selection turns red
   - Correct answer turns green when the selected answer is wrong
6. Show the feedback message.
7. Show the explanation.
8. Lock all answer options.
9. Enable Next Question.

### Question Progression

When the user clicks Next Question:

1. If another question exists, increment `currentQuestionIndex`.
2. Reset `selectedAnswer`.
3. Reset `hasAnswered`.
4. Hide feedback and explanation by returning the next question to its initial state.
5. Disable Next Question until the next answer is validated.

If the current question is the last one:

1. Finish the quiz.
2. Set `quizStatus` to `completed`.
3. Move to the Results screen.

### Timer Behavior

The quiz uses one global countdown timer for the entire session.

Rules:

- The timer starts when the quiz begins successfully
- The timer remains visible during the quiz
- The progress bar decreases with `remainingTime`
- The timer continues running while confirmation modals are open
- The timer stops when the quiz is completed, expired, or cancelled

When `remainingTime` reaches zero:

1. Set `quizStatus` to `expired`.
2. Stop the timer.
3. Count unanswered questions as incorrect.
4. Move the user to the Results screen.

No additional score adjustment is required because only correct answers increase `score`.

### Exit Flow

When the user clicks Exit Quiz:

1. Set `isExitModalOpen` to `true`.
2. Show the confirmation modal.
3. Keep the timer running.

Modal actions:

- Continue Quiz: close the modal and resume the current session with the timer still running
- Exit Quiz:
  1. Reset the quiz session
  2. Clear the timer
  3. Clear `selectedTopic`
  4. Reset `score` to `0`
  5. Set `quizStatus` to `cancelled`
  6. Set `currentScreen` to `home`

### Results Flow

The Results screen is shown when:

- All questions are answered
- The timer reaches zero

It receives:

- Final `score`
- `totalQuestions`
- `quizStatus`

## API LOGIC

Quiz questions are fetched from QuizAPI through `src/services/quizApi.js`.

That service is responsible for:

- Fetching questions from QuizAPI
- Filtering by selected topic if needed
- Formatting API data into the app's internal shape
- Handling API response errors
- Rejecting invalid or incomplete question data

### Internal Question Format

```json
{
  "id": "question-id",
  "question": "What does HTML stand for?",
  "answers": [
    "HyperText Markup Language",
    "Home Tool Markup Language",
    "Hyper Transfer Markup Link",
    "HyperText Machine Language"
  ],
  "correctAnswer": "HyperText Markup Language",
  "explanation": "HTML stands for HyperText Markup Language. It is used to structure content on web pages."
}
```

Each question must include:

- `id`
- `question`
- `answers`
- `correctAnswer`
- `explanation`

If a question has no explanation, show a fallback message such as:

`No explanation available for this question.`

The same explanation is shown whether the answer is correct or incorrect.

## ENVIRONMENT VARIABLES

### Required Variables

```env
VITE_QUIZ_API_KEY=your_api_key_here
```

Notes
- Environment variables are accessed through import.meta.env.
- API keys should not be hardcoded in source files.
- .env should be included in .gitignore.
- .env.example should be committed to the repository.

## QUIZ LOGIC RULES

### Question Randomization

- All questions returned for the selected topic are included in the session
- Questions are randomized once at quiz start
- The randomized order stays fixed for that session
- Questions are not removed or replaced during the session

### Answer Randomization

- Answer options are randomized before display
- The correct answer position must not be fixed
- Validation must compare answer values, not indexes

Correct approach:

`selectedAnswer === currentQuestion.correctAnswer`

Avoid:

`selectedAnswerIndex === correctAnswerIndex`

Example:

`const randomizedAnswers = shuffleArray(question.answers);`

### Navigation and Validation Rules

- One question is displayed at a time
- Questions are answered sequentially
- Only one answer can be selected per question
- There is no previous-question navigation
- Answers cannot be edited after validation
- Next Question stays disabled until validation completes

## ERROR HANDLING

The app should handle:

- API request failure
- Empty question response
- Invalid or incomplete question data
- Missing explanation
- Timer errors

If loading fails:

- Show an error message
- Allow the user to return Home
- Do not start the timer until questions load successfully

## MVP DECISIONS

- Use React
- Use JavaScript
- Use CSS
- Use Vite
- Use React state only
- Use QuizAPI
- No React Router
- No backend
- No database
- No authentication
- No local storage
- No saved results
- One question displayed at a time
- Global countdown timer
- Timer progress bar
- Timer continues running while confirmation modals are open
- Immediate answer validation
- No Submit Answer button
- Answers locked after validation
- No previous question navigation
- No answer editing
- Feedback message shown after validation
- Explanation shown after validation
- Same explanation shown for correct and incorrect answers
- Next Question button disabled until answer validation
- Questions randomized at quiz start
- Answer options randomized before display
- API logic separated from UI logic
