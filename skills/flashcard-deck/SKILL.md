---
name: flashcard-deck
description: >
  Generate an interactive, exportable flashcard deck from course material. Use this
  skill when the user asks to "make flashcards," "create a deck," "Anki cards,"
  "flashcard set," "drill cards," "make me cards for this topic," "export flashcards,"
  or any request to turn lecture material into a reviewable card deck. Also trigger
  when the user says "I need to memorize this," "help me drill [topic]," or "make
  something I can review on my phone." This skill produces a downloadable interactive
  HTML file (and optional CSV for Anki import) — it does not quiz the user in chat
  (use concept-check for that) and does not create study milestones (use
  lecture-milestones).
---

# Flashcard Deck Skill

## Purpose

Turn course material into a polished, self-contained HTML flashcard deck that the
learner can study offline, share, or import into Anki/Quizlet. Unlike our
concept-check skill (which runs an interactive Q&A in chat), this skill produces
a **file** — a standalone artifact the learner takes with them.

## Why this complements concept-check

Concept-check is a *dialogue*: Claude asks questions, the learner responds, Claude
gives feedback. It's great for testing understanding in the moment. Flashcard decks
are a *product*: the learner studies them independently, repeatedly, across days.
They support spaced repetition — the single most evidence-backed technique for
long-term retention. Different tools for different stages of learning.

## Workflow

### Step 1: Identify the material

Use `project_knowledge_search` to find the relevant lecture content. Determine:
- What topics/milestones the flashcards should cover
- What key terms, definitions, algorithms, and distinctions the material contains
- What level of detail is appropriate (vocabulary-level recall vs. procedural steps)

### Step 2: Generate the card content

Create 15–30 flashcards (unless the user requests a specific count). Each card
has a **front** (prompt) and **back** (answer).

**Card types to include:**

- **Term → Definition** (30–40% of cards): Core vocabulary and concepts.
  Front: "What is Chomsky Normal Form?"
  Back: "A restricted form of CFG where every rule is either X → Y Z (two
  nonterminals) or X → w (one terminal). Required for the CKY algorithm."

- **Process → Steps** (20–30%): Algorithms and procedures.
  Front: "What are the three phases of the CKY algorithm?"
  Back: "1. Create the n×n chart. 2. Initialize diagonal cells with terminal
  rules. 3. Fill cells bottom-up, checking all binary splits for matching rules."

- **Distinction → Explanation** (15–20%): Contrasts and comparisons.
  Front: "What's the difference between arguments and adjuncts in a CFG?"
  Back: "Arguments are obligatory (introduced by rules where head ≠ parent, e.g.,
  VP → V NP). Adjuncts are optional and recursive (head = parent, e.g., VP → VP PP)."

- **Application → Answer** (15–20%): "Given X, what is Y?" cards.
  Front: "Given the rule VP → V NP with P(rule) = 0.3, and the subtree
  probabilities P(V) = 1.0 and P(NP) = 0.2, what is P(VP)?"
  Back: "P(VP) = P(rule) × P(V) × P(NP) = 0.3 × 1.0 × 0.2 = 0.06"

**Card quality rules:**
- One concept per card. Never put two unrelated facts on one card.
- Keep fronts short and unambiguous. The learner should know exactly what's
  being asked.
- Keep backs concise but complete. 1–3 sentences or a short list. If the
  answer needs a paragraph, split into multiple cards.
- Use the course's notation and terminology, not your own.
- Include a mix of easy recall and harder application cards.
- For math/formulas, use LaTeX notation: `$P(\tau) = \prod P(r_i)$`

### Step 3: Build the HTML artifact

Create a self-contained HTML file as a React artifact (`.jsx`) or plain HTML
(`.html`) with these features:

**Core functionality:**
- Card display with front/back flip animation (click or spacebar to flip)
- Left/right arrow navigation (keyboard arrows or click buttons)
- Progress indicator ("Card 7 / 25")
- Shuffle button to randomize card order

**Study features:**
- "Know it" / "Still learning" buttons that sort cards into two piles
- A counter showing how many cards are in each pile
- Option to study only the "still learning" pile
- Reset button to start over

**Export:**
- A "Download CSV" button that exports all cards in `front,back` format
  (compatible with Anki, Quizlet, and most flashcard apps)

**Design:**
- Clean, readable typography (large text, high contrast)
- Cards should be centered with generous padding
- Subtle flip animation (CSS 3D transform)
- Mobile-friendly (responsive layout, touch support)
- Use CSS variables for theming (works in light/dark mode)

**Important implementation notes:**
- The HTML file must be completely self-contained (no external dependencies)
- All CSS and JS embedded in the single file
- Use in-memory state (React useState or plain JS variables) — no localStorage
- LaTeX rendering: include KaTeX CSS/JS inline if any cards use math notation,
  or render math as Unicode approximations for simpler formulas

### Step 4: Present the file

Save the HTML file to `/mnt/user-data/outputs/` and present it to the user.
Include a brief summary: how many cards, what topics they cover, and a reminder
about the CSV export for Anki import.

## Output format

The primary output is the HTML file. In the chat message, provide:

```
Here's your flashcard deck: [filename]

**[N] cards** covering [brief topic list].

How to use:
- Click or press Space to flip cards
- Use ← → arrows to navigate
- Mark cards as "Know it" or "Still learning" to focus your review
- Click "Download CSV" to import into Anki or Quizlet

[Any notes about card content, e.g., "I included 5 cards on CKY chart-filling
since that's the most procedural part of this milestone."]
```

## Important guidelines

- **Generate cards from the actual course material**, not general knowledge.
  Search project knowledge first. Use the terminology and examples from the
  slides.
- **Don't duplicate concept-check.** Flashcards are for recall and drilling.
  Concept-check handles deeper comprehension questions. If a concept needs
  explanation and feedback, that's a concept-check question, not a flashcard.
- **Err toward more cards at lower complexity** rather than fewer cards that
  each try to pack in too much. Flashcards work through repetition, not
  density.
- **Always include the CSV export.** Many learners have existing Anki workflows
  and will want to import the cards rather than use the HTML directly.
- **If the user asks for flashcards on material you can't find in project
  knowledge,** tell them honestly and offer to make cards based on what you
  can find, or ask them to point you to the right material.
