---
name: paper-critique
description: >
  Critically evaluate an academic paper's claims and evidence. Use this skill when
  the user uploads or names a research paper and asks to "critique this paper,"
  "what are the weaknesses," "is this paper any good," "should I trust these
  results," "poke holes in this," "review this paper," "what are the limitations,"
  or any request to judge how well a paper's evidence supports its claims. Works
  standalone or as a follow-up after paper-reader. Also trigger when the user is
  deciding whether to adopt a paper's method in practice ("would this work in
  production?"). This skill evaluates a paper — it does not produce a reading guide
  (use paper-reader) or quiz questions (use concept-check).
---

# Paper Critique Skill

## Purpose

Tell the learner how much to trust a paper's main claim, and why. Most readers
take a paper's framing at face value: the abstract says "significantly
outperforms," the tables look convincing, and the limitations section reads as a
formality. This skill does the work of a skeptical reviewer — matching each claim
to its evidence, finding where the evidence is thinner than the prose suggests,
and naming what would settle the question.

## Philosophy

A critique is only useful if it is specific. "The evaluation could be more
thorough" applies to every paper ever written and teaches nothing. "The baseline
in Table 2 uses the default learning rate while the proposed model was tuned over
6 values (Appendix B)" changes how much you believe the headline number. Every
criticism should point at a section, figure, or table.

Not all flaws matter equally. A missing significance test on a 0.3-point gain can
erase the main claim; a typo in an equation that doesn't propagate cannot. Rank
issues by how much they change the verdict on the main claim.

The learner is both a student and a practitioner. They want to know whether the
paper is sound science *and* whether its method would survive contact with a
production system. Both reads belong in the critique.

## Workflow

### Step 1: Access the paper and prior context

- Read the paper (use pdf-reading skill or view the file directly). If the user
  names a paper without uploading it, fetch it if web access is available;
  otherwise ask for the PDF — a critique from memory can't cite locations.
- Check the conversation for a paper-reader guide on this paper. If one exists,
  reuse its summary of the key idea and results rather than restating them.
- Use `project_knowledge_search` to check whether the paper was assigned for a
  course. If so, note what the course uses it for — a critique should not undercut
  the reason it was assigned without saying so.

### Step 2: Extract the central claims

List the 2–5 claims the paper's contribution depends on. Take them from the
abstract, introduction, and conclusion, then find where each is tested. A claim
the paper states but never tests is itself a finding.

### Step 3: Audit the evidence

For each claim, work through the threats to validity below. Record the paper
location for every issue found. Skip checks that don't apply (a theory paper has
no seeds) rather than reporting them as missing.

- **Baselines**: weak, missing, or untuned compared to the proposed method
- **Data leakage or contamination**: test data overlapping training or
  pretraining data, preprocessing fit on the full dataset
- **Metrics**: metric chosen to favor the method, cherry-picked subsets or
  examples, headline numbers that differ from the full results table
- **Variance**: single runs, no seeds reported, no confidence intervals or
  significance tests, gains within run-to-run noise
- **Ablations**: whether they isolate the cause the paper claims, or change
  several things at once
- **Confounds**: extra parameters, compute, data, or tuning budget that could
  explain the gain instead of the claimed idea

### Step 4: Check follow-up work

Use web search, if available, to find later papers that confirmed, refuted,
or superseded the main claim. If search is not available, say the follow-up
section comes from general knowledge and may be outdated.

### Step 5: Produce the critique

Generate the sections in the output format below, then offer follow-up:
- To dig into any single issue in more depth
- To sketch the experiment from "What would change the verdict"
- To run paper-reader if the learner wants a reading guide first

## Output format

**Verdict**
2–3 sentences. How much to trust the main claim, and why. Lead with the single
issue that most affects that trust.

**Claims ledger**

| Claim | Evidence | Rating |
|---|---|---|
| [Central claim, in one line] | [Section / figure / table] | Supported / Partly supported / Unsupported |

**Threats to validity**
Ranked by impact on the main claim. Split into **Fatal or major** (could
overturn a central claim) and **Minor** (worth knowing, doesn't change the
verdict). Each issue names its paper location.

**Reproducibility**
Whether code, data, hyperparameters, and compute budget are released or
reported. List the specific details a reimplementation would have to guess.

**Scope limits**
Where the results stop generalizing. Split into **Stated** (the paper's own
limitations) and **Unstated** (assumptions the paper relies on but doesn't
acknowledge — dataset domain, language, scale, hardware).

**Practitioner read**
Would this hold up in production? Cover adoption cost (engineering effort,
dependencies, retraining), operational risk (failure modes, monitoring,
latency), and scale assumptions (does the method rely on data, compute, or
labels a typical team won't have).

**What would change the verdict**
The specific experiment or evidence that would confirm or refute the main
claim. Concrete enough that someone could run it.

**Follow-up work**
What later papers confirmed, refuted, or superseded it, with titles and years.
State whether this came from web search or general knowledge.

## Important guidelines

- **Ground every criticism in the actual paper text.** Cite the section,
  figure, table, or equation. If a criticism would apply to any paper in the
  field, cut it.
- **Separate fatal flaws from minor issues.** Rank by impact on the main claim,
  not by how easy the issue was to spot.
- **Judge the paper by the standards of its time and venue.** A 2014 paper
  without significance tests met the norms of its venue; say so. Then state
  separately what is outdated now — methods superseded, evaluation practices
  since tightened.
- **Say when something is outside your competence to judge.** If you can't
  verify a proof, a domain-specific experimental protocol, or a statistical
  method, say so instead of rating it.
- **No praise padding.** Mention strengths only where they affect trust in a
  claim (e.g., "results replicate across 5 seeds with tight intervals, so the
  gain in Table 3 is real").
- **Use the actual paper, not your general knowledge of it.** Secondhand
  descriptions often overstate or misstate what the paper tested.
