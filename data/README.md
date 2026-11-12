# Data dictionary

All files are anonymised sample data generated for this hackathon. They contain no real students, teachers or schools, and the names are fictional first names. The questions are teacher-style examples for testing.

| File | Rows | One row per |
|---|---:|---|
| [`courses.csv`](courses.csv) | 6 | Course |
| [`users.csv`](users.csv) | 44 | Teacher or student |
| [`enrollments.csv`](enrollments.csv) | 128 | Student in a course |
| [`question_bank.csv`](question_bank.csv) | 90 | Question in a course's question bank |

Every file is a UTF-8, comma-separated CSV with a header row. Read [Things to know](#things-to-know) before you open them in Excel.

## courses.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `course_id` | Text | `ICT-9` | Unique key. |
| `name_en` | Text | `Information and Communication Technology` | The course name in English. |
| `name_ar` | Text | `تقنية المعلومات والاتصالات` | The course name in Arabic. |
| `grade_level` | Integer | `9` | The school grade, 8 or 9. |
| `teacher_id` | Text | `T-02` | The course's teacher. Joins to `users.csv`. Only this teacher manages the course's quizzes (`TEACHER_OWNS_COURSE`). |
| `language` | `ar` or `en` | `en` | The language of instruction. One course, `ICT-9`, has questions in both languages. |

## users.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `user_id` | Text | `S-017` | Unique key: `T-01` to `T-04` for teachers and `S-001` to `S-040` for students. |
| `role` | `TEACHER` or `STUDENT` | `STUDENT` | Teachers manage quizzes. Students take them. |
| `display_name_en` | Text | `Layla` | The name to show in the English UI. |
| `display_name_ar` | Text | `ليلى` | The name to show in the Arabic UI. |
| `preferred_language` | `ar` or `en` | `ar` | The UI language to start in. Users can switch at any time. |

Display names aren't unique: two students share a first name, so key everything on `user_id`.

## enrollments.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `student_id` | Text | `S-017` | Joins to `users.csv`. |
| `course_id` | Text | `ICT-9` | Joins to `courses.csv`. See [Things to know](#things-to-know). |

Grade 8 students take the four grade 8 courses and grade 9 students take the two grade 9 courses, so each student is in 2 or 4 courses. A student who isn't enrolled in a course can't take its quizzes (`ENROLLED_STUDENT`).

## question_bank.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `question_id` | Text | `Q-0019` | Identifies the question. See [Things to know](#things-to-know). |
| `course_id` | Text | `ICT-9` | The course whose question bank the question belongs to. Joins to `courses.csv`. |
| `language` | `ar` or `en` | `en` | The language the question is written in. A quiz uses one language (`QUESTION_LANGUAGE`). |
| `type` | `MCQ` or `TRUE_FALSE` | `MCQ` | Multiple choice with four options, or true/false with two. |
| `question_text` | Text | `What does CPU stand for?` | As the teacher wrote it. |
| `option_a` to `option_d` | Text | `Central Processing Unit` | The answer options. `TRUE_FALSE` questions use only `option_a` and `option_b`. |
| `correct_option` | `A`, `B`, `C` or `D` | `A` | The letter of the correct option. |
| `points` | Integer, 1 to 3 | `1` | Points for a correct answer. |
| `topic` | Text | `Hardware` | Written in the question's language, for example `الجبر` (algebra) for an Arabic maths question. |
| `author_id` | Text | `T-02` | The teacher who wrote the question. Joins to `users.csv`. It isn't always the course's teacher, and writing a question doesn't let a teacher manage that course's quizzes. |

## Things to know

Real question banks and school records are messy, so these are too. The question bank and the enrollments deliberately contain a small number of:

- **Duplicate question IDs:** the same `question_id` appears on more than one row, with different content.
- **Invalid questions:** a question's `correct_option` points to an empty option, and a question has no `points` value.
- **Inconsistent codes:** a `course_id` has trailing spaces, and a `language` code is upper case (`EN`).

These issues can change which questions teachers can use and which students can take a quiz. Decide how your app handles each one, record the decision in your plan, and test it.

Also note:

- **True/false questions:** `option_a` and `option_b` are `True` and `False` in English, or `صح` and `خطأ` in Arabic. `option_c` and `option_d` are empty.
- **Digits and direction:** Arabic questions use both Arabic-Indic digits (٠١٢٣٤٥٦٧٨٩) and Western digits (0-9), as the teachers typed them. Many mix Arabic with English terms, formulas and numbers, such as `(CPU)` or `2x - 5 = 11`, so check that mixed-direction text displays correctly in both UI languages.
- **Excel:** the files are UTF-8. Open them with **Data** > **From Text/CSV** and choose UTF-8 encoding (**65001: Unicode (UTF-8)**), or the Arabic text looks garbled.

The school is in Kuwait: use Kuwait time (UTC+03:00, no daylight saving time) for quiz open and close times. The school week runs from Sunday to Thursday.
