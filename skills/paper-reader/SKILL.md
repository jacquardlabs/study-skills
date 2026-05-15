---
name: paper-reader
description: >
  Produce a structured breakdown of an academic paper tied to the user's course
  context. Use this skill when the user uploads a research paper (PDF) and asks to
  "break down this paper," "summarize this paper," "help me read this paper,"
  "what's this paper about," "how does this paper relate to class," "paper review,"
  "reading guide for this paper," or any request to understand an academic paper in
  the context of their coursework. Also trigger when the user references assigned
  readings, says "we have to read this for class," or mentions a paper title and
  asks what it's about. This skill produces a structured reading guide — it does not
  generate milestones (use lecture-milestones) or quiz questions (use concept-check).
---

# Paper Reader Skill

## Purpose

Break down an academic paper into a structured reading guide that tells the
learner what the paper does, why it matters, and how it connects to what they're
studying in class. Graduate students often read papers inefficiently — either
getting bogged down in notation on page 2 or skimming so fast they miss the key
contribution. This skill provides the scaffolding for an effective read.

## Philosophy

The goal is NOT to replace reading the paper. It's to give the learner a
framework so that when they read it, they know what to pay attention to, what
they can skim, and where to slow down. Think of it as the reading guide a great
professor would hand out alongside the assignment.

The course connection is critical. A paper assigned for a parsing lecture is
being assigned *because* it introduces or improves on something relevant to
parsing. The breakdown should make that link explicit, not treat the paper in
isolation.

## Workflow

### Step 1: Access the paper and course context

- Read the uploaded paper (use pdf-reading skill or view the file directly)
- Use `project_knowledge_search` to find what topics the course is currently
  covering
- Identify why this paper was likely assigned: does it introduce a method
  discussed in lecture? Provide empirical evidence for a claim? Present the
  original version of an algorithm the slides simplified?

### Step 2: Produce the structured breakdown

Generate these sections:

**The One-Sentence Summary**
What does this paper do, in one plain sentence? No jargon. A non-expert should
be able to understand this sentence.

Example: "This paper introduces a way to translate sentences between languages
using neural networks that process the input one word at a time and then generate
the output one word at a time."

**The Problem**
What problem is the paper trying to solve? Why is it hard? What were people
doing before this paper, and why wasn't that good enough? Keep this to 3–5
sentences. Use language the learner already knows from their course.

**The Key Idea**
What is the paper's main contribution — the core insight or method? Explain it
in terms of concepts from the course. If the paper introduces a new model,
describe it in relation to models the learner already knows. If it introduces
an algorithm, describe how it differs from algorithms they've seen.

This is the most important section. Spend the effort to make it genuinely clear,
not just a compressed version of the abstract.

**How It Connects to Class**
Explicitly map the paper to the relevant lecture material:
- Which lecture topics does this paper relate to?
- Does the paper present the original version of something simplified in the
  slides?
- Does it propose an improvement to a method covered in class?
- Does it provide experimental evidence for a claim made in lecture?

If the connection isn't obvious, say so honestly — sometimes papers are assigned
for breadth rather than direct relevance.

**The Results (What They Showed)**
Summarize the key experimental findings in 3–5 sentences. Focus on:
- What task(s) they evaluated on
- What they compared against
- Whether the results were a clear win, marginal improvement, or mixed

Don't reproduce tables or exact numbers. Describe the story the results tell.

**What to Pay Attention To (and What to Skim)**
Give the learner a reading strategy:
- Which sections are essential for understanding the contribution
- Which sections can be skimmed (e.g., related work, some experimental details)
- Which figures or tables are the most informative
- Where the paper gets notation-heavy and how to approach those parts

**Key Terms**
List 3–7 terms the paper uses that the learner might not know or that the paper
defines differently than the course. Give short definitions.

**3 Things You Should Be Able to Explain After Reading**
Provide three specific claims or ideas that, if the learner can explain them to
someone else, demonstrate they understood the paper. Frame these as capabilities,
not comprehension questions.

Example:
- "Explain why the encoder-decoder architecture needs a fixed-length vector
  between encoding and decoding, and why that's a bottleneck."
- "Describe how reversing the input sequence helps, and why that's surprising."

### Step 3: Offer follow-up

After presenting the breakdown, offer:
- To answer questions about specific sections as they read
- To connect specific parts of the paper to specific slides
- To compare this paper's approach to alternatives covered in class

## Important guidelines

- **Anchor everything to the course.** The learner isn't reading this paper in
  a vacuum. Every explanation should use vocabulary and concepts from their
  lectures when possible.
- **Be honest about difficulty.** If parts of the paper require math background
  the course hasn't covered, say so and tell the learner it's okay to treat
  those parts as a black box for now.
- **Don't over-summarize.** The breakdown should make the learner *want* to read
  the paper with a clear sense of what to look for — not feel like they already
  read it and can skip it.
- **Respect the paper's contribution.** Don't be dismissive of older papers
  because newer methods exist. If a 2014 paper is assigned, it's because the
  ideas matter, not because the BLEU scores are still state-of-the-art.
- **Keep the one-sentence summary truly accessible.** No acronyms, no model
  names, no notation. If your summary includes "LSTM" or "PCFG," rewrite it.
- **Use the actual paper, not your general knowledge of it.** Read what's in
  the uploaded file. Papers often have nuances and specific framings that differ
  from how they're commonly described secondhand.
