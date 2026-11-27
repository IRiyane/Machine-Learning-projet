# Projet ML

## Lab 4 - Week1

### Write the Project Specification :

#### 1. Project Title: A concise and informative title.

- Detecting Essays Written by AI on New Topics

#### 2. Team Members: Names of all group members.

- IDRISSI KAITOUNI Riyane, Kang Nathan, Goutodier Lucas

#### 3. Context and Motivation: Description of the application domain and the motivation behind the project.

- AI tools can produce essays that look like student writing. We want to see how well a text classifier can tell the two apart

#### 4. Problem Statement: The problem you aim to solve and the main research question.

- Can a model distinguish human-written and AI-generated English essays when the essay topic was absent from its training data?

#### 5. Project Objectives: The expected outcomes and measurable objectives.

- Build a baseline and compare it with one other classifier. For each model, measure F1 for the AI class and the percentage of human essays flagged as AI. Compare those two scores on familiar and new topics.

#### 6. Dataset Description: Data source, size, features, target variable (if applicable), and potential limitations.

- Source : DAIGT v2 (kaggle)
- Size : ≈ 45.000 essays, 61 percent human and 39 percent AI generated
- Feature : Stylistic features (around 40)
- Target variable. The label, human or AI.
- Limitations : The LLMs date from 2023 and newer models may write differently.

#### 7. Machine Learning Task: Classification, regression, clustering, or another relevant task.

- This is binary text classification. The model reads the essay and predicts human-written (0) or AI-generated (1)

#### 8. Baseline Strategy: Selected baseline algorithm and justification.

- Use TF-IDF with logistic regression. TF-IDF gives more weight to words that are common in an essay and less common across the collection. This baseline is quick to train and gives us a simple point of comparison.
- The second model is Complement Naive Bayes, a classifier designed for text

#### 9. Evaluation Strategy: Data splitting methodology, evaluation metrics, and validation approach.

- Remove duplicate essays before splitting. Set aside all essays from three topics for the new-topic test. From the remaining topics, draw a random test with the same number of human and AI essays as in the new-topic test. Train both models on the remaining essays.
- Tune the settings inside the training set using three validation folds grouped by topic. Fit TF-IDF within each fold. On both final tests, report F1 for the AI class and the false positive rate, which is the share of human essays incorrectly flagged as AI.

#### 10. Initial Work Plan: Main tasks and their distribution among team members.

- Week 1  The data lead checks the labels, topics, and duplicate texts.
- Week 2  The modeling lead builds the baseline. The evaluation lead sets up the folds and metrics.
- Week 3  The modeling lead trains the second classifier. The evaluation lead compares the scores and checks the errors.
- Week 4  Everyone reviews the results, writes the report, and prepares the presentation

## Lab5 - Week 2

### Why “on New Topics” on the project title :

A standard train-test split on a single topic measures how well the detector classifies new essays on that topic. This is enough if the detector will only be used for that topic. On familiar topics, the model may pick up vocabulary that helps it classify essays but may be less useful when the writing prompt changes.

We train one model and evaluate it on two sets of essays it has never seen. The first contains new essays on topics represented in training. The second contains essays from three topics held out entirely. For example, the model can train on essays about cell phones at school and then be tested on essays about grades for extracurricular activities. Comparing the two tests shows how well the detector generalizes to new writing prompts.

## Lab 6 Week 3 Evaluation Results

We evaluated the majority baseline, TF-IDF with logistic regression, TF-IDF with Complement Naive Bayes, and a soft vote of the two tuned text models. Each test contains 4,162 human essays and 1,429 AI essays. The familiar-topic test contains new essays on topics represented in training. The unseen-topic test contains essays from three prompts reserved entirely for testing.

### Model comparison

F1 combines precision and recall for the AI class. The false-positive rate is the share of human essays flagged as AI. The counts in parentheses give the number of false alerts.

| Model | Familiar F1 (AI) | Familiar false positives | Unseen F1 (AI) | Unseen false positives |
| --- | ---: | ---: | ---: | ---: |
| Majority baseline | 0.000 | 0.00% (0) | 0.000 | 0.00% (0) |
| Logistic regression | 0.992 | 0.17% (7) | 0.896 | 3.10% (129) |
| Complement Naive Bayes | 0.930 | 2.96% (123) | 0.857 | 0.62% (26) |
| Soft voting | 0.977 | 0.62% (26) | 0.903 | 0.62% (26) |

[Figure 1. AI-class F1 and human false positives on the two tests.](Project-Report.pdf#page=2)

### Discussion

All three text models made more errors on unseen topics. The error counts rose from 24 to 294 for logistic regression, from 202 to 363 for Complement Naive Bayes, and from 65 to 258 for soft voting. All three had lower F1 on unseen topics. The majority baseline always predicted the human class.

For soft voting, false alerts totalled 26 on each test and missed AI essays rose from 39 to 232 (Figures 2 and 3). On unseen topics, soft voting had the highest F1 (0.903) and made 26 false alerts (0.62%), compared with 129 (3.10%) for logistic regression. Logistic regression detected 1,264 of 1,429 AI essays, soft voting detected 1,197, and Complement Naive Bayes detected 1,092. Complement Naive Bayes also made 26 false alerts.

These results describe the three held-out prompts in DAIGT v2. [Week 3 notebook](../notebooks/Week3.ipynb)

### Confusion matrices

[Figure 2. Confusion matrices on familiar topics. Rows show the true class and columns show the predicted class.](Project-Report.pdf#page=3)

[Figure 3. Confusion matrices on unseen topics. The upper-right cells count human essays wrongly flagged as AI.](Project-Report.pdf#page=3)

### Week 4 follow-up

We will test decision thresholds, character-level TF-IDF, and DistilBERT to see whether they improve AI detection while keeping the human false-positive rate low. [Week 4 experiment plan](Week%203.%20Topic-Based%20Evaluation%20and%20Ensemble%20Learning.md#week-4-experiments)
