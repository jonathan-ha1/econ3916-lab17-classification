# Classification: When 90% Accurate Is Worthless

## Objective
I wanted to show why accuracy is a misleading way to judge a classifier when one outcome is rare, and to pick a decision threshold based on what each type of error actually costs instead of using the default of 0.5.

## Methodology
- I worked with 6,000 simulated loan applications with a default rate of 9.6%, split them into training and test sets, and scored a baseline rule that approves everyone.
- I fitted a logistic regression on income, debt to income, and prior defaults, then compared its accuracy at the usual 0.5 threshold with the approve-everyone rule.
- I read the coefficients as both log-odds and odds ratios, and used the odds ratio for one more prior default to show how the same odds ratio leads to different probability changes depending on the starting probability.
- I traced precision and recall across six thresholds to see how moving the cutoff changes who gets declined.
- I priced the two errors, approving a loan that defaults and declining an applicant who would have repaid, and found the cost-minimising threshold on the training rows using cross-validated probabilities before pricing it once on the test rows.
- I wrote a small function, precision_recall_at(), that gives a classifier’s precision and recall at any threshold.

## Key Findings
- A rule that approves everyone is 90.4% accurate, and my logistic regression matched that exactly at the 0.5 threshold because it predicted 0 defaults, which shows that accuracy alone can make a useless model look good.
- The odds ratio for one more prior default was 1.63, but what that means in probability depends heavily on where an applicant starts, so reporting probability changes is clearer for a non-technical audience.
- The cost-minimising threshold I found on the training rows was 0.121, very close to the closed-form threshold of 0.118, and far below the default of 0.5.
- At my chosen threshold the expected cost on the test rows was 944 dollars per applicant, compared with 1,147 dollars per applicant at 0.5.
- At the closed-form threshold, my function gave a precision of 0.181 and a recall of 0.477, meaning the model caught almost half of the applicants who would default.
