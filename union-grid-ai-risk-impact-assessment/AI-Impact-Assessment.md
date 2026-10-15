# AI Impact Assessment — GridSense
**Union Grid Energy** · Document ID: UGE-AIIA-2026-004 · Version 1.0 · Classification: Internal — suitable for regulator sharing on request
**Accountable executive:** VP System Operations · **Prepared by:** AI Governance with Customer Programmes and Regulatory Affairs · **Date:** 2026-09-22

---

## 1. Purpose and Question Answered
**Who is affected by GridSense, in what ways — positive and negative — and are those effects fair, acceptable and adequately mitigated?**

The [AI Risk Assessment](AI-Risk-Assessment.md) examined how the system could fail. This document examines the **people and communities** on the receiving end of the system — **even when it works exactly as designed**. Several of the most significant findings below arise from correct operation, not failure.

**Audience:** Executive Committee, Public Utility Commission staff, customer and low-income advocates, Sustainability & Equity Council.

## 2. System in Plain Language
GridSense predicts how much electricity customers will use. When it predicts demand will be too high for the grid to handle comfortably, it can — in some cases automatically — ask enrolled customers' smart thermostats, water heaters and EV chargers to use less power for a short time (a "demand-response event"). Customers enrolled in these programmes receive bill credits and can opt out of any individual event.

## 3. Benefits Considered

| Benefit | Who benefits | Evidence |
|---|---|---|
| Fewer emergency outages during peak events | All customers; critical facilities | 2025 summer: 3 load-shed events avoided (simulation) |
| Lower peak generation cost → lower rates over time | All customers | Est. $18m/yr avoided peak procurement |
| Lower emissions from peaker plants | Public, communities near peakers (often disadvantaged) | Est. 41k tCO₂e/yr |
| Bill credits for participants | Enrolled customers | Avg $62/yr per enrolled household |

## 4. Stakeholder Map

| # | Stakeholder group | Size | Relationship to system | Voice in assessment |
|---|---|---|---|---|
| SH-1 | Enrolled residential customers | ~380k households | Directly subject to automated events | Survey (n = 2,400); complaint analysis |
| SH-2 | Medically vulnerable customers (medical-baseline, life-support registry) | ~14k enrolled of 52k registered | Directly subject; heightened health risk | Advocacy liaison interviews |
| SH-3 | Low-income / energy-burdened households | ~96k enrolled | Directly subject; may depend on bill credits | Low-income advocate workshop |
| SH-4 | Non-enrolled customers | ~1.3m | Indirect — benefit from reliability, exposed to outages if forecast fails | Proxy via PUC consumer advocate |
| SH-5 | Commercial & industrial customers | ~1,100 in direct-load control | Directly subject; production impacts | Account manager interviews |
| SH-6 | Control-room operators | 64 staff | Users; accountable for overrides | Workshop |
| SH-7 | Regulators & ISO/RTO | PUC, NERC/regional entity, ISO | Oversight; reliance on forecasts | Regulatory Affairs |

## 5. Harm Categories Assessed
1. **Service disruption** — unplanned loss or curtailment of electricity
2. **Health & safety** — heat/cold exposure, medical-device dependency, public safety during outages
3. **Equity of service** — whether burdens and benefits are distributed fairly
4. **Economic** — costs, lost credits, production losses
5. **Autonomy & privacy** — control of one's own devices; use of AMI interval data
6. **Transparency & trust** — understanding of and recourse against automated actions

## 6. Impact Analysis

### 6.1 Impact matrix

| Stakeholder | Service disruption | Health & safety | Equity | Economic | Autonomy & privacy | Transparency |
|---|---|---|---|---|---|---|
| SH-1 Enrolled residential | Moderate | Low | Moderate | +ve (credits) | Moderate | Moderate |
| SH-2 Medically vulnerable | Moderate | **Severe** | **High** | Low | Moderate | **High** |
| SH-3 Low-income | Moderate | Moderate | **High** | Mixed | Moderate | High |
| SH-4 Non-enrolled | Low (+ve reliability) | Low | Low | +ve | None | Low |
| SH-5 C&I | Moderate | Low | Low | Moderate | Low | Low |
| SH-6 Operators | — | Low | — | — | — | Moderate (accountability without insight) |
| SH-7 Regulators | — | — | — | — | — | Moderate |

### 6.2 Key findings

