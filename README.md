# Decision Case Pooling

**An agent skill for documenting evidence-backed engineering decisions made during AI-assisted software development.**

AI can propose designs, write code, run tests, and diagnose failures. That makes it harder to reconstruct which judgments were genuinely made by a human engineer, which options came from the AI, and which outcomes were actually verified. Decision Case Pooling collects those distinctions in a searchable Markdown/YAML case pool.

It is **not** an automatic productivity score, an AI-dependence metric, or a generator of flattering portfolio stories.

## Installation

With Skills CLI:

```sh
npx skills add aassaapp/decision-case-pooling --skill decision-case-pooling
```

Or ask your Codex skill installer to install the skill from the root of `https://github.com/aassaapp/decision-case-pooling`.

## Example prompts

```text
$decision-case-pooling Find the most meaningful engineering decisions from this session.
$decision-case-pooling Capture this decision, including who framed, tested, and selected the approach.
$decision-case-pooling Update this case with the latest validation evidence.
$decision-case-pooling Analyze independent human judgments versus AI-led work across the case pool.
$decision-case-pooling Migrate these v2 cases to v3 without guessing missing contributions.
```

The skill uses only conversations, code, and records it can actually access. It does not automatically collect past chats, run in the background, or require an MCP server or Notion connection.

## What a case records

Each case is one Markdown file with YAML frontmatter and an evidence-backed narrative.

Its three attribution dimensions are deliberately separate:

1. **Decision owner (`decided_by`):** Who chose the final approach?
2. **Review status (`user_review`):** Did the human actually examine that specific decision?
3. **Phase-level contributions (`contributions`, new in v3):** Who framed the problem, generated hypotheses, designed tests, implemented changes, interpreted results, and chose the outcome? Is there direct evidence of independent human judgment in a given phase?

For example, a human may select an AI-proposed design after reading the results. That is a **human-owned decision**, but it is **not evidence that the human independently devised the design**. Conversely, a human might independently identify a defective assumption while AI performs all subsequent code changes.

### v3 contribution example

```yaml
schema_version: 3
decided_by: user
user_review: reviewed
contributions:
  problem_framing:
    lead: user
    human_actions: [originated, challenged]
    independent_judgment: demonstrated
    evidence: [E1]
  implementation:
    lead: ai
    human_actions: [delegated]
    independent_judgment: not_observed
    evidence: [E2]
  final_decision:
    lead: user
    human_actions: [selected, reviewed]
    independent_judgment: not_observed
    evidence: [E3]
```

Here, `not_observed` means **there is no evidence of independent human judgment in that phase**. It does not mean the person lacks the skill. Missing phases are not classified and must not be counted as AI-owned.

## Repository and case-pool structure

This public repository distributes the skill, not your private work history.

```text
<central-case-pool>/
├── index.md
└── <project>/
    └── 2026-10-05-topic.md
```

Optionally, a project repository can contain a `.casepool` pointer to that central location. The skill should ask before creating this pointer. Case records remain in the chosen central location rather than being scattered among source repositories.

The template includes: situation, decision trigger, actual alternatives, contribution chronology, validation and limits, lessons, evidence, and open questions.

## Analysis without false precision

Across cases, the skill can count decision owners, verified reviews, observed phase leads, and demonstrated independent human judgments. It must report denominators, unknowns, correlated cases, and evidence limits.

**It must not turn these counts into an overall human-contribution percentage, an engineering-ability score, or a hiring-readiness prediction.** Cases are intentionally selected for interesting decisions, so they are not representative of all working hours.

For learning, an optional follow-up is to try explaining or reproducing an AI-led choice without assistance. That is new evidence of current ability, not a retroactive claim about the original decision.

## Version compatibility

**v3 adds phase-level contributions while preserving the existing v2 fields.** Old v2 records remain valid and readable. They are not automatically rewritten, and the absence of `contributions` means *unclassified*, not *AI-led*.

Upgrade a case only with its source conversation, relevant code, and test evidence available. Keep stable UUIDs and original dates; add only evidenced phases; preserve human-authored content. Detailed rules are in [SKILL.md](SKILL.md) and [MIGRATION.md](MIGRATION.md).

## Files

- [SKILL.md](SKILL.md) — Operating rules, provenance criteria, attribution, analysis, and saving.
- [references/case-template.md](references/case-template.md) — v3 Markdown/YAML case template.
- [MIGRATION.md](MIGRATION.md) — Safe v2-to-v3 migration examples and verification checklist.
- [agents/openai.yaml](agents/openai.yaml) — Codex display metadata.

## Privacy and reliability

AI-generated retrospectives are interpretations supported by cited records, not independent proof of competence. Before publication, remove secrets, internal documents, identifying personal data, and other confidential information. Saving locally does not authorize publishing or pushing changes.
