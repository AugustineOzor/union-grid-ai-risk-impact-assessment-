# AI Risk Assessment — GridSense Load Forecasting & Demand-Response Automation
**Union Grid Energy** · Document ID: UGE-AIRA-2026-004 · Version 1.0 · Classification: Confidential — CIP-sensitive details redacted
**System owner:** Director, Grid Analytics · **Assessor:** AI Risk Lead · **Date:** 2026-09-15 · **Next review:** 2027-03-15 or on trigger

---

## 1. Purpose and Question Answered
**What could go wrong with GridSense, how likely is it, how severe would it be, and what must be done about it?**
This assessment focuses on the system — its models, data, integrations, automation and oversight. The question of *who* bears consequences and whether that is fair is addressed separately in the [AI Impact Assessment](AI-Impact-Assessment.md).

## 2. System Characterisation

| Element | Description |
|---|---|
| Models | M1 gradient-boosted short-term load forecast (15 min – 48 h); M2 temporal neural network for 2–7 day forecast; M3 DR response-estimation model (predicts curtailment achieved per event) |
| Inputs | SCADA feeder load; AMI interval data (aggregated); weather (2 commercial providers + NWS); calendar; DER/solar telemetry; EV charging data |
| Outputs | Load forecast with prediction intervals; DR event recommendation (size, location, duration) |
| Automation | Day-ahead: operator approves DR plan. Real-time: DRA auto-dispatches events ≤ 150 MW total and ≤ 2 h when forecast peak exceeds operating threshold |
| Interfaces | Energy Management System (EMS) display; aggregator APIs (3 aggregators, ~410k enrolled devices); C&I direct-load control |
| Human oversight | Control-room operator can cancel/override any event; shift supervisor approves day-ahead plans |
| Users | System operators, DR programme managers, market operations |

**Context of use classification (reference):** Comparable systems in the EU would fall under EU AI Act Annex III point 2 (AI as safety component in the management and operation of critical infrastructure — supply of electricity), making Article 9 the appropriate structural model for this assessment.

## 3. Methodology

### 3.1 Process (mirrors EU AI Act Art. 9(2) and ISO/IEC 23894)
1. Identify known and reasonably foreseeable risks under intended use **and** reasonably foreseeable misuse.
2. Estimate and evaluate each risk (inherent).
3. Evaluate existing controls; estimate current risk.
4. Define treatment; estimate residual risk.
5. Evaluate residual risk against appetite; escalate for acceptance where required.

### 3.2 Likelihood scale

| Score | Label | Definition (over 12 months) |
|---|---|---|
| 1 | Rare | < 5% |
| 2 | Unlikely | 5–20% |
| 3 | Possible | 20–50% |
| 4 | Likely | 50–80% |
| 5 | Almost certain | > 80% |

### 3.3 Impact scale (highest applicable dimension)

| Score | Reliability | Safety | Financial | Regulatory / reputational |
|---|---|---|---|---|
| 1 | No customer impact | None | < $100k | Internal only |
| 2 | Minor localised curtailment beyond plan | Discomfort, no health risk | $100k–1m | PUC inquiry |
| 3 | Unplanned curtailment > 50k customers or reserve shortfall | Elevated health risk to vulnerable customers | $1–5m | Formal PUC investigation; media |
| 4 | Rotating outages / emergency load shed | Credible risk of serious injury | $5–25m | NERC violation; fines |
| 5 | Cascading outage / wide-area event | Credible risk of fatality | > $25m | Licence / rate-case consequences |

### 3.4 Rating matrix (L × I)

| Score | Rating | Appetite | Required action |
|---|---|---|---|
| 20–25 | **Critical** | Outside appetite | Treat immediately; executive notification |
| 12–19 | **High** | Outside appetite | Treat within 90 days; VP acceptance if residual remains High |
| 6–11 | **Medium** | Tolerable | Treat where cost-effective; Director acceptance |
| 1–5 | **Low** | Within appetite | Monitor |

## 4. Risk Identification Summary

18 risks across six categories (full detail in [Risk-Register.md](Risk-Register.md)):

| Category | Risks | Highest inherent |
|---|---|---|
| Model performance (accuracy, distribution shift, drift) | R-01 – R-04 | Critical |
| Data & pipeline | R-05 – R-07 | High |
| Automation & control | R-08 – R-10 | Critical |
| Human oversight & operations | R-11 – R-13 | High |
| Security (cyber-physical) | R-14 – R-16 | Critical |
| Third party & governance | R-17 – R-18 | Medium |

## 5. Key Risk Analysis