**F-1 — Medically vulnerable customers enrolled in automated events (Severe).**
14,000 customers on the medical-baseline or life-support registry were enrolled in thermostat programmes and **were not excluded** from real-time automated events. During the 2025 heat dome, 3,900 of them experienced at least one event > 90 minutes with indoor setpoint raised 4 °F. *This is correct system operation, not failure.*
→ **Mitigation:** Automatic exclusion of registry customers from automated real-time events (opt-in only for day-ahead, with max 2 °F offset); registry sync daily. **Status: implemented 2026-10-15.**

**F-2 — Event burden concentrated in older housing stock (High equity impact).**
Distributional analysis by census-tract median-income quintile (2025 season):

| Income quintile | Share of enrolled devices | Avg real-time events / device | Avg event minutes / device | Opt-out rate |
|---|---|---|---|---|
| Q1 (lowest) | 21% | 11.2 | 1,010 | 9% |
| Q2 | 22% | 9.4 | 840 | 12% |
| Q3 | 20% | 7.1 | 610 | 16% |
| Q4 | 19% | 5.6 | 470 | 19% |
| Q5 (highest) | 18% | 4.9 | 420 | 22% |

Q1 households received **2.3× more events** than Q5. Root cause: M3 learned that poorly insulated homes deliver more kW per event, so the optimiser targeted them. Lower opt-out rates in Q1 correlate with reliance on bill credits — meaning the burden fell on those least able to decline.
→ **Mitigations:** (a) fairness constraint in dispatch optimiser — event-rate ratio between quintiles ≤ 1.3; (b) per-premise caps (R-08); (c) weatherisation programme referrals for high-event premises; (d) monthly equity metric reported to Sustainability & Equity Council.

**F-3 — Customers do not know events are automated (High transparency impact).**
Survey: 64% of enrolled customers believed a person decides each event; 31% did not know they could opt out of an individual event.
→ **Mitigation:** Event notifications state "automatically scheduled by our grid-forecasting system"; one-tap opt-out in every notification; plain-language explainer; annual programme disclosure.

**F-4 — C&I production disruption from short-notice events (Moderate economic).**
Real-time events gave < 10 minutes' notice to 3 manufacturers on direct-load control.
→ **Mitigation:** Minimum 30-minute notice for C&I unless grid emergency declared; contract terms updated.

**F-5 — AMI data used beyond original purpose (Moderate privacy).**
Premise-level interval data used to train M3 response model; customer privacy notice referred only to billing and reliability.
→ **Mitigation:** Privacy notice updated; premise data pseudonymised in training; retention limited to 25 months; PUC data-access rules confirmed by Legal.

**F-6 — Operators accountable but lack insight (Moderate).** See Risk R-11. Operators reported uncertainty about whether they *should* override.
→ **Mitigation:** Explicit override authority in operating procedure; drivers & intervals displayed.

## 7. Redress & Recourse

| Mechanism | Description |
|---|---|
| Event opt-out | One-tap, no penalty beyond forfeiting that event's credit |
| Programme exit | Any time, no fee |
| Complaint route | Dedicated DR line; complaints tagged "automated event" and reviewed monthly |
| Medical emergency | 24/7 override line; registry customers' events cancelled immediately |
| Regulator escalation | PUC consumer-affairs route referenced in all programme materials |

## 8. Overall Impact Determination

| Question | Determination |
|---|---|
| Do benefits outweigh harms at population level? | Yes — reliability, cost and emissions benefits are substantial |
| Are harms acceptably distributed? | **Not before mitigation.** Acceptable after F-1 and F-2 mitigations, subject to monitoring |
| Are any groups exposed to unacceptable harm? | Medically vulnerable customers were — **resolved by exclusion** |
| Is transparency adequate? | After F-3 mitigation |
| Recommendation | **Continue operation with conditions:** F-1 exclusion live; F-2 fairness constraint live by 2026-12-01; equity metric reported monthly; re-assess after summer 2027 |

## 9. Monitoring of Affected Outcomes

| Indicator | Target | Frequency | Reported to |
|---|---|---|---|
| Event-rate ratio Q1:Q5 | ≤ 1.3 | Monthly | Sustainability & Equity Council |
| Registry customers in automated events | 0 | Per event | Customer Programmes |
| Complaints tagged "automated event" | Trending down | Monthly | VP System Operations |
| Customer awareness of automation | ≥ 80% | Annual survey | Executive Committee |
| C&I notice-period breaches | 0 outside emergencies | Per event | Key Accounts |

## 10. Sign-off

| Role | Decision | Date |
|---|---|---|
| VP System Operations (accountable executive) | Approved with conditions | 2026-09-29 |
| Chief Customer Officer | Concur | 2026-09-29 |
| General Counsel | Concur | 2026-09-30 |
