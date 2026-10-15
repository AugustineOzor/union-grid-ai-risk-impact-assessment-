# Risk Register — GridSense
**Union Grid Energy** · Document ID: UGE-AIRR-2026-004 · Version 1.0 · Linked to: UGE-AIRA-2026-004 (Risk Assessment), UGE-AIIA-2026-004 (Impact Assessment)
**Register owner:** AI Risk Lead · **Review cadence:** Monthly at Grid Analytics risk review; quarterly to Enterprise Risk Committee

> Scoring: Likelihood (L) 1–5 × Impact (I) 1–5. **Critical 20–25 · High 12–19 · Medium 6–11 · Low 1–5.** Structured for direct export to CSV/XLSX (one row per risk).

---

## 1. Register

| ID | Category | Risk description | Cause | Consequence | Inh. L | Inh. I | **Inh.** | Existing controls | Treatment (planned) | Option | Res. L | Res. I | **Res.** | Risk owner | Due | Status | Source |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R-01 | Model performance | Peak under-forecast in extreme heat | Few training days > 105 °F (distribution shift) | Reserve shortfall → emergency load shed | 4 | 5 | **20 Crit** | Operator judgement; ISO reserves | Extreme-weather ensemble; P90 dispatch above 100 °F; out-of-envelope alert; stress tests | Mitigate | 2 | 5 | **10 Med** | Dir. Grid Analytics | 2026-11-30 | In progress | RA §5 |
| R-02 | Model performance | Structural drift from rooftop solar, EV and heat-pump growth | Behind-the-meter DER changes load shape faster than retrain cycle | Systematic bias in evening peak | 4 | 4 | **16 High** | Annual retrain | Monthly retrain; DER-adjusted features; drift monitor on residuals by hour | Mitigate | 3 | 4 | **12 High** | Dir. Grid Analytics | 2026-12-31 | In progress — **residual accepted** | RA §4 |
| R-03 | Model performance | Over-forecast triggers unnecessary DR events | Conservative bias, holiday effects | Customer fatigue, programme opt-outs, cost | 4 | 2 | **8 Med** | Day-ahead operator approval | Calendar features; opt-out rate monitoring | Mitigate | 3 | 2 | **6 Med** | Mgr DR Programmes | 2027-01-31 | Open | RA §4 |
| R-04 | Model performance | Poor accuracy at feeder/substation level | Model trained at system level; sparse local data | Local overloads; mis-targeted DR | 3 | 4 | **12 High** | None specific | Hierarchical forecasting; feeder-level error metrics; local events need operator approval | Mitigate | 2 | 3 | **6 Med** | Dir. Grid Analytics | 2027-02-28 | Open | RA §4 |
| R-05 | Data & pipeline | Stale or missing SCADA/AMI inputs | Pipeline failure, comms outage | Forecast on stale data → wrong dispatch | 3 | 4 | **12 High** | Pipeline job alerts (IT only) | Staleness check > 15 min → Fallback Mode A; data-quality dashboard in control room | Mitigate | 2 | 3 | **6 Med** | Mgr Data Engineering | 2026-10-31 | Complete | RA §7 |
| R-06 | Data & pipeline | Weather provider outage | Third-party availability | Degraded forecast | 3 | 3 | **9 Med** | Two commercial providers | Add NWS as third source; automatic failover | Mitigate | 1 | 3 | **3 Low** | Mgr Data Engineering | 2026-10-31 | Complete | RA §4 |
| R-07 | Data & pipeline | Training data contaminated by outage / curtailment periods | Historic load during outages & DR treated as true demand | Learned under-estimation of demand | 3 | 3 | **9 Med** | Manual exclusion (ad hoc) | Automated outage/DR event flagging & demand reconstruction | Mitigate | 2 | 3 | **6 Med** | Lead Data Scientist | 2026-12-15 | In progress | RA §4 |
| R-08 | Automation & control | Over-curtailment / repeated cycling of same premises | M3 over-estimates curtailment; DRA retries | Health risk to vulnerable customers; complaints | 3 | 4 | **12 High** | Aggregator default limits (inconsistent) | Per-premise caps enforced in integration layer; medical-baseline exclusion; event-size ceiling | Mitigate | 2 | 3 | **6 Med** | Mgr DR Programmes | 2026-10-15 | Complete | RA §5, IA |
| R-09 | Automation & control | DRA control-logic error dispatches events outside approved envelope | Envelope enforced only in application logic; no independent limiter | Mass curtailment; rebound; customer harm | 4 | 5 | **20 Crit** | Code review | Independent hard limiter (MW, duration, frequency) outside DRA; two-person rule for envelope changes; integration tests | Mitigate | 1 | 5 | **5 Low** | Dir. System Operations | 2026-10-31 | Complete | RA §4 |
| R-10 | Automation & control | Rebound ("snapback") peak when events end simultaneously | Synchronous release of thousands of thermostats | New peak exceeding forecast | 4 | 4 | **16 High** | None | Staggered release randomisation; rebound modelled in M3 | Mitigate | 3 | 4 | **12 High** | Mgr DR Programmes | 2027-03-31 | In progress — **residual accepted** | RA §4 |
| R-11 | Human oversight | Automation bias — operators rubber-stamp recommendations | 99.4% approval, 22 s median review | Errors pass unchallenged | 4 | 3 | **12 High** | Training | Prediction intervals & drivers on EMS; challenge prompts; seeded-error drills; override KPI | Mitigate | 3 | 3 | **9 Med** | Mgr Operations Training | 2026-12-31 | In progress | RA §5 |
| R-12 | Human oversight | Operators cannot cancel real-time events fast enough | Cancel function 3 screens deep; aggregator latency | Harm continues after detection | 3 | 4 | **12 High** | Phone escalation to aggregators | One-click "cancel all active events"; aggregator cancel SLA ≤ 60 s | Mitigate | 2 | 4 | **8 Med** | Dir. System Operations | 2026-11-15 | In progress | RA §4 |
| R-13 | Human oversight | Erosion of manual forecasting skill | Reliance on GridSense | Unable to operate in Fallback B | 3 | 3 | **9 Med** | — | Quarterly manual-forecast drills | Mitigate | 2 | 3 | **6 Med** | Mgr Operations Training | 2027-01-31 | Open | RA §4 |
| R-14 | Security | Manipulation / spoofing of weather input feed | Internet-facing third-party feed | False forecast drives automated DR | 4 | 5 | **20 Crit** | TLS | Multi-source consensus & divergence alarm; plausibility bounds; auto Fallback A; CIP-013 inclusion | Mitigate | 1 | 5 | **5 Low** | OT Security Manager | 2026-11-30 | In progress | RA §5 |
| R-15 | Security | Compromise of aggregator API enabling mass device control | Third-party credential compromise | Coordinated load manipulation | 3 | 5 | **15 High** | API keys | mTLS; signed commands; command-rate limits at aggregator; contract security clauses; joint IR plan | Mitigate / Transfer (partial) | 2 | 5 | **10 Med** | OT Security Manager | 2027-01-31 | In progress | RA §4 |
| R-16 | Security | Unauthorised model or configuration change via MLOps pipeline | Pipeline sits in IT, deploys into OT-adjacent environment; outside CIP scope | Tampered model in production | 4 | 5 | **20 Crit** | Git access control | Signed model artefacts; deployment approval in change mgmt; pipeline brought into ESP-adjacent controls; integrity check at load | Mitigate | 3 | 4 | **12 High** | CISO | 2027-02-28 | In progress — **residual accepted** | RA §4 |
| R-17 | Third party | Changes to cloud ML platform / libraries alter model behaviour | Upstream dependency updates | Silent performance change | 2 | 3 | **6 Med** | Pinned versions (partial) | Full dependency pinning; regression test on upgrade | Mitigate | 2 | 2 | **4 Low** | Lead Data Scientist | 2026-11-30 | In progress | RA §4 |
| R-18 | Governance | Model updates deployed without risk review | No AI change-classification process | Unassessed changes reach production | 3 | 3 | **9 Med** | General IT change mgmt | AI change classification; re-assessment triggers (RA §9) | Mitigate | 1 | 3 | **3 Low** | AI Risk Lead | 2026-10-31 | Complete | RA §9 |

## 2. Heat Map

**Inherent**

| L \ I | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| **5** | | | | | |
| **4** | | R-03 | R-11 | R-02, R-10 | R-01, R-09, R-14, R-16 |
| **3** | | | R-06, R-07, R-13, R-18 | R-04, R-05, R-08, R-12 | R-15 |
| **2** | | | R-17 | | |
| **1** | | | | | |

**Residual**

| L \ I | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| **5** | | | | | |
| **4** | | | | | |
| **3** | | R-03 | R-11 | R-02, R-10, R-16 | |
| **2** | | R-17 | R-04, R-05, R-07, R-08, R-13 | R-12 | R-01, R-15 |
| **1** | | | R-06, R-18 | | R-09, R-14 |

## 3. Summary

| Rating | Inherent | Residual |
|---|---|---|
| Critical | 4 | 0 |
| High | 8 | 3 |
| Medium | 6 | 10 |
| Low | 0 | 5 |
| **Total** | **18** | **18** |

## 4. Residual High Risks Requiring Acceptance
R-02, R-10, R-16 → see [Residual-Risk-Acceptance.md](Residual-Risk-Acceptance.md).
