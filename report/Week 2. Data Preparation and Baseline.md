# Week 2. Data Preparation and Baseline

Nov 20, 2026

Project: Detecting AI-Generated Essays with Stylistic Features and Feature Selection

Team: IDRISSI KAITOUNI Riyane, Kang Nathan, Goutodier Lucas

1. ## &#32;Objective

The goal of this week was to explore, clean and preprocess the dataset, define the training, validation and test sets, and finally implement a first baseline model, which gives us a reference to evaluate our models in the next weeks.

2. ## Analyze features, missing values and target distribution.

We used the DAIGT v2 dataset from Kaggle. It contains exactly 44,868 essays with 5 columns (text, label, prompt\_name, source, RDizzl3\_seven). The text column is the essay, label is the target (0 if human, 1 if AI), prompt\_name is the topic, source is the origin of the text, and RDizzl3\_seven is a column from the original competition that we don't use.

In terms of missing values and duplicates, there are no missing values and no duplicated texts, probably because the creator of the dataset already cleaned it.

For the target distribution, the dataset is imbalanced, with 27,371 human essays (61%) and 17,497 AI essays (39%). This is important for the choice of metrics, because imbalanced datasets are harder to analyse.

For the sources practically all the human essays come from the Persuade corpus and the rest come from the official competition essays. The AI essays come from 15 sources which correspond to 7 model families (Mistral, Llama, ChatGPT, Claude, PaLM, Falcon...).

We see in our code that human essays are longer on average (418 words vs 329 words) and their length varies much more (standard deviation of 189 vs 94). AI models produce essays of quite regular length. So we can deduce for the future that length could be a great clue to separate the two classes.

3. ## Training, validation and test sets

To ease our work and reduce the computation time of the training, we took a random sample of 10,000 essays. It is of course stratified on the label, so it keeps the same proportion of human and AI essays (61/39) in this sample. Then we split this sample into a training set (80%, so 8,000 essays) and a test set, also stratified to keep the same imbalance between the training and testing sets, meaning both sets have 61% human and 39% AI essays. The test set is saved in a csv and will be used only once at the end of the project, to get a final score for our model.                                                                                                We use a 5-fold stratified cross-validation on the training set. It works by splitting the training set into 5 parts of the same size, and each part is used once for validation while the model is trained on the 4 other parts. The final score is the mean of the 5 scores. This generally gives a better and more reliable estimate.

4. ## Evaluation metrics

Because the classes are imbalanced, accuracy alone is not enough. For example, a model that always predicts human gets 61% accuracy. So we use accuracy, precision, recall, F1-score and ROC-AUC, and we look at the confusion matrix. All of that gives us a clear view of the performance of our model.                                                                                                                                                       In our context we decided that a false positive (predicting AI when the essay is actually human) is worse than a false negative, because it could unfairly punish a student who really wrote the essay. So we give more importance to the precision of the AI class, while of course not neglecting the recall.

5. ## Baseline

The baseline that we did is a dummy classifier that always predicts the most frequent class, so human in our case. It doesn't read the text, and it gives the minimum score that any real model must beat, so it helps us check that our future models really learn something from the texts.

In terms of results, it gets 61% accuracy, but 0 for precision, recall and F1, and a ROC-AUC of 0.5. The confusion matrix shows that all the human essays (4,880) are correctly classified, but all the AI essays (3,120) are missed, which is logical since the model never predicts AI. The AUC of 0.5 shows it does not distinguish the two classes at all, because it gives the same score to every text, so it is no better than random.

6. ## Conclusion&#32;

In conclusion, the dataset is clean and split into a training set and a test set with the same class proportion. The dummy model shows that accuracy alone is misleading for our problem.

Next week we will implement a model which is TF-IDF with a logistic regression. TF-IDF transforms each essay into a vector where each column is a word with a high value when the term is frequent in this essay but rare in the other essays. Then the logistic regression will be trained on these vectors. It's a simple and common method for text classification, so it will normally be way better and more accurate than the dummy model. After that, we will extract stylistic features and use feature selection to check if a small set of interpretable features can reach a similar performance like we did in the lab 4 (mutual information filter, RFE, Lasso...). We will also test the models on an AI generator not seen during training, which should be much harder and gives us a better idea of the quality of our model. Finally we will study how the decision threshold changes precision and recall, to limit false positives.
