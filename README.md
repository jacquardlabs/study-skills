# Study Skills for Claude

5 composable Claude skills for digging into something new: mapping an unfamiliar topic, reading a paper, critiquing it, synthesizing a body of work, and checking what you actually understood. Built for grad students and working practitioners, in any field.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## What are skills?

Skills are markdown instruction files (`SKILL.md`) that teach Claude how to perform one task consistently — a playbook instead of a vague prompt. Claude loads a skill when your request matches its description. Every skill lives in its own folder and can be used independently or chained together.

## Install

Via the Jacquard Labs marketplace:

```bash
/plugin marketplace add jacquardlabs/marketplace
/plugin install study-skills@jacquardlabs-marketplace
```

Or install this plugin directly:

```bash
/plugin marketplace add jacquardlabs/study-skills
/plugin install study-skills@study-skills
```

On claude.ai, zip each folder under `skills/` and upload it as a custom skill from the Skills settings (requires code execution). Custom skills on claude.ai are separate copies — re-upload after updating.

## Usage

Ask the way you normally would:

- "Get me up to speed on retrieval-augmented generation — I need to decide whether to use it at work"
- "Help me read this paper"
- "Critique this paper — should I trust these results?"
- "How do these four papers on speculative decoding relate?"
- "Quiz me on what I just read"
- "Walk me through the derivation in section 3"

### The research loop

```
topic-map             → What to know first, and what to read in what order
  ├─ paper-reader     → Reading guide for each paper on the path
  │    └─ paper-critique → How much to trust it
  └─ literature-map   → How the papers fit together
understanding-check   → At any checkpoint: questions or a worked problem
```

Enter anywhere. A single paper starts at `paper-reader`; a pile of papers starts at `literature-map`; "I know nothing about X" starts at `topic-map`.

## The skills

| Skill | What it does | Produces |
|---|---|---|
| **topic-map** | Plans entry into an unfamiliar topic: prerequisites, concepts in dependency order, milestone checkpoints, and a verified reading path | Roadmap in chat |
| **paper-reader** | Structured reading guide: summary, problem, key idea, where it sits in the field, results, what to read closely | Reading guide in chat |
| **paper-critique** | Evaluates claims against evidence: claims ledger, threats to validity, reproducibility, scope limits, and production readiness | Critique in chat |
| **literature-map** | Synthesizes several papers: lineage, claims across papers, result comparability, consensus, contested points, open problems | Literature map in chat |
| **understanding-check** | Conceptual questions, or a worked example plus practice problem, with Socratic feedback | Interactive Q&A in chat |

## How it works

Each skill grounds its output in the papers and material you provide. topic-map and literature-map use web search to find readings, and confirm each recommended paper exists or mark it unverified.

Skills hand off to each other: topic-map checkpoints and paper-reader's "3 things you should be able to explain" become understanding-check questions, and paper-critique and literature-map reuse reading guides already in the conversation.

Everything happens in conversation. For long-term retention, put what you learned into a spaced-repetition tool.

## Customization

Each `SKILL.md` is self-contained markdown — instructions, not code. Edit the output sections in `paper-critique` to match your field's review norms, or the milestone count in `topic-map` to match how you like to pace.

## Contributing

> TODO: no `CONTRIBUTING.md` or contribution guidelines found in this repo. Add one if outside contributions are wanted, or state here that it's maintained internally.

## License

MIT — use these however you want.
