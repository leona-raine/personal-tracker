# Personal Tracker: Project Plan

## Overview

`personal-tracker` is a learning app that combines note-taking, focus timers, habit tracking, and study methods in one place. It is for anyone who studies, whether for school, work, or personal interest. It also helps users balance study time with entertainment by tracking time spent on music, movies, games, and social media during breaks.

## Goals

- Build my first software app to understand the full software engineering workflow: planning, branching, pull requests, testing, and deployment.
- Use the app in my own daily studying.
- Establish my first complete software engineering project.

## Non-goals

- Deployment and app store releases are not planned for now.
- No native desktop or mobile apps until the web version is stable.
- No automatic screen-time tracking in early versions. Usage limits use manual timers.
- The app does not block other apps or detect activity outside the app. Breaks are tracked only when started from the app.
- No scraping of services that do not offer a public API.

## Scope

### v1: Accounts and notes

- Sign up, log in, log out
- Create, edit, and delete notes
- Notes support Markdown text and code blocks with syntax highlighting
- Organize notes by subject

### v2: Focus timer and habits

- Focus timer with selectable methods: Pomodoro, 52/17, Custom
- Link a study session to a subject
- Habit tracker with daily check-ins and streaks

### v3: Study tools and stats

- Flashcards with spaced repetition
- Note templates: Cornell, Outline, Free-form
- Study stats: time per subject, per week, and streaks

### v4: Balance

- Break mode with activity types: Watch movie, Game, Social media, Other
- Pick a service (e.g. Netflix, HBO Max, Prime Video, Disney+, Steam) and open it from the app
- Break timer with a time limit
- Notifications when a limit is reached
- Play bank: earn break time by completing study sessions

### v5: Integrations

- Now playing through Last.fm
- Steam and Riot playtime
- Watch history through Trakt
- More study methods: Flowtime, Feynman, Blurting, Interleaving
- "What works for you" stats based on study method usage

## Study methods

### Timing

| Method | Focus | Break | Notes |
|---|---|---|---|
| Pomodoro | 25 min | 5 min | 15 min break after 4 rounds |
| 52/17 | 52 min | 17 min | |
| Flowtime | Open | Based on focus length | |
| Timeboxing | Fixed per task | Optional | |
| Custom | User-defined | User-defined | |

### Learning

| Method | App feature |
|---|---|
| Active recall | Flashcards, quiz mode |
| Spaced repetition | Flashcard scheduling |
| Feynman technique | Guided note template |
| Blurting | Blank page, then compare with notes |
| Interleaving | Session that rotates subjects |

### Note-taking

| Method | Structure |
|---|---|
| Cornell | Notes, cues, summary |
| Outline | Headings and nested bullets |
| Free-form | Markdown with code blocks |

## Data model

### User

| Field | Type | Notes |
|---|---|---|
| id | string | Provided by the auth service |
| email | string | Required, unique |
| displayName | string | Optional |
| createdAt | date | |

### Subject

| Field | Type | Notes |
|---|---|---|
| id | string | |
| userId | string | Owner |
| name | string | Required, e.g. "React" or "Calculus" |
| color | string | Optional, for display |

### Note

| Field | Type | Notes |
|---|---|---|
| id | string | |
| userId | string | Owner |
| subjectId | string | Optional |
| title | string | Required |
| content | string | Markdown, including code blocks |
| createdAt | date | |
| updatedAt | date | |

### Habit

| Field | Type | Notes |
|---|---|---|
| id | string | |
| userId | string | Owner |
| name | string | Required |
| frequency | string | e.g. "daily" or "weekly" |
| completedDates | date[] | Used to calculate streaks |
| createdAt | date | |

### Study session

| Field | Type | Notes |
|---|---|---|
| id | string | |
| userId | string | Owner |
| subjectId | string | Optional |
| method | string | e.g. "pomodoro", "52-17", "custom" |
| startedAt | date | |
| endedAt | date | |
| focusMinutes | number | Total focus time |
| completed | boolean | Whether the session was finished |

### Break session

| Field | Type | Notes |
|---|---|---|
| id | string | |
| userId | string | Owner |
| activity | string | "movie", "game", "social", "other" |
| service | string | e.g. "netflix", "steam", or a custom name |
| limitMinutes | number | Time limit chosen at the start |
| startedAt | date | |
| endedAt | date | |
| overLimit | boolean | Whether the break went past the limit |

## Screens and routes

| Route | Screen | Main components |
|---|---|---|
| `/login` | Log in | LoginForm |
| `/signup` | Sign up | SignupForm |
| `/` | Dashboard | Today's habits, recent notes, start session button |
| `/notes` | Notes list | SubjectFilter, NoteList |
| `/notes/:id` | Note editor | NoteEditor, CodeBlock |
| `/timer` | Focus timer | MethodPicker, Timer, SessionSummary |
| `/break` | Break mode | ActivityPicker, ServicePicker, BreakTimer |
| `/habits` | Habit tracker | HabitList, HabitCalendar |
| `/settings` | Settings | ProfileForm, LogoutButton |

## Tech stack

| Area | Choice | Reason |
|---|---|---|
| Framework | React (Vite) | Most widely used, with the most learning resources |
| Language | JavaScript | One new thing at a time. May move to TypeScript later |
| Routing | React Router | Separate pages with their own URLs |
| Auth and database | Supabase | Login, database, and storage without building a backend |
| Code editor | CodeMirror | Syntax-highlighted code blocks in notes |
| Testing | Vitest, Testing Library | Fast tests that work with Vite |
| Linting and formatting | ESLint, Prettier | Consistent, error-checked code |

## Testing

- Unit tests for logic: streak calculation, timer math, play bank balance.
- Component tests for key flows: creating a note, starting a session.
- Lint and tests must pass before merging a pull request.

## Decisions

| Date | Decision | Reason |
|---|---|---|
| 2026-10-24 | Use React with Vite and JavaScript | Most learning resources, fewer new concepts at once |
| 2026-10-24 | Use ESLint and Prettier | Standard tools with the most support |
| 2026-10-24 | Protect `main` and use pull requests for every change | Practice a team workflow |
| 2026-10-25 | Change the app's focus from a general tracker to a learning app | Clearer purpose, and every feature supports studying |
| 2026-10-25 | Track movie, game, and social media time through break sessions started in the app | Streaming services have no public API, and a manual timer works on every platform |

## Open questions

- Should the project and repo be renamed to match the learning app focus?
- Should Supabase be added in v1, or should v1 use localStorage and add login in v2?
- Should habits store completed dates as a list, or as separate check-in records?
- How should the play bank convert study time into entertainment time?
- Which notification method works best on both desktop and mobile?

## Security and privacy

- Each user can only read and change their own data, enforced in the database with Supabase Row Level Security.
- API keys and secrets are stored in `.env` files and never committed.
- Passwords are handled by Supabase Auth, never stored by the app.

## Risks

| Risk | Plan |
|---|---|
| Scope grows too large | Finish each version before starting the next |
| Timers drift or pause when the tab is in the background | Calculate time from start and end timestamps, not by counting seconds |
| Notifications on iPhone only work if the app is installed to the home screen | Note this in the app, and test early |
| Third-party APIs change or close | Keep integrations in v5, and design the app to work without them |