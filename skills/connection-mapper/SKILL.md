---
name: connection-mapper
description: >
  Map conceptual connections between the current week's lecture material and the
  rest of the course — both backward to prior lectures and forward to upcoming
  topics. Use this skill when the user asks "how does this connect to what we
  learned before," "what's the big picture," "how does this fit in," "what themes
  keep coming back," "concept map," "connection map," "show me how everything links
  together," or any request to see how topics across multiple weeks relate. Also
  trigger when the user seems confused about why a topic is being taught ("why are
  we learning this?") or when they notice a similarity to earlier material and want
  to explore it ("this feels like Viterbi again"). This skill maps connections — it
  does not teach the material itself or create study milestones (use
  lecture-milestones for that).
---

# Connection Mapper Skill

## Purpose

Make the cumulative structure of a course visible by explicitly mapping how this
week's concepts connect to prior and upcoming material. Graduate courses build
ideas on top of each other, but lectures rarely pause to draw the full map.
This skill fills that gap.

## Why this matters

One of the biggest differences between students who struggle and students who
excel in grad courses is whether they see the material as a sequence of isolated
weekly topics or as a web of interconnected ideas. When you recognize that "oh,
Viterbi CKY is the same dynamic programming pattern I saw in HMM decoding, just
applied to trees instead of sequences," you're not learning a new algorithm from
scratch — you're extending a pattern you already own. Connection mapping makes
these links explicit so the learner builds a denser, more durable knowledge
structure.

## Workflow

### Step 1: Identify the scope

Determine what the learner wants connected:
- **Current week's material**: Use `project_knowledge_search` to find the
  specific lectures/topics they're working on right now
- **Prior material in the project**: Search for earlier lectures to find what
  concepts have already been covered
- **Specific connection request**: If they're asking about a particular link
  ("how is this like Viterbi?"), focus there rather than doing a full map

### Step 2: Find the connections

Search through available lecture materials (both current and prior weeks) and
identify connections in these categories:

**Recurring patterns** — The same technique or framework appearing in a new
context. These are the highest-value connections because they reduce cognitive
load ("you already know how to do this").
- Same algorithm, different domain (Viterbi in HMMs → Viterbi in PCFGs)
- Same math, different application (MLE counting in n-grams → MLE counting
  in PCFGs)
- Same evaluation framework (precision/recall in classification → precision/
  recall in parsing)

**Building blocks** — A concept from earlier that is now a component of
something larger. These show *why* the earlier material was taught.
- POS tags become the terminal/preterminal layer of parse trees
- N-gram probability estimation becomes the basis for rule probability
  estimation in PCFGs
- Finite-state automata motivate the need for context-free grammars

**Contrasts** — Cases where the current material explicitly departs from or
improves on something earlier. These clarify *what's new*.
- CFGs handle hierarchical structure that FSAs cannot
- PCFGs add disambiguation that plain CFGs lack
- Discriminative models vs. generative models for the same task

**Foreshadowing** — If you can identify upcoming topics (from the project's
lecture files, a syllabus, or the current slides mentioning future topics),
note what the current material sets up.

### Step 3: Present the map

Organize the connections into a clear, scannable format. For each connection:

1. **Name the link concisely** — a short phrase that captures the relationship
2. **Identify the two endpoints** — what from this week connects to what from
   when
3. **Explain the connection in 1–2 sentences** — not just "these are related"
   but *how* they're related and *why* it matters for the learner's understanding
4. **Give a concrete anchor** — a specific example, formula, or algorithm step
   that makes the connection tangible rather than abstract

### Step 4: Highlight the highest-value connections

After presenting all connections, call out the 2–3 that are most important for
the learner to internalize. These are typically the "recurring pattern"
connections, because recognizing them dramatically reduces the effort needed to
learn the new material.

Frame these as: "If you remember X from [earlier lecture], then Y in this week's
material works the same way, except [specific difference]."

## Output format

```
## Connection Map: [This Week's Topic]

### Recurring Patterns
**[Pattern name]**
[Earlier topic] (Lecture N) → [Current topic] (Lecture M)
[1–2 sentence explanation with concrete anchor]

### Building Blocks
**[What's being built on]**
[Foundation concept] (Lecture N) → [How it's used now] (Lecture M)
[1–2 sentence explanation]

### Key Contrasts
**[What changed]**
[Old approach] (Lecture N) vs. [New approach] (Lecture M)
[1–2 sentence explanation of what's different and why]

### Looking Ahead
**[What this sets up]**
[Current concept] → [Where it leads]
[Brief note, if identifiable from available materials]

---

### Top connections to lock in
1. [Most important connection — concrete "if you know X, then Y is just..."]
2. [Second most important]
3. [Third, if applicable]
```

## Important guidelines

- **Be specific, not vague.** "This is related to earlier material" is useless.
  "The recurrence relation in Viterbi CKY multiplies child probabilities and
  rule probability, exactly like Viterbi for HMMs multiplies transition and
  emission probabilities" is useful.
- **Don't force connections that aren't there.** If two topics are genuinely
  independent, don't manufacture a link. It's fine to have a short map for a
  week that covers mostly new ground.
- **Only map connections you can ground in the actual materials.** Don't claim
  a connection exists based on general knowledge if you can't find evidence of
  it in the slides the learner has access to.
- **Prioritize connections the learner can act on.** "This is like Viterbi"
  is actionable (they can go review Viterbi). "This relates to information
  theory" is only actionable if their course covered information theory.
- **Keep it concise.** A connection map with 15 entries is overwhelming. Aim
  for 4–8 connections total, with the top 2–3 highlighted. Quality over
  quantity.
