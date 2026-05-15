---
name: study-session
description: >
  Orchestrate a full study session by chaining the right skills in the right order
  based on where the learner is in their weekly study cycle. Use this skill when the
  user says "study session," "help me study this week," "full study workflow," "walk
  me through everything," "I have a few hours to study," "plan my study session,"
  "what should I do next," "where did I leave off," or any request that implies they
  want a guided, multi-step study process rather than a single specific task. Also
  trigger when the user seems unsure what to do next ("I finished watching the
  lectures, now what?") or wants to prepare comprehensively for an exam ("midterm is
  next week, help me get ready"). This skill orchestrates other skills — it does not
  teach content or generate artifacts directly.
---

# Study Session Orchestrator

## Purpose

Guide the learner through a complete study cycle by invoking the right skills in
the right order, based on where they currently are in their learning process. This
is the "coach" that sits above all the other skills and makes sure nothing falls
through the cracks.

## Philosophy

Most learners, when left to their own devices, do one of two things: they either
passively re-read slides (low retention) or jump straight to practice problems
without building foundations (high frustration). A good study workflow alternates
between input (learning new material), processing (testing and connecting), and
output (producing artifacts for later review). This skill enforces that rhythm.

The key insight from the open-exam-skills repo's workflow packs is that chaining
skills creates more value than any single skill alone. But their chains are static
(always: mindmap → flashcards → quiz). Ours should be *adaptive* — the sequence
depends on what the learner has already done, what they struggled with, and what's
coming up.

## The Study Cycle

A complete weekly study cycle has these phases. The learner may enter at any phase.

```
Phase 1: PREVIEW
  └─ lecture-milestones → Break lectures into thematic milestones

Phase 2: LEARN (repeat for each milestone)
  ├─ Watch/read the milestone material
  ├─ concept-check → Test understanding of this milestone
  └─ worked-examples → Practice procedures (if milestone is algorithmic)

Phase 3: CONNECT
  └─ connection-mapper → Map this week to prior and upcoming material

Phase 4: CONSOLIDATE
  ├─ flashcard-deck → Generate cards for spaced repetition
  └─ practice-quiz → Self-grading test across all milestones

Phase 5: REVIEW
  └─ Revisit weak areas identified by concept-check and practice-quiz
      (re-enter Phase 2 for specific milestones)
```

If an assigned paper is involved, `paper-reader` can slot in anywhere — typically
before Phase 2 or as a parallel track.

## Workflow

### Step 1: Assess where the learner is

When triggered, figure out the learner's current state by checking:

1. **Conversation history**: Have milestones already been generated in this chat
   or a recent one? Have they completed any concept-checks?
2. **What they say**: "I just finished watching" → Phase 2 checkpoint. "I haven't
   started yet" → Phase 1. "Midterm next week" → Phase 4.
3. **What's in project knowledge**: Are lecture slides uploaded? Are there papers?
   How many weeks of material are available?

Based on this assessment, determine which phase they're in and what the next
action should be.

### Step 2: Recommend the next action

Present the learner with a clear, specific next step — not a menu of everything
they could do. The recommendation should include:

1. **What to do next** (one specific action)
2. **Why this is the right next step** (one sentence connecting it to where they
   are in the cycle)
3. **How long it will take** (rough estimate)
4. **What comes after** (so they can see the path ahead)

**Example recommendations by phase:**

**Phase 1 (hasn't started):**
"Let's start by breaking this week's lectures into study milestones so you have
a roadmap. This takes about 5 minutes, and then you'll know exactly what to
focus on when you watch. Want me to generate milestones now?"

**Phase 2 (finished a milestone):**
"You just finished the CKY Algorithm milestone — that's the hardest one this
week. Let me run a concept check to see how well it landed. This takes about
10 minutes. After that, I'd suggest a worked example where you fill in a chart
by hand."

**Phase 3 (finished all milestones):**
"You've worked through all the milestones for this week. Before we move to
drilling, let me map how this week's parsing material connects back to the
HMM and Viterbi content from a few weeks ago. Takes about 5 minutes, and it'll
make the PCFG material click better."

**Phase 4 (ready to consolidate):**
"You've got a solid understanding of the material. Two things to do now:
I'll generate a flashcard deck for spaced repetition (you can review these
daily until the exam), and then a practice quiz so you can test yourself under
exam-like conditions. Want to start with flashcards or the quiz?"

**Phase 5 (post-quiz review):**
"Your practice quiz score was 13/20. You were solid on CFG fundamentals and
CKY, but the PCFG probability computations and PARSEVAL evaluation tripped
you up. I'd suggest we go back to those specific topics — I can run a focused
concept-check on just PCFG tree probabilities, or we can do a worked example
computing P(τ) step by step. Which would help more?"

### Step 3: Execute the recommended skill

Once the learner agrees, invoke the appropriate skill. You don't need to
re-explain what each skill does — just execute it. The orchestrator's job is
*sequencing and context-passing*, not teaching.

**Context to pass between skills:**
- When invoking concept-check after milestones: reference the specific milestone
  checkpoints ("test whether they can fill in a CKY chart and explain CNF")
- When invoking worked-examples: reference what concept-check revealed as weak
  spots
- When invoking connection-mapper: reference all milestones completed this week
  plus any prior lecture material in project knowledge
- When invoking flashcard-deck or practice-quiz: reference the full week's
  milestones and any concept-check gaps

### Step 4: After each skill completes, loop back

After each skill finishes, briefly summarize what was accomplished and recommend
the next step. Keep the momentum going — don't wait for the learner to ask
"what's next?" Proactively suggest it.

**Transition examples:**
- After concept-check: "Nice work — you're solid on 4 out of 5 checkpoints.
  The one gap was [X]. Want to do a worked example on that, or move on to the
  connection map?"
- After worked-examples: "You nailed that second practice problem. I think
  you're ready to move on. Next up: connecting this week to prior material,
  then building your flashcard deck."
- After flashcard-deck: "Your 25-card deck is ready for daily review. Last
  step: a practice quiz covering everything. Ready?"

## Handling interruptions and shortcuts

Not every learner wants the full cycle every time. Handle common shortcuts:

- **"Just quiz me"** → Skip to Phase 4, invoke practice-quiz directly. But note
  what they're skipping: "Sure, jumping straight to the quiz. If you want to
  build up to it with concept checks first next time, just say 'study session'
  and we'll do the full flow."
- **"I only have 30 minutes"** → Prioritize. If they haven't started: milestones
  + concept-check for the first milestone. If they're mid-cycle: the next
  milestone's concept-check + one worked example. If they're near the end:
  practice quiz.
- **"I'm cramming for tomorrow"** → Flashcard deck + practice quiz. Skip the
  exploratory phases. Note: "For cramming, flashcards and a practice quiz are
  your best bet. If you have time after the quiz, we can revisit anything you
  got wrong."

## Handling pasted quiz results (Phase 5 review)

When the learner pastes practice quiz results into the chat (the structured
output from the "Copy Results for Review" button), this is the trigger for
Phase 5 — targeted remediation. This is where the most learning happens.

### How to handle pasted results

**Step 1: Parse and prioritize.**
Read the pasted results. Identify which questions were wrong and group them
by topic tag. Prioritize topics where multiple questions were missed — a
single miss might be a careless error, but two or more misses on the same
topic signal a real gap.

**Step 2: Diagnose the misconception type.**
For each wrong answer, look at what the learner *chose* vs. the correct answer.
This reveals the nature of the misunderstanding:
- Chose a plausible-but-wrong option → likely a conceptual confusion between
  two similar ideas
- Chose an obviously wrong option → likely didn't understand the topic at all
- Pattern of wrong answers in one topic → systematic gap, not a fluke

**Step 3: Design the review, not as re-quizzing but as teaching.**
Do NOT re-ask the same questions as multiple choice. Instead, for each weak
topic area, choose the most effective remediation approach:

- **Conceptual confusion** (mixed up two similar ideas) → Ask the learner to
  explain the difference between the two concepts in their own words. Correct
  their explanation where it diverges. This is the Feynman technique — teaching
  back reveals exactly where the mental model breaks.

- **Procedural error** (got a computation wrong) → Invoke the worked-examples
  pattern. Walk through a fresh example step by step, then give them one to try.

- **Fundamental gap** (didn't understand the topic) → Give a brief, focused
  re-explanation of the concept, anchored to the course slides. Then ask one
  or two targeted concept-check questions to verify it landed.

**Step 4: Confirm understanding before moving on.**
After working through each weak topic, ask one final check question. If they
get it, mark that topic as resolved. If not, try a different angle of
explanation.

**Step 5: Summarize the review session.**
At the end, tell the learner:
- Which topics are now solid
- Whether any gaps remain that need more time
- Suggest adding the weak topics to their flashcard deck if they haven't already
- Offer to generate a focused "retry quiz" covering just the problem areas
  (a shorter 5–8 question quiz on the weak topics only)

### Example flow

```
User pastes:
  QUIZ RESULTS: Lectures 15-16 Syntax & Parsing
  SCORE: 13/20 (65%)
  WRONG:
  - Q3: What is P(τ) for this tree? | Chose: 0.024 | Correct: 0.00024 | Topic: PCFG probability
  - Q8: Which rule violates CNF? | Chose: X→YZ | Correct: X→YZW | Topic: CNF
  - Q11: What does PARSEVAL recall measure? | Chose: predicted/correct | Correct: correct/predicted... | Topic: evaluation
  ...

Claude responds:
  "You scored 13/20 — solid on the fundamentals but three areas need
  attention: PCFG probability computation, CNF rules, and PARSEVAL
  metrics. Let's work through them.

  The PCFG probability question is the most important since it shows
  up everywhere from here on. I notice you got 0.024 instead of
  0.00024 — that's off by a factor of 100, which usually means a
  rule probability got skipped in the multiplication. Let me walk
  you through a fresh example step by step..."
```

## Important guidelines

- **Be a coach, not a menu.** Don't list all 7 skills and ask which one they
  want. Recommend one specific action based on where they are. The learner
  should feel guided, not overwhelmed by choices.
- **Track progress conversationally.** You don't have persistent state between
  sessions, but within a session you can reference what's been done: "You've
  completed milestones 1–3 and the concept check for milestone 2. Next up
  is the concept check for milestone 3."
- **Don't force the full cycle.** If someone just wants flashcards, make
  flashcards. The workflow is a recommendation, not a requirement.
- **Time estimates matter.** Grad students are busy. Saying "this will take
  about 10 minutes" helps them decide whether to continue or stop for now.
- **Celebrate progress.** Brief, genuine acknowledgment of what they've
  accomplished keeps motivation up across a long session. "That's 3 milestones
  down, 2 to go — you're past the hardest part" is more motivating than just
  moving to the next task.
