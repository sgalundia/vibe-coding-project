# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Share dashboard with with other users 

### Step 1: Expand, build new screens in a strict sequence
```
Build the next phase of this app in a strict sequence:
1. Add a screen " Recommendations" within the section of the dashboard of "Why scores are low"?. Recommendations should be actionable for the users. 
2. Navigation: Within the module of "why scores are low"?: user can see side by side the actions to take to proactively resolve the high risk accounts. Example of Actions: contacts accounts to re-login in case they forgot. Example 2 complete onboarding.


Build these in order so {{screen A}} is the anchor for {{screen B}}.
```

### Step 2: Behavior, hard-code the states
```
Apply the following logic constraints to the {{screen}} flow:
- If no recommendations is present, show the empty state: " no recommendations".
- For error states where recommendations are not actionable, guide user to have a customer feedback session. 
Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine, one surgical polish
```
The error  needs a professional {{style}} polish.
1. The recommendations should be highlighted in a seperate color (blue)  for user to see. 
2. Resize the headers of the seciton of why scores are low to " Reason scores are low and what actions to take". 

Don't change anything else in the project or touch the underlying logic.
```

## Reusable techniques learned

- chain of thought and how to write the prompt based on the user experienece and how they stepwide the worflow.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

The guradrails of errors or no recommendations are not reflected for user to validate. 
