---
name: practice-quiz
description: >
  Generate a self-grading interactive practice quiz as a downloadable HTML file
  from course material. Use this skill when the user asks to "make a practice
  exam," "generate a quiz file," "create a test I can take," "mock exam,"
  "practice test," "self-grading quiz," "make me a quiz I can download," or any
  request for a standalone, scoreable quiz they can take independently. Also
  trigger when the user says "I want to simulate an exam," "give me a timed
  test," or "make something I can take before the midterm." This skill produces
  a downloadable HTML file with scoring — it does not run an interactive Q&A in
  chat (use concept-check for conversational quizzing).
---

# Practice Quiz Skill

## Purpose

Generate a polished, self-grading HTML quiz file that simulates exam conditions.
The learner downloads it, takes it in their browser, gets scored, reviews their
mistakes, and can retake it. Unlike concept-check (which is a back-and-forth
dialogue), this produces a **standalone test** — closer to what an actual exam
feels like.

## Why this complements concept-check

Concept-check is formative: it happens *during* studying, with immediate coaching
and follow-up. Practice quizzes are summative: they happen *after* studying, to
measure readiness. Concept-check adapts in real time based on your answers. A
practice quiz presents a fixed set of questions and scores you at the end — no
hints, no do-overs until review mode. Different purposes, different psychology.

## Workflow

### Step 1: Determine scope and difficulty

Use `project_knowledge_search` and conversation context to identify:
- What lectures/milestones/topics the quiz should cover
- How many questions the user wants (default: 15–20)
- What difficulty mix is appropriate (if they're prepping for a midterm, lean
  harder; if they just finished a milestone, keep it moderate)

### Step 2: Generate the questions

Create a mix of question types. Every quiz should include at least three of
these five types:

**Type 1: Conceptual multiple choice (40–50% of questions)**
Tests whether the learner understands *what* and *why*.

```json
{
  "question": "Why does the CKY algorithm require the grammar to be in Chomsky Normal Form?",
  "options": [
    "So that all rules produce exactly one terminal symbol",
    "So that every rule has exactly two symbols on the right-hand side, enabling binary splits of spans",
    "So that the grammar has no recursive rules",
    "So that every nonterminal appears on the left-hand side of exactly one rule"
  ],
  "correctIndex": 1,
  "hint": "Think about how CKY fills chart cells by combining two sub-spans.",
  "explanation": "CKY considers all ways to split a span into two sub-spans. CNF guarantees every rule is X → Y Z (binary) or X → w (terminal), so only binary splits need to be checked. Without CNF, rules could have 3+ symbols on the right-hand side, and CKY's O(n³) dynamic programming structure wouldn't work."
}
```

**Type 2: Procedural/computation (20–30%)**
Tests whether the learner can *do* the procedure.

```json
{
  "question": "Given a PCFG with P(S → NP VP) = 0.8, P(NP → Noun) = 0.2, P(VP → V NP) = 0.3, P(Noun → 'John') = 0.1, P(V → 'eats') = 0.5, P(Noun → 'sushi') = 0.1, what is P(τ) for the tree [S [NP [Noun John]] [VP [V eats] [NP [Noun sushi]]]]?",
  "options": [
    "0.0024",
    "0.00024",
    "0.024",
    "0.24"
  ],
  "correctIndex": 1,
  "hint": "Multiply all rule probabilities: P(S→NP VP) × P(NP→Noun) × P(Noun→John) × P(VP→V NP) × P(V→eats) × P(NP→Noun) × P(Noun→sushi)",
  "explanation": "P(τ) = 0.8 × 0.2 × 0.1 × 0.3 × 0.5 × 0.2 × 0.1 = 0.00024. Each rule used in the tree contributes its probability, and we multiply them all together."
}
```

**Type 3: Distinction/comparison (10–15%)**
Tests whether the learner can tell similar concepts apart.

**Type 4: Error identification (5–10%)**
Presents a statement or worked example with an error; the learner picks which
option identifies the mistake.

**Type 5: Application to new scenario (5–10%)**
Presents a novel situation and asks the learner to apply a concept.

**Question quality rules:**
- Four answer options per question (A/B/C/D) plus a fifth option: **"I don't
  know — needs review."** This option is always present on every question and
  always appears last (as option E). Selecting it counts the question as
  "needs review" in the results — it is never counted as correct and is
  explicitly included in the WRONG/REVIEW section of the results export.
  This prevents false positives from lucky guesses and gives the learner
  permission to be honest about gaps without penalty.
- Distractors should be plausible — they should reflect common misconceptions,
  not absurd options
- Every question must have an explanation that teaches, not just says "the
  answer is B"
- Hints should nudge toward the right reasoning without giving the answer away
- Use the course's actual notation, terminology, and examples
- Difficulty should be distributed: ~30% straightforward, ~50% moderate, ~20% challenging
- Questions should cover the full scope of the requested material, not cluster
  on one subtopic
