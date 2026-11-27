# Week 3 Topic-Based Evaluation and Ensemble Learning

November 27, 2026

**Team** IDRISSI KAITOUNI Riyane, Kang Nathan, Goutodier Lucas

## Objective

We tested whether a classifier trained on English essays can detect AI-generated writing when the writing prompt is new. Week 2 used a 10,000-essay sample for exploration and a majority-class baseline. Week 3 uses the full cleaned DAIGT v2 corpus so we can reserve whole topics and train on the remaining essays. On the three unseen topics, soft voting achieved the highest AI-class F1 of the four models tested.

## Data preparation and topic split

The original file contains 44,868 essays. We normalized Unicode, letter case, and whitespace to identify duplicates, then removed all 12 rows in the six duplicate groups. The classifiers use the original text of the remaining 44,856 essays.

The dataset provides each writing prompt in the `prompt_name` column. With a fixed seed of 42, `GroupShuffleSplit` reserved all essays from three prompts for the unseen-topic test. The prompts were “A Cowboy Who Rode the Waves”, “Grades for extracurricular activities”, and “Phones and driving”. We sampled the familiar-topic test from the other prompts, matching the human and AI counts in the unseen test. The two tests therefore have the same class balance.

| Split | Human essays | AI essays | Total essays | Topics |
| --- | ---: | ---: | ---: | ---: |
| Training and cross-validation | 19,035 | 14,639 | 33,674 | 12 |
| Familiar-topic test | 4,162 | 1,429 | 5,591 | 12 |
| Unseen-topic test | 4,162 | 1,429 | 5,591 | 3 |

The notebook checks that each essay belongs to a single split and that all three unseen prompts are absent from the training data and familiar-topic test.

## Model training and hyperparameter search

The majority classifier retained from Week 2 serves as a reference. For the two text classifiers, word-level TF-IDF turns essays into numerical features for logistic regression and Complement Naive Bayes. The pipeline uses up to 20,000 terms and keeps terms seen in at least two training essays.

`GridSearchCV` selects settings using the mean F1 for the AI class across three folds grouped by `prompt_name`. All essays from a validation prompt stay together in that fold. The TF-IDF pipeline learns its vocabulary and term weights from the training portion of each fold. Both test sets are reserved for final evaluation.

The search selected `C=4.0` for logistic regression (validation F1 of 0.874) and `alpha=1.0` for Complement Naive Bayes (0.861). The soft-voting ensemble averages the two tuned models’ AI probabilities.

## Test results

AI-class F1 combines precision and recall for AI-generated essays. The false-positive rate is the percentage of human essays incorrectly flagged as AI. The counts in parentheses show how many of the 4,162 human essays were flagged on each test.

| Model | Familiar F1 (AI) | Familiar false positives | Unseen F1 (AI) | Unseen false positives |
| --- | ---: | ---: | ---: | ---: |
| Majority baseline | 0.000 | 0.00% (0) | 0.000 | 0.00% (0) |
| Logistic regression | 0.992 | 0.17% (7) | 0.896 | 3.10% (129) |
| Complement Naive Bayes | 0.930 | 2.96% (123) | 0.857 | 0.62% (26) |
| Soft voting | 0.977 | 0.62% (26) | 0.903 | 0.62% (26) |

## Interpretation

All three text classifiers scored lower on unseen topics. Logistic regression achieved the highest F1 on familiar topics (0.992). Its F1 fell to 0.896 on unseen topics.

Soft voting reached 0.903 F1 on the unseen test and flagged 26 human essays, compared with 129 for logistic regression. It detected 1,197 of the 1,429 AI essays, compared with 1,264 for logistic regression. The gain in F1 came with fewer false alerts and lower AI recall. Complement Naive Bayes also flagged 26 human essays and detected 1,092 AI essays.

The results describe generalization to these three writing prompts within DAIGT v2. The notebook contains the confusion matrices and comparison charts. [Week 3 notebook](../notebooks/Week3.ipynb)

## Week 4 Experiments

We will compare three approaches with the Week 3 soft-voting baseline.

- Tune the decision threshold of the soft-voting model.
- Test logistic regression with character-level TF-IDF using sequences of three to five characters.
- Fine-tune DistilBERT to classify essays as human-written or AI-generated.

We will use validation folds grouped by writing prompt to choose settings. We will compare AI-class F1, recall, and the percentage of human essays flagged as AI. The goal is to detect more AI essays while keeping false alerts low.
