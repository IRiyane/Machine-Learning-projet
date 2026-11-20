# Project discussion log

The submission is kept in [project-specification.md](../report/project-specification.md). This file records the project discussions, the reasoning behind decisions, and questions that remain open.

## 2026-11-20 — Week 2 requirements and current progress

The Lab 5 milestone calls for a cleaned dataset, an implemented baseline, and initial evaluation results. It accounts for 25% of the project grade. The project deliverables are in English.

The local DAIGT V2 archive contains 44,868 English essays across 15 topics. The fields checked contain no empty values. The proposed cleaning removes 12 duplicate rows, leaving 44,856 essays. The project script already contains logic for data checks, splitting, training, and evaluation. The final notebook and baseline scores had yet to be verified when we reviewed the folder.

## 2026-11-20 — Evaluation protocol and baseline

The agreed baseline is TF-IDF with logistic regression. The specification calls for three validation folds grouped by topic within the training set, with TF-IDF fitted inside each fold. On each test set, report F1 for the AI class, the share of human essays incorrectly flagged as AI, and the confusion matrix. The work plan schedules the comparison with Complement Naive Bayes for Week 3.

We found a discrepancy in the test split. The script gives both tests the same total size, but the test on familiar topics contains about 41% AI essays compared with about 26% for the held-out topics. The specification calls for matching both class counts. With the current held-out topics, the target for each test is 4,162 human essays and 1,429 AI essays. Correcting the split and verifying the baseline results remain open.

## 2026-11-20 — Course policy on AI assistance

We asked why the error audit mentions AI and whether the course explicitly permits an AI agent. The audit concerns errors in AI-assisted project work, such as proposed code or methodology. False positives from the essay detector belong in the model evaluation.

The Lab 4 Project Guidelines, pages 19 and 22, explicitly allow generative AI tools as engineering aids under student oversight. We found no explicit mention of Codex or code agents in the materials we checked. We interpreted the policy as allowing Codex to propose code that the group checks, tests, and can defend. The guidelines link to Moodle, which we could not access. The Lab 5 handout asks for 4–6 significant prompts and at least four concrete flaws caught in AI outputs, with at least one prompt and one flaw per student. The split discrepancy could be an audit entry if the script was AI-generated and the group verifies and documents the fix.

## 2026-11-20 — Why test on new topics?

We asked whether splitting essays from a single topic into training and test sets would be enough. We kept both tests because they answer different questions. The familiar-topic test measures performance on new essays, and the held-out-topic test measures performance on new writing prompts. For example, the detector can train on essays about cell phones at school and later be tested on a held-out topic such as grades for extracurricular activities. We added the explanation to the Week 2 section of the project specification.

## 2026-11-20 — Working documents

We confirmed that the Google Doc could be read and edited, added the Week 2 explanation there, and exported it to the local Git repository. We replaced the Word copy in the repository with Markdown so it would be easier to read in Codex and on GitHub. The team asked to keep the specification for submission and record ongoing project discussions in this separate file.

## 2026-11-20 — Week 2 submission checklist

The local repository contains the specification and discussion log. The notebook builder is still in the course workspace. For the Week 2 submission, move the reproducible baseline workflow into the project repository and complete these checks in order.

1. Export the cleaned dataset and summarize missing values, duplicate removal, text and metadata fields, and the human/AI label distribution.
2. Match the class counts across the two held-out tests. Check essay separation, prompt separation for the unseen-topic test, and the three grouped validation folds inside training.
3. Fit the TF-IDF and logistic regression baseline using training folds and record the validation and test results.
4. Report AI-class F1, the false positive rate on human essays, and the confusion matrix on both tests. Explain what changes on unseen topics and justify the baseline and split.

## 2026-11-20 — Moodle submission layout

We moved the specification to `report/` and the working discussion log to `ai_governance/`. We placed the three required governance files at the repository root as shown in the Lab 5 diagram and left their entries open for the team to document. The project notebooks will go in `notebooks/`. The final PDF and pinned dependencies are still pending.

## 2026-11-20 — Repository guide for AI agents

