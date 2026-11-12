# Bilingual quiz builder

**GitHub Copilot Hackathon: Build Smart, Spend Smart** · Education breakout · Challenge B: Workflow app

Teachers build quizzes from their own question bank, students take them in Arabic or English and every score is recorded.

> [!IMPORTANT]
> Anonymised sample data only. Never bring real student data or credentials into this repo.

## Start here

1. Select **Use this template** > **Create a new repository** to create your team's copy. Make it public, or give the judges access.
2. Clone it and open it in your IDE with GitHub Copilot Chat.
3. Read this brief and the [data dictionary](data/README.md).
4. **Before you build**, plan with Copilot Plan mode and estimate your AI Credits per phase in [docs/ai-credit-budget.md](docs/ai-credit-budget.md).
5. Build, test, review and document with Copilot. Commit often: your commit history is evidence.
6. Before your demo, complete [Your solution](#your-solution) at the end of this README.

## The day at a glance

The day has two goals:

1. Use GitHub Copilot beyond autocomplete: plan, agent, instructions, prompt files and review.
2. Build the habit of planning AI Credit consumption before you start prompting.

| | |
|---|---|
| **Team** | 3–5 developers |
| **Build time** | 4 hours |
| **Usage checkpoints** | Your facilitator records each seat's AI Credit usage before the build starts, at the mid-point check (2:00) and at the end. |
| **Demo** | 5 minutes, then Q&A |
| **Scoring** | 100 points. See the [scoring rubric](#scoring-rubric). |

## The challenge

**Who it's for:** teachers and students at a bilingual (Arabic and English) school.

**The problem:** teachers keep their question banks in spreadsheets and build every quiz by hand. Many quiz tools also break in right-to-left Arabic layouts, so Arabic questions with numbers or English terms come out scrambled.

**What to build:** a workflow app where teachers create and publish quizzes from their course's question bank, students take them and teachers see each student's score and percentage. It works in Arabic and English, with full right-to-left support.

### Workflow

| From | Action | To | Who can do it |
|---|---|---|---|
| (new) | Create a quiz: title, course, language, questions from the bank, open and close times | `DRAFT` | The course's teacher |
| `DRAFT` | Edit, add or remove questions | `DRAFT` | The course's teacher |
| `DRAFT` | Publish | `PUBLISHED` | The course's teacher |
| `PUBLISHED` | Submit an attempt | (attempt recorded) | An enrolled student, while the quiz is open |
| `PUBLISHED` | Close (or automatically at the close time) | `CLOSED` | The course's teacher |

### Rules

| Rule ID | Rule |
|---|---|
| `TEACHER_OWNS_COURSE` | Teachers manage quizzes only for courses they teach. |
| `QUESTION_LANGUAGE` | Every question in a quiz matches the quiz language (`ar` or `en`). |
| `PUBLISH_READY` | A quiz needs at least one question to be published, and can't be edited once published. |
| `ENROLLED_STUDENT` | Only students enrolled in the course can take its quizzes. |
| `QUIZ_OPEN` | Attempts are accepted only while the quiz is `PUBLISHED` and between its open and close times. |
| `ONE_ATTEMPT` | One attempt per student per quiz. |
| `SCORING` | Score = the sum of points for correct answers; unanswered = 0. Percentage = score ÷ total points, rounded to one decimal place. |
| `TEACHER_WRITTEN` | Questions come only from the teachers' question bank. Don't generate questions with AI. |

The defaults are a starting point. You can change them if you record the change in your plan and test it. When a rule refuses an action, show the rule and the reason in the user's language, for example `ONE_ATTEMPT: you've already submitted this quiz`.

### Arabic and English

Both languages are must-haves:

- Every UI string exists in both languages, in resource files. No hard-coded text.
- The Arabic UI is right-to-left: `dir="rtl"`, a mirrored layout and logical CSS properties, such as `margin-inline-start` instead of `margin-left`.
- Quiz content keeps its own direction: an English quiz inside the Arabic UI still reads left to right.
- Mixed text, such as Arabic with English terms or numbers, displays correctly.
- Numbers and dates follow the selected language. Decide whether Arabic uses Arabic-Indic digits (٠١٢٣٤٥٦٧٨٩).

Decide these in your plan and cover them with tests:

- **After submitting:** what students see after they submit, for example their score now and the correct answers only after the quiz closes.
- **Running out of time:** what happens when the close time passes during an attempt.
- **Invalid questions:** what the app does with questions in the bank that can't be used as they are, and how the teacher finds out.
- **Boundaries:** test an attempt submitted exactly at the close time, and percentage rounding: is 13 of 16 points (81.25%) shown as 81.2% or 81.3%?
- **Messy data:** the data has deliberate quality issues. See [Things to know](data/README.md#things-to-know).

### What to build

**Must have**

- [ ] Load the CSV files in [`data/`](data/).
- [ ] A teacher flow: create, edit, publish and close quizzes, then view the results.
- [ ] A student flow: see open quizzes, take a quiz and see the score.
- [ ] All the rules, each with a clear message when it refuses an action.
- [ ] An Arabic and English UI with right-to-left support.
- [ ] Automated tests for scoring and the rules, plus a check that every UI string exists in both languages.

**Stretch goals (optional)**

- Shuffle the question and option order for each student, and keep scoring correct.
- Per-question results for teachers: which questions most students missed.
- Export results to CSV.
- Timed quizzes: a time limit for each attempt.
- Accessibility: keyboard navigation and screen-reader labels in both languages.

Use any language, framework or database, as long as it runs locally from your README. No machine learning is needed: plain business rules only.

## Starter data

| File | Rows | Contents |
|---|---:|---|
| [`courses.csv`](data/courses.csv) | 6 | Grade 8 and grade 9 courses, with their teacher and language of instruction |
| [`users.csv`](data/users.csv) | 44 | 4 teachers and 40 students, with names in English and Arabic and a preferred language |
| [`enrollments.csv`](data/enrollments.csv) | 128 | Which students take which courses |
| [`question_bank.csv`](data/question_bank.csv) | 90 | The teachers' question bank: multiple-choice and true/false questions, 50 in Arabic and 40 in English |

All data is anonymised sample data, and the names are fictional first names. The sample questions are teacher-style examples for testing: simple school questions in maths, science, English, Arabic grammar, ICT and social studies. The [data dictionary](data/README.md) explains every column and the data-quality issues.

## Copilot focus for this challenge

Every team should use the practices the rubric scores: Plan mode, agent mode, a `.github/copilot-instructions.md` file with your team's conventions, reusable prompt files and Copilot Chat review. This challenge puts extra weight on three of them:

| Practice | How to apply it here |
|---|---|
| **Instructions file for i18n** | Put your i18n rules in `.github/copilot-instructions.md`: no hard-coded strings, one resource file per language, logical CSS properties, `dir` attributes and `Intl` formatting for numbers and dates. Agent mode then follows them in every file. |
| **Prompt files for tests** | Save reusable prompts in `.github/prompts/` for scoring tests and i18n checks, for example a check that every key exists in both languages. |
| **Copilot Chat review** | Ask Copilot Chat to review a page for right-to-left and i18n problems, and the scoring for bugs. Fix the findings and note what it caught. |

<details>
<summary>Example prompts</summary>

**Instructions file**

```text
Read README.md and data/README.md. Draft .github/copilot-instructions.md with our i18n rules:
UI strings in ar and en resource files, dir="rtl", logical CSS properties and Intl formatting.
```

**Tests** (for example, saved as `.github/prompts/scoring-tests.prompt.md`)

```text
Write unit tests for the scoring in the current file. Cover correct, wrong and unanswered
questions, mixed points, an attempt with no answers and percentage rounding, such as
13 of 16 points. Follow our test conventions.
```

**Review**

```text
Review this page for right-to-left and i18n problems, such as hard-coded text, physical CSS
properties and mixed-direction text. Then review the scoring for bugs. List the findings by
severity. Don't change any code yet.
```

</details>

## AI Credit budget

Estimate AI Credits per phase before you build, then track what you actually use in [docs/ai-credit-budget.md](docs/ai-credit-budget.md). The example below is from the kickoff and assumes a team of 3–5 (950 credits is about USD 9.50). Make your own estimate: the point is the habit of estimating, not hitting an exact number.

| Phase | Copilot feature | Model tier | Budget (AI Credits) |
|---|---|---|---:|
| Plan | Plan mode / Ask | Frontier | 150 |
| Scaffold | Agent mode + instructions file | Low-cost | 100 |
| Build features | Agent / Edit mode | Mixed | 500 |
| Tests + review | Prompt file for tests, chat review | Low-cost | 150 |
| Docs + demo | Ask, README generation | Low-cost | 50 |
| **Total** | | | **950** |

Tips to spend smart:

- Match the model to the task. Save frontier models for planning and tricky logic, and use low-cost models for scaffolding, tests and docs.
- Keep context scoped. Attach only the files Copilot needs.
- Reuse prompt files instead of retyping long prompts.

## Scoring rubric

The rubric awards 100 points across the lifecycle. It's the same in all three sectors, so winners are comparable.

| Stage | Points |
|---|---:|
| Plan | 15 |
| Build & quality with Copilot | 40 |
| Token efficiency | 25 |
| Impact & demo | 20 |
| **Total** | **100** |

### How scoring works

- Judges pick one level per stage: **Solid**, **Strong** or **Outstanding**. Each level is worth the points shown.
- Your score is the sum of the four stage scores. A stage with no evidence scores 0.
- Build & quality plus Token efficiency carry 65 points: they're the two goals of the day.
- Judges score from evidence, not claims: your repo, budget sheet, usage export and demo.
- Tie-break: the team with the higher Token efficiency score wins.
- Solid is the expected baseline. Most teams should reach Strong in four hours. Outstanding should stand out.

### Levels

| Stage | Solid | Strong | Outstanding |
|---|---|---|---|
| **Plan**<br>15 points | **5 pts**<br>Plan written with Copilot Plan mode; credit budget estimated per phase. | **10 pts**<br>Plan has acceptance criteria and a model choice per phase, with the reasoning explained. | **15 pts**<br>Plan drove the build, and the team re-planned at the mid-point check using actual usage. |
| **Build & quality with Copilot**<br>40 points | **16 pts**<br>Agent mode built the main features; `copilot-instructions.md` sets team conventions; runs end to end with tests on the core logic. | **28 pts**<br>Adds reusable prompt files for repeat tasks (tests, docs); edge cases tested; Copilot asked in chat to review the code and findings fixed. | **40 pts**<br>Copilot used across the whole loop (plan, build, test, review, docs); team shows where it saved the most time and where they had to correct it; another team could run it from the README. |
| **Token efficiency**<br>25 points | **10 pts**<br>Usage tracked; actual within ±30% of budget. | **18 pts**<br>Within ±15% of budget; models matched to tasks; context kept scoped. | **25 pts**<br>A concrete before/after where a prompt, context or model change cut credits without hurting the output. |
| **Impact & demo**<br>20 points | **8 pts**<br>Clear user and problem; working demo in 5 minutes. | **14 pts**<br>Rules or workflow are realistic for the sector; Q&A handled well. | **20 pts**<br>Credible path to a real pilot; judges can picture it being used. |

### What judges look at

| Stage | Evidence |
|---|---|
| Plan | `docs/plan.md` from Plan mode, and the budget and mid-point re-plan in `docs/ai-credit-budget.md` |
| Build & quality | `.github/copilot-instructions.md`, `.github/prompts/*.prompt.md`, tests, commit history and this README |
| Token efficiency | Budget versus actual usage in `docs/ai-credit-budget.md`, the usage export, and your before/after example |
| Impact & demo | Your 5-minute demo and Q&A |

### Clarifications

- **Token efficiency isn't about using the fewest credits.** It's judged against your own budget plus proof of optimization, so a team that barely used Copilot can't win it by spending little. Judges reward value delivered per credit. The ±30% and ±15% bands apply to overspending and underspending.
- **IDE features are enough.** Every level can be reached with Copilot Chat features in the IDE: Plan mode, agent mode, instructions, prompt files and chat review. If your organization also has Copilot code review on pull requests, MCP or the Copilot coding agent, they count as extra evidence for Outstanding, but they're never required.
- **Sector judgement counts.** Sector judges apply domain judgement in Impact & demo. For this challenge, strong teams make Arabic a first-class experience, not a translation added at the end, and keep question writing in teachers' hands.

## Your solution

> [!NOTE]
> Complete this section before your demo. Judges, and other teams, will run your app from it.

- **Team name:**
- **Team members:**
- **Stack:**

### Run it

```bash
# Replace with the exact steps to install and run your app.
```

### Test it

```bash
# Replace with the exact command that runs your tests.
```

### Rule-to-test traceability

| Rule | Implemented in | Tested in |
|---|---|---|
| `TEACHER_OWNS_COURSE` | | |
| `QUESTION_LANGUAGE` | | |
| `PUBLISH_READY` | | |
| `ENROLLED_STUDENT` | | |
| `QUIZ_OPEN` | | |
| `ONE_ATTEMPT` | | |
| `SCORING` | | |
| `TEACHER_WRITTEN` | | |
| Arabic and English (right-to-left) | | |

### Copilot retrospective

- **Where Copilot saved us the most time:**
- **Where we had to correct it:**

### Evidence checklist

- [ ] Plan: `docs/plan.md`
- [ ] Budget, actual usage, mid-point re-plan and before/after example: [`docs/ai-credit-budget.md`](docs/ai-credit-budget.md)
- [ ] Usage export: `docs/usage/`
- [ ] Team conventions: `.github/copilot-instructions.md`
- [ ] Prompt files: `.github/prompts/`