- **Tag every question with a short topic label** during generation (e.g.,
  "CKY chart-filling," "CNF conversion," "PCFG tree probability," "PARSEVAL
  evaluation"). These tags are embedded in the HTML data and appear in the
  results export so Claude can identify which topic areas need review.

### Step 3: Build the HTML artifact

Create a self-contained HTML file (`.html` or `.jsx` React artifact) with
these features:

**Quiz flow:**
1. **Start screen**: Shows quiz title, number of questions, estimated time,
   and a "Begin Quiz" button
2. **Question view**: One question at a time, four clickable options (A/B/C/D),
   optional "Show Hint" button, Previous/Next navigation
3. **Answer feedback**: After selecting an answer, immediately show whether
   it's correct or wrong, display the explanation, highlight the correct answer
   in green (wrong selection in red)
4. **Completion screen**: Score (e.g., "14/20 — 70%"), breakdown by correct/
   wrong/skipped, and buttons for "Review Answers," "Retake Quiz," and
   **"Copy Results for Review"**
5. **Review mode**: Navigate through all questions seeing your answers,
   the correct answers, and all explanations

**Results export (critical feature):**
The "Copy Results for Review" button on the completion screen copies a
structured summary to the clipboard that the learner can paste back into
Claude for targeted remediation. The format must be exactly:

```
QUIZ RESULTS: [Quiz Title]
SCORE: [correct]/[total] ([percentage]%)
TOPICS: [comma-separated list of topics covered]

WRONG:
- Q[N]: [full question text] | Chose: [their answer] | Correct: [right answer] | Topic: [topic tag]

NEEDS REVIEW (chose "I don't know"):
- Q[N]: [full question text] | Correct: [right answer] | Topic: [topic tag]

CORRECT:
- Q[N]: [short question summary] | Topic: [topic tag]
```

This format gives Claude everything it needs to run a targeted review:
which questions were missed, what the learner chose (revealing their
misconception), which questions the learner flagged as unknown (revealing
honest gaps), what the right answer was, and what topic area each
question belongs to. The "NEEDS REVIEW" section is especially valuable —
it tells Claude exactly where the learner knows they don't know, which
is often more actionable than wrong guesses. The topic tags should be
short labels assigned during question generation (e.g., "CKY
chart-filling," "PCFG probability," "PARSEVAL evaluation").

The button should show brief confirmation text ("Copied! Paste into Claude
for a targeted review session.") after clicking.

**UI requirements:**
- Progress bar showing completion (e.g., "Question 7 of 20")
- Clean, exam-like aesthetic (white background, readable fonts, no distractions)
- Keyboard support: number keys 1–4 to select options, Enter to confirm,
  arrow keys to navigate
- Mobile-responsive layout
- All CSS/JS embedded — completely self-contained, no external dependencies
- Use in-memory state only — no localStorage or sessionStorage

**Design notes:**
- Use a calm, focused color scheme (not the purple gradient from NotebookLM —
  go for something that feels more like an actual exam interface)
- Correct answers: green accent. Wrong answers: red accent. Neutral: blue/gray.
- Large, readable question text. Generous spacing between options.
- Explanations should be visually distinct from the question (e.g., in a
  light-bordered box below the options)

### Step 4: Present the file

Save to `/mnt/user-data/outputs/` and present to the user. Include:
- Number of questions and topics covered
- Estimated time to complete (roughly 1–1.5 minutes per question)
- A note that they can retake it and review answers after

## Question generation from course material

**Always search project knowledge first.** The questions should be grounded in
what the slides actually teach, using the same notation, examples, and emphasis.
If the slides spend three parts on CKY and one part on evaluation, the quiz
should weight accordingly.

**Cross-reference milestones if available.** If the learner has already generated
milestones for these lectures, align quiz coverage to the milestone checkpoints.
Each milestone's "what you should be able to do" items are natural question seeds.

**Avoid trivial recall.** "What does CFG stand for?" is not a useful exam
question. "Which of these properties distinguishes a CFG from a regular grammar?"
is. Match the depth of what a grad school exam would actually ask.

## Important guidelines

- **This is a file, not a conversation.** Don't ask follow-up questions or
  offer to discuss answers in chat. The quiz is self-contained. If the learner
  wants to discuss specific questions afterward, they can ask — but the quiz
  itself should work without Claude.
- **Include explanations for every question, both correct and incorrect paths.**
  The review mode is where most of the learning happens.
- **Default to 15–20 questions** unless the user specifies otherwise. Under 10
  feels trivial; over 25 causes fatigue.
- **Test the full breadth of the material.** Don't let 60% of questions be about
  the first topic just because it's freshest in context.
- **LaTeX for math.** If questions involve formulas, use proper notation. Include
  KaTeX inline in the HTML if math is present.
