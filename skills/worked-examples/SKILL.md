---
name: worked-examples
description: >
  Generate step-by-step worked examples for procedural or algorithmic course
  material, then provide a fresh problem for independent practice. Use this skill
  when the user asks to "walk through an example," "show me how to do this,"
  "practice problem," "work through a [specific algorithm/procedure]," "I need to
  practice [topic]," "can you give me a problem to try," or any request for hands-on
  practice with a method, algorithm, derivation, or computation from their course.
  Also trigger when the user says things like "I don't get how to actually do this"
  or "I understand the concept but can't do the problems." This skill is for
  procedural practice — for conceptual understanding questions, use the concept-check
  skill instead.
---

# Worked Example Skill

## Purpose

Help learners develop procedural fluency by providing a carefully annotated
worked example followed by an independent practice problem. This targets the gap
between "I understand the concept" and "I can actually do it" — the gap that
costs people points on exams.

## Philosophy

Worked examples are most effective when they follow the "I do → you do" pattern:
first the learner watches every step performed with explicit reasoning, then they
attempt a similar (but not identical) problem on their own. The key is that the
worked example doesn't just show *what* to do at each step — it explains *why*
that step is being taken and *how* to decide what to do next.

The independent practice problem should test the same procedure but differ enough
that the learner can't just copy-paste with different numbers. Changing the
structure slightly (a longer sentence, an extra edge case, a different rule set)
forces them to engage with the procedure rather than pattern-matching.

## Workflow

### Step 1: Identify the procedure

Determine what specific procedure/algorithm/method the learner wants to practice.
Use `project_knowledge_search` or conversation history to find:
- The exact algorithm or method from the course slides
- Any notation, conventions, or specific rule sets used in the course
- The level of complexity appropriate for the learner's current stage

Common examples across courses: filling in a dynamic programming table, computing
probabilities in a model, applying an algorithm step-by-step, performing a
derivation, tracing through a data structure, executing a training update.

### Step 2: Create the worked example

Design a **small but non-trivial** example. It should be:
- Small enough to work through completely (don't use 20-word sentences for a
  parsing example — use 4–6 words)
- Complex enough to exercise the interesting parts of the procedure (include at
  least one spot where a decision has to be made, like choosing between two
  possible parses)
- Different from the examples used in the slides (but using the same notation
  and conventions)

Walk through the solution with these principles:

**Show your reasoning at decision points.** Don't just write "next we fill in
cell [2][4] with NP." Say: "To fill cell [2][4], we look at all ways to split
the span 'ate the fish' into two parts. Split at k=2 gives us cell[2][2] and
cell[3][4]... Cell[2][2] has V and cell[3][4] has NP. Is there a rule X → V NP?
Yes: VP → V NP. So we add VP to cell[2][4]."

**Number every step.** This makes it easy for the learner to ask "I got lost at
step 4" and for you to reference specific steps later.

**Flag common mistakes.** At points where learners typically go wrong, add a
brief note: "Common mistake here: don't forget to check *all* possible splits,
not just the first one that works."

**Use consistent visual formatting.** For table-filling algorithms, show the
table state after key steps. For derivations, show each line clearly. For tree
construction, use indentation or bracketed notation consistently.

**End with a verification.** If possible, show how to check the answer (e.g.,
"we can verify by multiplying all rule probabilities: 0.8 × 0.3 × ... = 0.0384,
which matches").

### Step 3: Present the practice problem

Immediately after the worked example, present a new problem that:
- Uses the **same procedure** but with **different input**
- Is **slightly more complex** than the worked example (e.g., one more word in
  the sentence, one additional rule in the grammar, one more variable in the
  derivation)
- Includes a **specific edge case or twist** that the worked example didn't have
  (e.g., an ambiguous case where two parses are possible, a step where the
  obvious approach fails)

Frame it as: "Your turn. Try this one, and show me your work at whatever level
of detail you're comfortable with. I'll check each step."

### Step 4: Coach through their attempt

When the learner submits their work:

**If they're correct:** Confirm it, point out anything they did especially well,
and note if there's a shortcut or elegance they might have missed.

**If they made a procedural error:** Don't just show the right answer. Point to
the exact step where they diverged: "Your work is right through step 3. At step
4, check what's in cell[2][3] — I think you may have missed one of the entries."
Let them try to fix it before revealing the correction.

**If they're stuck and can't start:** Break the problem into a smaller first
step: "Let's just start with the initialization. What goes in the diagonal cells
of the chart?" Build momentum from there.

**If they ask for hints:** Give the *minimum viable hint* — the smallest nudge
that unblocks them. "What rule has VP on the left-hand side and could match
what's in cells [2][2] and [3][4]?" is better than "You need to use VP → V NP."

### Step 5: Offer continuation

After the practice problem is resolved, offer:
- Another problem at the same difficulty (for reinforcement)
- A harder problem (if they nailed it and want a challenge)
- Moving on to the next milestone (if they feel confident)

## Problem design guidelines

- **Scale to the procedure.** A CKY chart-filling problem should use 4–6 words
  and 8–12 grammar rules. A probability computation should use a tree with 4–6
  nodes. Don't make problems so large that the bookkeeping overwhelms the
  learning.
- **Include enough rules/data to be realistic but not exhausting.** For grammar
  problems, include some rules that are distractors (won't be used) so the
  learner has to make choices.
- **Anticipate the hard parts.** If the algorithm has a step that's conceptually
  tricky (like considering all split points in CKY), design the problem so that
  step is exercised meaningfully.
- **Use the course's notation.** Match whatever conventions the slides use for
  nonterminals, probability notation, table layout, etc. Don't introduce your
  own notation — that creates unnecessary translation overhead.
