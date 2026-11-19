# EEG motor imagery: left fist vs right fist

## Goal
Predict from EEG alone whether a person imagines moving their left or
right fist, using a deliberately simple pipeline as a baseline.
Data: PhysioNet EEG Motor Movement/Imagery dataset, subjects 1 to 10,
runs 4, 8 and 12 (about 45 left/right trials per subject).

## Pipeline
1. Load the 3 recordings per subject with MNE-Python and join them
2. Band-pass filter 1-40 Hz to remove drift and noise
3. Read the annotations and cut the signal into 4-second epochs
4. Keep the 8-30 Hz band (mu and beta rhythms, linked to movement)
5. Summarize each epoch as the log-variance of 3 channels (C3, Cz, C4),
   placed over the motor areas
6. Standardize the features, then classify with logistic regression
7. Evaluate each subject with 5-fold cross-validation repeated 10 times

## Results
Mean accuracy over 5-fold cross-validation repeated 10 times (chance = 0.5).
Subject 1 was used to choose the methods; subjects 2-10 were held out.

| Method | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Baseline (3 channels) | 0.63 | 0.61 | 0.38 | 0.78 | 0.46 | 0.44 | 0.76 | 0.55 | 0.62 | 0.67 |
| CSP (4 components, 64 channels) | 0.75 | 0.72 | 0.60 | 0.53 | 0.54 | 0.43 | 0.97 | 0.46 | 0.40 | 0.49 |

## Comparison with CSP
Common Spatial Patterns (4 components, chosen on subject 1) learn spatial
filters from all 64 channels instead of using 3 fixed channels; the rest of
the pipeline (standardization, logistic regression, evaluation) is the same.
On subjects 2-10, CSP gives a mean accuracy of about 0.57, slightly below
the baseline (about 0.59). CSP scored higher for 4 of the 9 subjects
(2, 3, 5 and 7) and lower for the other 5. The difference between the means
is small compared with the variation between subjects, so this experiment
does not show that either method is better.

## Conclusion
The subjects 2-10 had a mean accuracy of about 0.59 with the baseline.
Five subjects scored 0.6 or higher, while three scored at or below 0.5,
so the baseline generally doesn't work for everybody. CSP did not improve
the mean accuracy. With only about 45 trials per subject, individual scores
are uncertain, and neither method is shown to work in general.