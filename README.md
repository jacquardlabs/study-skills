# Study Skills for Claude Projects

8 composable Claude skills that turn lecture slides into a full study workflow: milestones, concept checks, worked examples, flashcard decks, practice quizzes, and more. Designed for graduate coursework, adaptable to any subject.

## What are skills?

Skills are markdown instruction files (`SKILL.md`) that you add to a [Claude Project](https://claude.ai). They teach Claude how to perform a specific task consistently, like giving it a detailed playbook instead of a vague prompt. Each skill lives in its own folder and can be used independently or chained together.

## The skills

| Skill | What it does | Produces |
|---|---|---|
| **lecture-milestones** | Breaks lecture slide decks into thematic study checkpoints grouped by conceptual dependency, not file boundary | Structured roadmap in chat |
| **concept-check** | Generates 5–7 targeted comprehension questions (apply, explain, connect, spot-the-error) after studying a milestone | Interactive Q&A in chat |
| **worked-examples** | Walks through a procedure step-by-step, then gives a fresh problem for independent practice | Guided practice in chat |
| **connection-mapper** | Maps how the current week's material connects to prior and upcoming topics (recurring patterns, building blocks, contrasts) | Connection map in chat |
| **flashcard-deck** | Generates an interactive HTML flashcard deck with flip animations, know-it/still-learning sorting, and CSV export for Anki | Downloadable HTML file |
| **practice-quiz** | Generates a self-grading HTML quiz with hints, explanations, and a "Copy Results for Review" button that feeds back into Claude | Downloadable HTML file |
| **paper-reader** | Produces a structured reading guide for an academic paper, tied to the course context | Reading guide in chat |
| **study-session** | Orchestrates the other skills in the right order based on where the learner is in their weekly study cycle | Coaching workflow in chat |

## Installation

Install via the Jacquard Labs marketplace:

```bash
/plugin marketplace add jacquardlabs/marketplace
/plugin install study-skills@jacquardlabs-marketplace
```

Or install this plugin directly:

```bash
/plugin marketplace add jacquardlabs/study-skills
/plugin install study-skills@study-skills
```

## The study cycle

The `study-session` skill coordinates everything into a 5-phase cycle:

```
Phase 1: PREVIEW
  └─ lecture-milestones → Break lectures into thematic milestones

Phase 2: LEARN (repeat per milestone)
  ├─ Watch/read the milestone material
  ├─ concept-check → Test understanding
  └─ worked-examples → Practice procedures (if algorithmic)

Phase 3: CONNECT
  └─ connection-mapper → Link this week to prior material

Phase 4: CONSOLIDATE
  ├─ flashcard-deck → Cards for spaced repetition
  └─ practice-quiz → Self-grading exam simulation

Phase 5: REVIEW
  └─ Paste quiz results back into Claude for targeted remediation
```

You don't have to follow the full cycle every time. "Just quiz me" skips to Phase 4. "I only have 30 minutes" gets a prioritized subset. The orchestrator adapts to what you need.

## Setup

### 1. Create a Claude Project

Go to [claude.ai](https://claude.ai) and create a new Project.

### 2. Add the skills

Upload each `SKILL.md` file as project knowledge. You can add all 8 or just the ones you want:

- For the full workflow: add all 8 skills
- For just quizzing: add `concept-check` and/or `practice-quiz`
- For just study planning: add `lecture-milestones` and `study-session`

### 3. Add your course material

Upload lecture slides (PDFs, PPTXs) and any assigned papers as project knowledge. The skills search this material to generate content grounded in your actual course.

### 4. Start studying

Open a new conversation in the project and say something like:

- "Study session" — starts the full workflow
- "Break down this week's lectures into milestones"
- "Quiz me on Lecture 15"
- "Make flashcards for the parsing material"
- "Walk me through a CKY chart-filling example"
- "How does this week connect to what we learned before?"

## Skill design principles

All 8 skills search your uploaded course material first. Questions use your course's notation, terminology, and examples, not generic textbook versions.

Each skill does one thing and passes context to the next: milestones define checkpoints → concept-check tests them → worked-examples targets gaps → flashcard-deck covers the full scope → practice-quiz measures readiness.

The quiz's "Copy Results for Review" button produces a summary you paste back into Claude. Claude diagnoses misconceptions from your wrong answers and runs targeted remediation, not re-quizzing but teaching.

The study-session orchestrator tracks what you've done and recommends the next step. It adjusts for time constraints, skipping ahead, or drilling a weak area.

## Customization

Each `SKILL.md` file is self-contained markdown. You can:

- Edit question types in `concept-check` or `practice-quiz` to match your exam format
- Change the card distribution in `flashcard-deck` (more application cards, fewer definition cards, etc.)
- Adjust milestone grouping in `lecture-milestones` for courses with different lecture structures
- Modify the study cycle phases in `study-session` to match your study habits

The skills are readable and modifiable; they're instructions, not code.

## File structure

```
study-skills-repo/
├── README.md
├── LICENSE
└── skills/
    ├── concept-check/
    │   └── SKILL.md
    ├── connection-mapper/
    │   └── SKILL.md
    ├── flashcard-deck/
    │   └── SKILL.md
    ├── lecture-milestones/
    │   └── SKILL.md
    ├── paper-reader/
    │   └── SKILL.md
    ├── practice-quiz/
    │   └── SKILL.md
    ├── study-session/
    │   └── SKILL.md
    └── worked-examples/
        └── SKILL.md
```

## License

MIT — use these however you want.