### R-01 — Under-forecast during extreme heat (distribution shift)
- **Description:** Training data contains few days above 105 °F. During record heat, M1 under-forecast peak by 6.8% (back-test on 2025 heat dome) — larger than the operating reserve margin.
- **Consequence chain:** under-forecast → insufficient DR dispatched → reserve shortfall → emergency load shed.
- **Inherent:** L4 × I5 = **20 Critical**
- **Existing controls:** Operator judgement; ISO/RTO reserve requirements.
- **Treatment:** (a) Extreme-weather ensemble with physics-informed temperature response; (b) prediction-interval-based dispatch (use P90, not P50, when temperature > 100 °F); (c) synthetic stress scenarios in validation; (d) operator alert when inputs are outside training envelope.
- **Residual:** L2 × I5 = **10 Medium**

### R-08 — Automated over-curtailment from erroneous forecast or estimation error
- **Description:** DRA auto-dispatches events; M3 over-estimates achievable curtailment, so DRA calls repeated events to reach targets, cycling the same premises.
- **Inherent:** L3 × I4 = **12 High** (safety dimension for vulnerable customers)
- **Treatment:** Hard per-premise caps (max 2 events/day, 4/week) enforced in aggregator integration, not in model; medical-baseline exclusion; event-size ceiling.
- **Residual:** L2 × I3 = **6 Medium**

### R-14 — Manipulation of weather data feed
- **Description:** Weather provider API compromised or spoofed → falsified temperature inputs drive wrong forecast and automated DR.
- **Inherent:** L4 × I5 = **20 Critical** — likelihood rated 4 because the feed is internet-facing, single-sourced for real-time use, and automated dispatch removes the human check that would otherwise catch implausible values.
- **Treatment:** Multi-source weather consensus with divergence alarm; signed/TLS-pinned feeds; plausibility bounds; automatic fallback to operator-approval mode on divergence; inclusion in CIP-013 supply-chain risk plan.
- **Residual:** L1 × I5 = **5 Low**

### R-11 — Automation bias in control room
- **Description:** Operators accept DR recommendations without challenge; observed approval rate 99.4% with median review time 22 s.
- **Inherent:** L4 × I3 = **12 High**
- **Treatment:** Show prediction intervals and top drivers on EMS display; mandatory challenge prompts when forecast deviates > 5% from operator's persistence forecast; quarterly simulator drills including seeded bad forecasts; track override rate as a KPI.
- **Residual:** L3 × I3 = **9 Medium**

## 6. Measurement Plan (NIST AI RMF MEASURE)

| Metric | Threshold | Frequency | Owner |
|---|---|---|---|
| Day-ahead peak MAPE | ≤ 3.0% (≤ 4.5% on days > 100 °F) | Daily | Grid Analytics |
| Prediction-interval coverage (P10–P90) | 78–82% | Weekly | Grid Analytics |
| Input out-of-envelope rate | Alert at any occurrence; report monthly | Real-time | Grid Analytics |
| DR curtailment estimate error (M3) | ≤ 15% | Per event | DR Programmes |
| Operator override rate | Investigate if < 1% or > 15% over 30 days | Monthly | Ops Training |
| Weather source divergence | Alarm > 4 °F across sources | Real-time | OT Security |
| Events per premise (distribution) | Cap breaches = 0 | Per event | DR Programmes |
| Equity metric (from Impact Assessment) | Event-rate ratio between tract quintiles ≤ 1.3 | Monthly | Customer Programmes |

## 7. Disengagement & Fail-Safe (NIST AI RMF MANAGE 2.4)
- **Fallback mode A:** Real-time auto-dispatch disabled; all events require operator approval. Triggered automatically on weather divergence, pipeline staleness > 15 min, or input out-of-envelope.
- **Fallback mode B:** GridSense forecast replaced by legacy regression forecast and ISO forecast. Triggered by operator or Grid Analytics.
- **Tested:** Mode A tested monthly in production (scheduled); Mode B tested quarterly in simulator. Last test 2026-08-29 — switchover time 41 s.

## 8. Conclusion
Before treatment, GridSense carried **4 Critical** and **8 High** risks — predominantly from the combination of distribution shift and **automated** dispatch. After treatment, **no Critical risks remain** and **3 High residual risks** (R-02, R-10, R-16) require VP-level acceptance, recorded in [Residual-Risk-Acceptance.md](Residual-Risk-Acceptance.md). Real-time automated dispatch may continue **only** with fail-safe Mode A triggers operational.

## 9. Re-assessment Triggers
Model retrain with new features or architecture · increase in auto-dispatch envelope · new aggregator or device class · major forecast-error event (> 5% peak miss) · cyber incident affecting inputs · regulatory change · 6-month scheduled review.
