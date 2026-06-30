# AI Governance Risk-Screening Agent

A two-agent pipeline that does a first-pass risk screen on any AI use case before deployment. One agent assesses, a second agent critiques, and the output is a structured risk memo.

Built with the Anthropic API in Google Colab.

---

## Why this exists

Most teams plug an LLM into a workflow without a structured check on whether the use case is actually risky. This notebook is a small attempt to operationalize that check — grounded in a real framework, with a second agent that actively tries to poke holes in the first agent's conclusions.

---

## How it works

**Agent 1 — Assessor**
Scores the use case across five risk dimensions drawn from the NIST AI Risk Management Framework (Govern, Map, Measure, Manage):

- Bias & fairness
- Privacy & data governance
- Transparency & explainability
- Human oversight & accountability
- Regulatory exposure

Each dimension gets a risk level (Low / Medium / High) and a short rationale.

**Agent 2 — Critic**
Reads the assessor's output and challenges it. Looks for overconfident claims, missing failure modes, and gaps in reasoning. Then writes a final one-page risk memo with an overall rating and prioritized mitigations.

Keeping the two agents separate matters: the critic has no stake in defending the assessor's conclusions, so it's structurally free to disagree.

---

## Test cases

The notebook runs four use cases that span the risk spectrum:

| Use Case | Overall Rating | Key Finding |
|---|---|---|
| AI resume screening | 🔴 High | Structurally broken — bias, regulatory exposure, and no meaningful human oversight |
| Internal coding assistant | 🟡 Low-Medium | Control case — privacy and automation bias are the real risks, not obvious ones |
| ER triage assistant | 🔴 High | Workflow design is the flaw — anchoring bias degrades nurse judgment even when review happens |
| Support ticket router | 🟠 Medium-High | Incentive structure makes human oversight illusory — tickets-per-hour rewards ratification, not review |

The critic agent caught something the assessor missed in every single case.

---

## Setup

1. Open the notebook in Google Colab
2. Get an API key from [console.anthropic.com](https://console.anthropic.com)
3. In the Colab sidebar, click the key icon → add a secret named `ANTHROPIC_API_KEY` → paste your key → toggle notebook access on
4. Run all cells in order

To screen your own use case, replace the text in the final cell with any AI deployment scenario you want assessed.

---

## Stack

- Anthropic API (`claude-sonnet-4-6`)
- Python
- Google Colab

---

## Limitations

- This is a first-pass screen, not a compliance determination or legal opinion
- The assessor and critic are both LLMs — they can miss things, hallucinate regulatory details, or be overconfident in ways the pipeline itself cannot fully catch
- Output quality depends on how specifically the use case is described — vague inputs produce vague assessments
- No ground truth to evaluate against; findings should be reviewed by qualified legal, compliance, and technical stakeholders before any deployment decision

---

Built by [Ananya Awasthi](https://linkedin.com/in/ananya-awasthi2000/)
