# Residual Risk Acceptance Record
**Union Grid Energy** · Document ID: UGE-RRA-2026-004 · Version 1.0 · Classification: Confidential
**System:** GridSense Load Forecasting & Demand-Response Automation
**Linked records:** UGE-AIRA-2026-004 · UGE-AIRR-2026-004 · UGE-AIIA-2026-004

---

## 1. Purpose
Under Union Grid's Enterprise Risk Management Policy (ERM-POL-01 §7), any risk whose **residual** rating remains **High** after treatment must be formally accepted by a named executive at Vice-President level or above, with stated conditions and a review date. Risk acceptance is a deliberate, accountable decision — not a default outcome of unfinished treatment.

## 2. Accountable Owner

| Field | Detail |
|---|---|
| Accepting executive | **Dana Whitfield, Vice President, System Operations** |
| Authority | ERM-POL-01 §7.3 — VP may accept residual High risks for systems within their operational accountability |
| Advised by | Director Grid Analytics; CISO; AI Risk Lead; Enterprise Risk Manager |
| Notified | Chief Operating Officer; Enterprise Risk Committee (2026-10-08 meeting) |

## 3. Risks Presented for Acceptance

### RRA-1 — R-02 Structural drift from DER / EV / heat-pump growth
| Item | Detail |
|---|---|
| Residual rating | L3 × I4 = **12 High** |
| Why not lower | Load-shape change from customer-side technology is external and accelerating; monthly retraining reduces but cannot eliminate lag. Further reduction requires DER visibility (interconnection telemetry) not available until the ADMS programme completes (2028). |
| Compensating controls | Monthly retrain; residual drift monitor by hour-of-day with alert at 3% bias; prediction-interval dispatch on high-risk days; Fallback Mode B available |
| Conditions of acceptance | (1) Drift alert breaches reported to VP within 5 business days; (2) any 2 consecutive months of bias > 3% → mandatory model review and re-assessment |
| Acceptance period | 12 months |

### RRA-2 — R-10 Rebound ("snapback") peak after events
| Item | Detail |
|---|---|
| Residual rating | L3 × I4 = **12 High** |
| Why not lower | Staggered release is implemented by 2 of 3 aggregators; the third (≈ 31% of devices) cannot support it until a firmware release in Q2 2027. |
| Compensating controls | M3 now models rebound; operators see projected post-event peak; events on non-staggering aggregator limited to 60 min |
| Conditions of acceptance | (1) Aggregator C contract amendment requiring staggered release by 2027-06-30; (2) if rebound exceeds forecast by > 100 MW in any event, real-time auto-dispatch for Aggregator C suspended pending review |
| Acceptance period | Until 2027-06-30 (shorter than standard) |

### RRA-3 — R-16 Unauthorised model / configuration change via MLOps pipeline
| Item | Detail |
|---|---|
| Residual rating | L3 × I4 = **12 High** |
| Why not lower | Signed artefacts and change approval are implemented; full re-architecture of the pipeline into the OT security zone is a 9-month project (budgeted FY27). |
| Compensating controls | Signed model artefacts verified at load; deployment requires approved change ticket; independent hard limiter (R-09) bounds the physical consequence of a tampered model; integrity monitoring alerts to SOC |
| Conditions of acceptance | (1) Pipeline re-architecture funded and started by 2027-01-31; (2) quarterly access review of pipeline credentials; (3) any integrity-check failure → automatic Fallback Mode B and incident response |
| Acceptance period | 12 months or until re-architecture complete |

## 4. Aggregate Consideration
The accepting executive considered the **combined** effect of the three residual risks. RRA-1 and RRA-2 could coincide on a high-DER, high-heat day. This combination was judged acceptable because: (a) the independent hard limiter caps automated MW; (b) P90 dispatch logic applies on high-heat days; (c) operators retain override; (d) Fallback Mode A is triggered automatically on input anomalies.

## 5. Risks Not Eligible for Acceptance
No **Critical** residual risks remain. Under ERM-POL-01, Critical residual risks cannot be accepted below CEO level and would have required suspension of real-time automated dispatch.

## 6. Decision

> I, **Dana Whitfield, Vice President, System Operations**, have reviewed the AI Risk Assessment, Risk Register and AI Impact Assessment for GridSense. I understand the residual risks RRA-1, RRA-2 and RRA-3, their potential consequences for grid reliability and customers, and the compensating controls in place. I **accept** these residual risks subject to the conditions stated above, and I accept accountability for their ongoing monitoring until the review date.

| Field | Value |
|---|---|
| Decision | **Accepted with conditions** |
| Signature | [Signed electronically] |
| Date | 2026-10-02 |
| Review date | 2027-06-30 (RRA-2) · 2027-10-02 (RRA-1, RRA-3) |

## 7. Revocation Triggers
Acceptance is automatically void, and the risk returns for treatment, if: a stated condition is breached; a related incident occurs; the auto-dispatch envelope is increased; or the Impact Assessment equity metric is breached for 3 consecutive months.

## 8. Distribution
COO · Enterprise Risk Committee · CISO · Director Grid Analytics · AI Governance · Internal Audit (for annual ERM audit sample)
