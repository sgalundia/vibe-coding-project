# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

This is a high-fidelity, frontend-only prototype for testing whether a transparent account health score helps Customer Success teams identify and save at-risk B2B SaaS accounts during their first 90 days. It has four screens: the executive health dashboard (/), retention hypothesis (/hypothesis), historical model backtest (/backtest), and intervention experiment (/experiment). The interactions and calculations are functional: account scores are calculated from five weighted factors, accounts are ranked and filterable, recommendations expand inline, affected-account links open the matching account, the backtest cutoff recomputes model metrics, and experiment summaries are derived from cohort rows. However, every account, historical outcome, intervention, and recommendation is mocked in source files. There is no backend, authentication, persistence, telemetry ingestion, CRM integration, notification delivery, or production analytics. Treat this as a product-validation artifact and a clean frontend foundation—not a deployable retention system.

## Architecture (plain language)

- **Frontend:** Framework: React 19 with TanStack Start and TanStack Router; Vite is the development and build tool. Styling: Tailwind CSS v4 using semantic tokens in src/styles.css; shared controls are in src/components/ui/. Charts and icons: Recharts and Lucide React. Routing: File-based routes in src/routes/; the generated route tree must not be edited. Shared shell: src/routes/__root.tsx supplies the document shell, error/not-found handling, Query Client, and nested-route outlet. Feature organization: Product code is grouped under src/features/: account-health/ — score definition, executive summary, at-risk accounts, score calculations. churn-rationale/ — churn-driver distribution, factor analysis, recommendations, account-analysis tabs. model-validation/ — historical backtest and kill-criteria calculations. intervention-experiment/ — intervention/control cohorts and outcome summaries. retention-hypothesis/ — hypothesis and success/stop criteria. navigation/ — shared screen navigation. Within each feature: components/ displays and handles local UI state, data/ contains mocked records and fixed product constants, and model/ contains calculations and derived views.
- **Backend / data:** There is no backend. No database or external service is connected, and no network request is needed to populate the screens. Data is imported into the browser bundle from TypeScript modules:  src/features/account-health/data/account-health.data.ts — 20 mocked accounts, five signal definitions, and score-band metadata. src/features/churn-rationale/data/churn-rationale.data.ts — six churn drivers, static recommendations, factor-to-driver mappings, and next steps. src/features/model-validation/data/model-validation.data.ts — 120 deterministic historical records generated from seed 20250912; 36 are marked churned and 84 retained. src/features/intervention-experiment/data/intervention-experiment.data.ts — two fabricated ten-account cohorts plus the 30% baseline and 20% target. The calculations are real but operate only on mock data. The account-health model computes a weighted score from product usage (30%), onboarding milestones (25%), seat activation (20%), support and sentiment (15%), and sponsor engagement (10%). Risk bands are Critical 0–39, At risk 40–59, Watch 60–74, and Healthy 75–100. The validation model recomputes recall, false-positive rate, precision, confusion-matrix counts, and the Ship/Rebuild verdict from the selected cutoff. The committed kill criteria are at least 65% churn recall and no more than 25% false positives.
- **Key flows:** Portfolio review: / loads the mock portfolio, calculates all scores and issue counts, then shows executive metrics, score definitions, and churn-driver distribution.
Investigate low scores: In Account Analysis, the user opens a factor recommendation or selects an affected account. Selecting an account changes to the At-risk accounts tab, scrolls to the list, and expands that account’s score calculation.
Prioritize accounts: The account list starts lowest score first, supports text and risk-band filters, and exposes derived issue counts and recommendations.
Log an intervention: The control is present for hypothesis testing, but it changes no durable state and writes nothing.
Validate the model: /backtest applies the selected 50–75 cutoff to the generated historical rows and updates all model metrics and the Ship/Rebuild verdict immediately.
Compare outcomes: /experiment calculates save rates and retained/lost MRR from static intervention and control cohorts.
Review the hypothesis: /hypothesis presents the business hypothesis, baseline, target, historical summary, kill criteria, and default verdict.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Feature-based structure with display, mock data, and calculation logic separated. | solid | _____ |
| All customer and outcome data is fabricated and ships in the client bundle. | rough | _____ |

## Risks & assumptions for the team

Product and model risks
Unvalidated predictive power: The score weights, thresholds, and mock backtest may not generalize to real customers. The model must be re-run on leakage-safe historical data before operational use.
False-positive workload: A cutoff that improves recall can overwhelm CS. The ≤25% false-positive limit assumes that level is workable; CS capacity must validate it.
Data leakage: Production backtests must use only signals available before churn or renewal—not facts learned afterward.
Causal overclaiming: A difference between these fabricated cohorts proves nothing. A real experiment needs matching or randomization, stopping rules, sample-size planning, and treatment-contamination controls.
Control-group ethics: Withholding intervention from known-risk accounts may be commercially or ethically unacceptable. Leadership must define guardrails.
Score gaming and trust: CSMs need source timestamps, factor lineage, explanations, and score history before acting on the number.
Recommendation safety: Static plays may be wrong for a segment or account. Production actions need eligibility rules, ownership, escalation, and an explicit non-actionable path.

## How to run it

```
Product and model risks
Unvalidated predictive power: The score weights, thresholds, and mock backtest may not generalize to real customers. The model must be re-run on leakage-safe historical data before operational use.
False-positive workload: A cutoff that improves recall can overwhelm CS. The ≤25% false-positive limit assumes that level is workable; CS capacity must validate it.
Data leakage: Production backtests must use only signals available before churn or renewal—not facts learned afterward.
Causal overclaiming: A difference between these fabricated cohorts proves nothing. A real experiment needs matching or randomization, stopping rules, sample-size planning, and treatment-contamination controls.
Control-group ethics: Withholding intervention from known-risk accounts may be commercially or ethically unacceptable. Leadership must define guardrails.
Score gaming and trust: CSMs need source timestamps, factor lineage, explanations, and score history before acting on the number.
Recommendation safety: Static plays may be wrong for a segment or account. Production actions need eligibility rules, ownership, escalation, and an explicit non-actionable path.
```
