# Module 2 Lab 1: Frame a Business Problem as a Machine Learning Problem

Name: Daz Arnaud

Organization: Santa Monica College tutoring centers

GitHub portfolio repository URL: https://github.com/darnaud-ops/new/tree/claude/claude-md-docs-0a6ucv/CS79F

Date started: November 4, 2026

## Step 1: Business request

Santa Monica College offers tutoring appointments through its tutoring centers. I propose identifying appointments at risk of a no-show so staff can send one extra reminder. This is a proposed use case, not a claim that SMC currently runs this model.

Source: https://www.smc.edu/student-support/academic-support/tutoring-centers/

## Prediction 1: Threshold

I predicted a threshold near 0.05 before calculating. My reason was, "A missed tutoring session costs more than an extra reminder."

## Step 2: Machine learning problem

Type: Binary classification.

Input: One scheduled tutoring appointment, evaluated 24 hours before the session.

Features: Lead time between booking and session, day of week, session time, delivery mode, subject area, prior no-show count, prior attendance count, and confirmation status at the time of prediction.

Output: Probability that the student will miss the appointment without advance cancellation.

Decision: If the probability exceeds the cost-based threshold, send one extra reminder. Do not cancel an appointment or deny tutoring access based on this score.

## Step 3: Threshold from money

Cost of an unnecessary reminder, false positive: Estimated $0.50 in service and staff time.

Cost of a missed session, false negative: Estimated $25 in scheduled tutor time that another student might have used.

These are planning estimates. They are not SMC accounting figures.

Act when p x $25 > $0.50. The threshold is p > $0.50 / $25 = 0.02.

My predicted 0.05 is higher than the computed 0.02. Both are far below 0.5.

## Step 4: Metric

I would optimize recall for genuine no-shows. A missed session costs an estimated 50 times as much as an extra reminder, so false negatives drive the decision. I would also monitor the number of extra reminders to avoid wasting staff time.

## Step 5: Data and weakest point

I would need one to two academic years of appointment records from SMC's scheduling system. The records must include booking timestamps, reminder history, cancellation timestamps, and actual attendance status.

The weakest point is how staff record attendance across tutoring centers. A student who cancels in advance must not be labeled as a no-show.

## Prediction 2: Deciding property

I predicted consistency. My reason was, "Consistency, because identical appointment details should get the same decision."

## Step 6: Foundation model

I would start with a tabular classifier instead of a foundation model. Structured appointment fields and a single probability output favor a repeatable classifier. Consistency was the deciding property I predicted, and it remains the deciding property after the comparison. I would test a foundation model only if unstructured notes or messages became essential inputs, after a privacy review.

## Reflection draft for review

These answers were drafted with AI assistance under the accommodation Daz described. The assignment page marks reflections as student-authored, so Daz should confirm the accommodation covers this section and revise the wording before submission.

1. I predicted 0.05 because a missed session costs more than an extra reminder. Using my estimates, the threshold is 0.50 divided by 25, or 0.02. My guess was more conservative than the cost calculation. At a 0.03 no-show risk, the expected missed-session cost is 0.75, which exceeds the 0.50 reminder cost, so the extra reminder is worthwhile.

2. If one tutoring center records late cancellations as no-shows and another does not, the model learns two different meanings for the same label. A random test split from those mixed records might show a strong score because the same recording habits appear in both training and test data. In a center with different habits, the model might send reminders to students who already canceled. I would standardize the label and test on a later term and a separate tutoring center before using the score.

3. A 0.5 threshold would skip reminders when a student's no-show risk is between 0.02 and 0.5. At a 0.10 risk, the expected loss is 2.50, while the extra reminder costs only 0.50 under my estimates.

## AI use disclosure

I used AI assistants to structure the framing, review the SMC tutoring page, check the arithmetic, and draft reflection answers. I supplied both predictions and their reasons before the calculations. I will review the draft and confirm with my instructor how my approved accommodation applies to the reflection section.