We added a root `AGENTS.md` to help new agents locate the submission and discussion log, pick up the Week 2 baseline, verify the evaluation protocol, and keep the required governance records.

## 2026-11-20 — Scope of the agent guide

We shortened the agent guide to cover how to work in the repository. The project specification provides the methodology, and this log records decisions and progress.

## 2026-11-20 — Recording substantial prompts and outputs

Nathan asked the agent to record each substantial project prompt with its output. [AGENTS.md](../AGENTS.md) now tells the agent to update the applicable logs before its final response. The Week 2 notebook review is the first entry in the [prompt log](../PROMPT_LOG.md#2026-11-20--week-2-notebook-review).

## 2026-11-20 — Class stratification and the new-topic test

A teammate noted that the Week 2 split preserves the minority-class share. The exported files have 39% AI essays in each set (3,120 of 8,000 in training and 780 of 2,000 in testing) and share all 15 topics. We found no identical essay in the two files. The current split therefore provides a test set for familiar topics. The notebook has prepared that set and so far reports cross-validation results from the training set. [Week 2 notebook](../notebooks/Week2.ipynb)

We want to evaluate the same model in two situations.

- **Familiar topics.** The model trains on some essays about cell phones at school. We test it on other essays about cell phones at school that it has never seen.
- **Unseen topics.** If we hold out driverless cars, every essay about driverless cars goes into the unseen-topic test. The model encounters that topic for the first time when we test it.

To build both final tests, reserve all essays from three topics before creating the training and validation sets. Then sample the familiar-topic test from the remaining topics, matching the number of human essays and the number of AI essays in the unseen-topic test. Use the leftover essays for training and validation. Evaluate the same fitted model on both tests and compare the AI-class F1 score and the percentage of human essays flagged as AI. This comparison shows how performance changes when the topic is new. [Evaluation protocol](../report/project-specification.md#9-evaluation-strategy-data-splitting-methodology-evaluation-metrics-and-validation-approach) · [Prompt and response](../PROMPT_LOG.md#2026-11-20--class-proportions-and-topic-holdout-in-week-2)

## 2026-11-20 — Week 2 submission handoff

We checked the current notebook against the Week 2 deliverables. The existing notebook explores the data, exports an 8,000/2,000 split from 10,000 essays, and evaluates a majority-class reference in cross-validation. To complete the submission, justify the dataset scope, regenerate the splits under the agreed topic protocol, and train and evaluate TF-IDF with logistic regression on both held-out tests. Submit the runnable notebook with its outputs, the cleaned data, and pinned dependencies. [Prompt and response](../PROMPT_LOG.md#2026-11-20--week-2-submission-requirements)

## 2026-11-20 — Week 2 notebook verification

GitHub main and local HEAD both point to `c6ce23f`. We replayed all eight code cells and reproduced both CSVs byte for byte. The confusion matrix added in `db0f11e` uses out-of-fold predictions from the majority-class model on the training essays. It contains 4,880 human essays correctly classified and 3,120 AI essays classified as human. The run reproduced 0.61 accuracy and 0.00 F1 for the AI class. The familiar-topic and unseen-topic evaluation of TF-IDF with logistic regression is still open, as are the dependency pins. [Prompt and response](../PROMPT_LOG.md#2026-11-20--verify-the-current-week-2-work)

## 2026-11-20 — Completion by Week 2 deliverable

The two exported subsets have zero empty fields, zero normalized duplicate essays within each file, and zero normalized text overlap between them. The full raw dataset has six groups of normalized duplicates covering 12 rows. The existing notebook checks for exact duplicates and reports initial cross-validation results from an implemented majority-class baseline. The explicit cleaning step, the two topic-based tests, and the TF-IDF logistic regression experiment remain open under the project specification. [Prompt and response](../PROMPT_LOG.md#2026-11-20--status-of-the-three-week-2-deliverables)

## 2026-11-20 — AI model attribution in the prompt log

We added a model field to each prompt entry and updated the repository instructions to record the assistant, model, and exact variant when known. We recorded the known Codex model family for entries from this chat and marked the two earlier models as not recorded. [Prompt and response](../PROMPT_LOG.md#2026-11-20--recording-the-ai-model)
