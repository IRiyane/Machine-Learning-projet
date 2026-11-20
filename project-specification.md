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
