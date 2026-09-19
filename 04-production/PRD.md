# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

30% of new B2B SaaS accounts churn within 90 days because CS teams lack an early-warning signal; surfacing a composite health score with churn-risk signals enables earlier, proactive intervention.



Includes baseline (30% 90-day drop) and target (trend to ~20% over 1-2 quarters).

## Users & jobs

- **Primary user:** Customer Success Manager (CSM)
- **Job to be done:** spot at-risk Spot accounts early, understand why a score is low, take/schedule the recommended action, log an intervention, validate the model before rollout.

## Scope

- **In:** Everything currently on screen across the four routes (executive summary, score definition, churn-driver chart, churn rationale with inline recommendations, at-risk accounts with issue counts + log intervention, backtest screen, intervention experiment screen, hypothesis/kill-criteria screen).
- **Out (explicitly):** Real account data integration, live CRM/ticketing connections, authentication, email/reminder sending, actual intervention logging persistence, model retraining.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Score computed from five weighted factors, accounts ranked lowest to highest | Must | Compute scores of at risk to churn accounts using the 5 factors related to onboarding, engagment or activity , onboarding, tickets filled and workflow completed |
| 2 | "Why scores are low" with per-factor rationale, inline recommendations, clickable affected accounts that deep-link to the expanded account | Must | Ability for user to view rationale related to why scores are low from hgest to lowest risk with inline recommendations and actionable next steps for each accounts will help reduce churn and increase retention. |

## Data & events

_What gets stored, what gets tracked._

Honest labeling: ALL data is sample/mocked — churn-data.ts (20 accounts, signals, drivers, playbook), backtest-data.ts (120 seeded historical accounts, cohorts). No backend, no persistence.
Events implied but not yet real: score computed (derived client-side), intervention logged, recommendation scheduled, feedback session planned.
Notes what a production version would need (event stream, account telemetry source, intervention log store).

## Open questions

1. Real data source and refresh cadence?
2. Who owns the intervention SLA? 
3. Whether kill criteria thresholds (65% recall / 25% FPR) hold on real cohorts? 
4. How "Log intervention" persists and syncs to CRM? 
5. Notification channel for alerts; what happens to the control-group ethics at scale?
