# L1 / L2 / L3 Tier Model

## Overview

The tier model defines how AI's role changes based on task complexity, sensitivity, and cross-department scope. It is not a routing hierarchy — it's a cognitive model that determines what the workspace assembles for the fulfiller at each moment.

## Tier Definitions

### L1 — AI Autonomous
**AI role:** Resolves end-to-end
**Human role:** Validates or approves (if needed)
**Screen:** Queue View — auto-resolved items with reopen option

**Criteria:**
- Known resolution path exists
- Low sensitivity / low risk
- Single department
- Precedent exists for automated handling

**Volume expectation:** 60–75% of cases in mature deployments

**Examples by department:**

| Department | L1 Examples |
|-----------|-------------|
| S2P | Catalog orders, PO status inquiries, reorders, auto 3-way match, standard PO amendments |
| Finance (AR) | Auto cash application, standard dunning, unapplied cash matching, payment status inquiries |
| Finance (AP) | Auto-approved payments, duplicate detection, standard payment scheduling |
| HR | Policy FAQs, PTO inquiries, standard form submissions, room bookings, password resets |
| Legal | Standard NDA processing (template match), term extraction, renewal reminders |
| Facilities | Standard room bookings, badge provisioning, catalog equipment orders, desk setup (known plans) |

---

### L2 — AI-Augmented Specialist
**AI role:** Summarizes, recommends, surfaces evidence
**Human role:** Makes the judgment call
**Screen:** Case Detail — AI summary + recommended actions + confidence + evidence

**Criteria:**
- Transferred or inherited case
- Conflicting data or accounts
- Policy nuance or ambiguity
- Sensitivity requiring human judgment
- Threshold exceeded (financial, authority, risk)

**Volume expectation:** 20–35% of cases

**Examples by department:**

| Department | L2 Examples |
|-----------|-------------|
| S2P | Invoice variance resolution, supplier risk assessment, RFP evaluation, contract redline review |
| Finance (AR) | Credit hold/release decisions, unauthorized deductions, write-off approvals, payment pattern analysis |
| Finance (AP) | Tolerance exceptions, payment timing decisions, accrual discrepancies |
| HR | Benefits edge cases, transfer processing, cross-dept onboarding coordination, request reclassification |
| Legal | Redline review with AI summaries, non-standard terms, compliance exceptions |
| Facilities | Non-standard workspace configs, capacity planning, vendor coordination for specialized work |

---

### L3 — Human-Led (AI-Informed)
**AI role:** Provides evidence, cross-department visibility, risk assessment
**Human role:** Leads complex decisions, coordinates across teams
**Screen:** Orchestration View — dependency tracking, blocker visibility, authority escalation

**Criteria:**
- Multi-department workflow
- Blocked dependencies requiring coordination
- Authority or policy exception required
- Org-wide impact
- Regulatory or legal sensitivity

**Volume expectation:** 5–15% of cases

**Examples by department:**

| Department | L3 Examples |
|-----------|-------------|
| S2P | Sole-source exceptions, strategic supplier selection, fraud escalations, supplier termination |
| Finance (AR) | Large customer default, credit term restructuring, concentration risk mitigation |
| Finance (AP) | Audit-triggered holds, payment control redesign, financial policy exceptions |
| HR | ER investigations (misconduct, harassment), termination recommendations, accommodation requests |
| Legal | M&A agreements, regulatory disputes, external counsel engagement, board-level contracts |
| Facilities | Major relocations, lease negotiations, building safety incidents |

## Phase Escalation

A critical concept: **tasks routinely escalate across tiers within the same case lifecycle.** This is especially true in S2P and HR onboarding.

Example: A procurement request starts as L1 (catalog order) → escalates to L2 (supplier compliance gap found during onboarding) → escalates to L3 (policy exception needed because no compliant alternative meets the timeline).

The workspace must handle this fluidly — not as three separate cases, but as one evolving case where the AI role adapts at each phase gate.
