# Interaction Modes

## Overview

The workspace supports three interaction modes. The mode is determined by case complexity, sensitivity, and cross-department scope — not by the fulfiller's preference or department.

## Conversational Mode
**When:** Routine cases where AI handles resolution
**AI behavior:** Resolves autonomously, presents result for validation
**Fulfiller behavior:** Reviews auto-resolved cases, reopens if needed, validates AI decisions
**Screen surface:** Queue View with auto-resolved section

**Characteristics:**
- Known resolution path
- Low sensitivity
- Single department
- AI confidence is high (>90%)

**Design implication:** The Queue View needs two sections — "Needs you" (cases requiring human judgment) and "AI handled" (auto-resolved cases with reopen option). The split itself communicates the AI's role.

---

## Collaborative Mode
**When:** Cases requiring human judgment alongside AI support
**AI behavior:** Summarizes context, recommends actions, surfaces evidence with confidence levels
**Fulfiller behavior:** Reviews AI analysis, applies domain judgment, decides and acts
**Screen surface:** Case Detail View with AI summary, recommendations, and evidence panels

**Characteristics:**
- Transferred or inherited case
- Conflicting accounts or data
- Sensitivity or policy nuance
- AI confidence is moderate (60–90%)

**Design implication:** The Case Detail must answer three questions in 30 seconds: (1) What happened / what does AI think? (2) What should I do? (3) Why does AI think that? The specialist must be able to accept, modify, or override the recommendation with equal ease.

**Critical pattern: AI recommendation with override**
Every AI recommendation must include:
- The recommendation itself ("Approve with conditions")
- Confidence level (85%)
- Reasoning (3–4 bullet points)
- Evidence trail (linked documents, data sources)
- Counterpoint (the strongest argument against the recommendation)
- Action buttons including an explicit override path

---

## Orchestration Mode
**When:** Multi-department cases with dependencies, blockers, and coordination needs
**AI behavior:** Maps dependencies, identifies critical path, flags blockers, suggests nudge/escalation actions
**Fulfiller behavior:** Coordinates across teams, unblocks dependencies, makes authority-level decisions
**Screen surface:** Orchestration View with dependency chain, status across departments, and action options

**Characteristics:**
- Multi-department workflow
- Blocked dependencies
- Ownership ambiguity
- Authority or policy exception required

**Design implication:** The Orchestration View must show the full dependency chain — who is blocking, who is waiting, what's the critical path, and what action the fulfiller can take (nudge, escalate, reassign). This is the screen where cross-department visibility becomes the product differentiator.

## Mode Transitions

Modes are not static. A case can transition between modes as complexity changes:

- **L1 → L2:** Auto-match fails, case moves from Queue to Case Detail
- **L2 → L3:** Specialist identifies cross-department blocker, case gains Orchestration View
- **L3 → L2:** Blocker resolved, case returns to specialist for final decision

The workspace should handle these transitions without losing context — the AI summary should accumulate, not reset.
