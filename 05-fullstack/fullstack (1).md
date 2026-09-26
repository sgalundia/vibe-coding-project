# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

https://churn-whisperer-19v6.lovable.app

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| Profiles are created | profiles are created,  critical line defaults to 40 | Profiles are created automatically when someone signs up. People can see their own profile and their teammates'. Critical line defaults to 40. The database rejects any setup where it isn't below the at-risk line, and the at-risk line below the healthy line. Factor to churn driver links are filled in for all five factors, using the pairings you gave. Feedback sessions can be requested by any workspace member. The person who asked, or an admin, can update them. Intervention history is a permanent log. Nobody can edit or delete an entry once it's written. Experiment groups: an account can only be in one group per experiment. None of the existing sample accounts were in both. Current account health shows each account's l |
| Intervention History | _____ | Intervention history is a permanent log. Nobody can edit or delete an entry once it's written. Experiment groups: an account can only be in one group per experiment. None of the existing sample accounts were in both. Current account health shows each account's latest score. Only members of that workspace can see it. |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Sign-in page: people can sign in with email and password or with Google. New email sign-ups get a confirmation email first.
Screens need sign-in: anyone signed out who opens a screen is sent to the sign-in page. I checked this works.
Sign out: there's a new button in the navigation.
Demo workspace: everyone is added to it automatically the first time they sign in. It shows a "Demo data" badge in the navigation.
Access rules: members can view everything and log actions. Only owners and admins can change accounts, scores, factors, recommendations or experiments.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Data rule factor weighting to 100% | New database rule: the scoring factor weights must now add up to 100%. The Integration Plan asked for this, but nothing enforced it before. |
| Bad / malicious input | _____ | _____ |
| Failure / offline | _____ | _____ |

## Stress test results

_What you threw at it, and what held / broke._

In the stress test the Lovable app did not show error message when the
