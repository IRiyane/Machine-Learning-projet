# Prompt log

Add an entry for each substantial project request. Include the date, student, AI assistant and model, purpose, exact prompt, final output, and checks or constraints. Record the exact model variant when known and use "Not recorded" for unknown models. Keep the prompt and output in their original language. Link to outputs delivered as project files.

For the course submission, select 4–6 significant prompts from this log, with at least one per student.

## 2026-11-20 — Week 2 notebook review

- **Student** — Nathan Kang
- **AI model** — Not recorded
- **Purpose** — Review the Week 2 work pulled from GitHub and compare it with the agreed evaluation protocol.
- **Checks** — Replayed all seven code cells in a temporary copy, compared the regenerated CSVs with the committed files, and checked topic overlap across the splits and validation folds.

### Exact prompt

> J'ai pull le travail de mon collègue du week2, Peux tu l'analyser?

### Output

J’ai analysé le dernier pull (`95f9815`) du projet Machine Learning et rejoué [Week2.ipynb](notebooks/Week2.ipynb). **Le notebook s’exécute et reproduit les résultats enregistrés. Le Week 2 reste incomplet sur deux points principaux, le protocole de test et la baseline prévue.**

1. **Le test ne mesure pas la généralisation à de nouveaux sujets.** J’ai retrouvé les mêmes 15 sujets dans `train.csv`, dans `test.csv` et des deux côtés de chacun des cinq plis de validation. Le protocole du groupe prévoit de réserver trois sujets entiers, de construire un test sur les sujets connus avec les mêmes effectifs par classe, puis de grouper les plis de validation par sujet. [Découpage actuel](notebooks/Week2.ipynb) · [Protocole du groupe](report/project-specification.md#9-evaluation-strategy-data-splitting-methodology-evaluation-metrics-and-validation-approach)
2. **Le seul modèle exécuté prédit toujours « humain ».** J’ai reproduit ses scores en validation croisée : **61 % d’accuracy, F1 de 0 pour la classe IA et ROC AUC de 0,50**. C’est un point de comparaison utile. Il reste à entraîner la baseline TF‑IDF + régression logistique et à mesurer, sur les deux tests, le F1 de la classe IA et la part des textes humains signalés à tort. Le notebook n’évalue actuellement aucun modèle sur `test.csv`. [Évaluation actuelle](notebooks/Week2.ipynb) · [Baseline prévue](report/project-specification.md#8-baseline-strategy-selected-baseline-algorithm-and-justification)
3. **La provenance des textes peut influencer les scores.** Sur 17 sources, 16 contiennent une seule classe. Près de 95 % des textes humains viennent de `persuade_corpus`. Même avec des sujets réservés, un bon score pourrait donc refléter en partie des différences entre sources de rédaction. C’est une limite à signaler dans le rapport. [Analyse par source](notebooks/Week2.ipynb)

L’exploration initiale est en place. Le découpage utilise **10 000 textes sur 44 868**, les deux CSV sont régénérés à l’identique et je n’ai trouvé aucun texte commun aux deux jeux, même après normalisation de la casse et des espaces. Les versions des dépendances restent à renseigner pour reproduire l’exécution sur une autre machine. [Dépendances](requirements.txt)

Je corrigerais **le découpage en premier**, puis je garderais le Dummy comme référence et j’évaluerais TF‑IDF + régression logistique sur les deux nouveaux jeux de test.

## 2026-11-20 — Class proportions and topic holdout in Week 2

- **Student** — Nathan Kang
- **AI model** — Not recorded
- **Purpose** — Evaluate a teammate's observation in the attached screenshot that stratifying the split preserves the minority-class percentage.
- **Checks** — Inspected the split code, counted each label in the exported CSVs, compared the distinct topic lists, and checked the project specification.

### Exact prompt

> Concernant le notebook Week2

### Output

**Ton collègue a raison sur la proportion des classes.** Le découpage est stratifié par label, et j’ai vérifié que la classe IA représente exactement **39 %** des deux jeux : 3 120 textes sur 8 000 dans le train, et 780 sur 2 000 dans le test. [Notebook Week 2](notebooks/Week2.ipynb)

Le point soulevé dans la revue concerne **les sujets des textes**. Les mêmes 15 sujets apparaissent dans les deux jeux. Le test actuel mesure donc la performance sur de nouveaux textes écrits sur des **sujets déjà vus à l’entraînement**.

Votre protocole prévoit **deux tests**. Gardez le principe du découpage stratifié pour le test sur les sujets connus, après avoir réservé tous les textes de trois sujets pour le test sur les sujets inédits. Les deux tests finaux doivent avoir les mêmes effectifs par classe pour que la comparaison se fasse avec la même proportion humain/IA. [Protocole du projet](report/project-specification.md#9-evaluation-strategy-data-splitting-methodology-evaluation-metrics-and-validation-approach)

## 2026-11-20 — Week 2 submission requirements

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Identify what the Week 2 submission still needs after reviewing the current repository.
- **Checks** — Checked the Lab 5 Week 2 instructions and the agreed protocol, inspected the current notebook and dependency file, and used the verified split findings already recorded in the project logs.

### Exact prompt

> Pour le rendu du week2 qu'est ce qu'il faut faire?

### Output

Pour le rendu **Week 2**, la consigne demande **un dataset nettoyé, une baseline implémentée et les premiers résultats**.

1. **Fournir les données nettoyées et le code qui les régénère.** Résumer en anglais le nettoyage, les valeurs manquantes et la répartition humain/IA. Le notebook actuel explore les données et exporte des jeux à partir de 10 000 essais. Si vous retenez ce sous-échantillon, justifiez-le.
2. **Corriger le découpage selon votre spécification.** Réserver trois sujets entiers pour le test « sujets inédits ». Construire un test « sujets vus » avec les mêmes effectifs par classe, puis faire la validation en trois plis groupés par sujet sur le reste. Les deux CSV actuels partagent les 15 sujets. Le test actuel porte donc sur des sujets déjà vus.
3. **Entraîner TF-IDF + régression logistique et présenter les scores des deux tests.** Le notebook a déjà un modèle majoritaire qui sert de référence. Il reste à entraîner la baseline prévue et à présenter, pour chaque test, le **F1 de la classe IA**, le **taux de faux positifs sur les essais humains** et la **matrice de confusion**, avec un court commentaire en anglais.

**Concrètement**, complétez `notebooks/Week2.ipynb`, gardez ses résultats et renseignez les versions utilisées dans `requirements.txt`. Le premier chantier est le découpage.

[Week 2 notebook](notebooks/Week2.ipynb) · [Project specification](report/project-specification.md)

## 2026-11-20 — Verify the current Week 2 work

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Verify which Week 2 tasks are already present and reproducible.
- **Checks** — Compared local HEAD with GitHub main in read-only mode. Replayed all eight code cells in a temporary copy, compared SHA-256 hashes of the regenerated CSVs, and checked the evaluated model, the confusion matrix and topic overlap.

### Exact prompt

> Normalement le travail a déjà été fait, peux tu vérifier?

### Output

**Une partie du Week 2 est terminée.** J’ai vérifié que le dépôt local correspond à la branche `main` sur GitHub (`c6ce23f`) et rejoué les huit cellules de code du notebook sur une copie temporaire. Les deux CSV produits sont identiques à ceux du dépôt.

- **Déjà fait.** L’exploration des données, le découpage de 10 000 essais en 8 000 pour l’entraînement et 2 000 pour le test, ainsi que la baseline majoritaire et sa matrice de confusion. J’ai reproduit ses résultats en validation croisée, avec **61 % d’accuracy et un F1 de 0 pour la classe IA**. Elle prédit toujours « humain ».
- **Encore à faire selon votre spécification.** Les mêmes 15 sujets figurent dans les deux CSV. Il reste à réserver les sujets inédits et à former les deux tests prévus, puis à entraîner **TF-IDF + régression logistique** et à mesurer ses résultats sur ces deux tests. La matrice déjà ajoutée provient de la validation croisée du modèle majoritaire sur les données d’entraînement. Les versions des dépendances restent aussi à renseigner.

**Le travail de ton collègue est bien présent et reproductible.** Ce qui manque, c’est l’expérience qui répond à la question du projet sur les sujets inédits.

[Week 2 notebook](notebooks/Week2.ipynb) · [Project specification](report/project-specification.md)

## 2026-11-20 — Status of the three Week 2 deliverables

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Separate completed work from remaining work for the three Lab 5 deliverables.
- **Checks** — Reviewed the notebook's data operations and the prior replay. Checked normalized text uniqueness, overlap, and empty fields in the exported files. Counted duplicate groups in the full raw CSV after Unicode, case, and whitespace normalization.

### Exact prompt

> Dans ces points là (Clean dataset, baseline implementation and initial experimental results) qu'est ce qui a été fait et qu'est ce qui n'a pas été fait?

### Output

Sur les trois livrables demandés, **une baseline simple et des premiers résultats existent déjà. Le dataset est partiellement préparé.**

| Livrable | Déjà fait | Reste à faire |
| --- | --- | --- |
| **Clean dataset** | Le notebook vérifie les valeurs manquantes et les doublons exacts. Il exporte `train.csv` et `test.csv` à partir de **10 000 essais**. J’ai vérifié que ces deux fichiers contiennent zéro champ vide et zéro doublon après normalisation. | Appliquer et documenter le nettoyage avant de créer les jeux finaux. Sur le corpus brut complet, j’ai trouvé **12 lignes appartenant à 6 groupes de doublons** après normalisation. Préparer aussi les jeux qui isolent les sujets inédits. |
| **Baseline implementation** | Le **modèle majoritaire** est implémenté et s’exécute. Il prédit toujours « humain » et constitue une baseline simple au sens du Lab 5. | Implémenter et entraîner **TF-IDF + régression logistique**, la baseline prévue dans votre spécification. |
| **Initial experimental results** | Le modèle majoritaire a déjà des résultats en **validation croisée sur le train** et une matrice de confusion. Il obtient **61 % d’accuracy** et un **F1 de 0 pour la classe IA**. | Évaluer la régression logistique sur les deux jeux finaux, **sujets vus et sujets inédits**, puis comparer le F1 IA et le taux de faux positifs sur les essais humains. |

[Week 2 notebook](notebooks/Week2.ipynb) · [Project specification](report/project-specification.md)

## 2026-11-20 — Recording the AI model

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Add the model used to the prompt log and repository instructions.
- **Checks** — Checked the five existing entries. Recorded the known model family for this chat and marked unverified historical models as not recorded.

### Exact prompt

> Dans les prompts log, peux tu aussi ajouter le modèle d'ia utilisé? (dans les instructions aussi)

### Output

J’ai ajouté le champ **AI model** au journal des prompts et aux consignes du dépôt. Les entrées de ce chat indiquent **Codex, famille GPT-6**. J’ai marqué **« Not recorded »** dans les deux entrées antérieures où je n’ai pas pu vérifier le modèle. Les prochaines entrées devront indiquer la variante exacte quand elle est connue.

[Prompt log](PROMPT_LOG.md) · [Repository instructions](AGENTS.md)

## 2026-11-20 — Review of the Week 2 report

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Compare the newly pulled Week 2 report with the agreed project protocol and identify what to add.
- **Checks** — Read the report from commit `0a47bab`, compared it with the project specification and discussion log, and used the previously reproduced notebook outputs and normalized duplicate counts.

### Exact prompt

> J'ai pull le report du week2, qu'est ce qu'il faudrait ajouter comparé à ce qu'on s'est dit?

### Output

Le rapport décrit bien ce qui a été exécuté en Week 2. Il présente le corpus, explique le sous-échantillon de 10 000 essais et donne les résultats du modèle majoritaire. **L’ajout prioritaire est le protocole sur les sujets inédits**, qui est au cœur de votre spécification.

1. **Objectif et découpage.** Rétablir le titre ou la question de recherche sur les sujets inédits. Présenter le découpage actuel comme une première expérience, puis décrire le protocole convenu. Réserver trois sujets entiers, constituer un test « sujets vus » avec les mêmes effectifs humains/IA et faire la validation en trois plis groupés par sujet sur le train. Le futur test sur un générateur d’IA inédit peut rester une extension.
2. **Baseline et résultats.** Nommer les chiffres actuels comme les résultats du `DummyClassifier` en validation croisée sur le train. Une fois **TF-IDF + régression logistique** exécuté, ajouter un tableau comparant, pour les deux tests, le **F1 de la classe IA**, le **taux de faux positifs** (essais humains signalés / total des essais humains) et les matrices de confusion. Présenter les scores après les avoir mesurés.
3. **Nettoyage.** Préciser que le notebook trouve zéro doublon **exact**. Après normalisation de l’Unicode, de la casse et des espaces, le corpus brut contient **six groupes de doublons, soit douze lignes**. Documenter la règle retenue et le nombre de lignes conservées avant le découpage final.
4. **Limites.** La provenance des textes est très liée à la classe. Présenter la longueur des essais comme une piste à tester, car une partie de l’écart observé peut venir des différences entre sources.

Dans la conclusion, je distinguerais le **résultat déjà mesuré** avec le modèle majoritaire et l’**expérience à terminer** avec TF-IDF sur les deux tests.

[Week 2 report](<report/Week 2. Data Preparation and Baseline.md>) · [Project specification](report/project-specification.md)

## 2026-11-20 — Add the topic-based evaluation to the Week 2 report

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Add the agreed topic-based split and validation plan to the report.
- **Checks** — Checked the report's existing split and the project specification. Verified that the new text describes future work and that the generator holdout is presented as optional.

### Exact prompt

> oui ajoute ça

The student annotated the recommendation to reserve three topics, match the class counts in a familiar-topic test, and use three validation folds grouped by topic.

### Output

J’ai ajouté au rapport le découpage prévu avec trois sujets réservés, deux tests aux mêmes effectifs par classe et trois plis de validation groupés par sujet. Le test sur un générateur inédit figure comme extension facultative.

[Updated Week 2 report](<report/Week 2. Data Preparation and Baseline.md>)

## 2026-11-27 — Week 3 topic holdout and Lab 6

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Implement the agreed topic-based evaluation, tune the two text classifiers, and apply soft voting.
- **Checks** — Ran all six code cells against the local DAIGT v2 CSV. Checked essay uniqueness after normalization, split coverage, topic separation in the test and grouped folds, matching test class counts, and the saved metrics and figures. Checked that the Week 2 notebook was byte-identical.

### Exact prompt

> Il y a normalement un notebook nommé week2. Peux tu faire un nouveau notebook nommé week3 qui reprend le week2 en appliquant ce qu'il manquait pour le week2 (les sujets complètements différents pour éviter le "data leakage") et le lab6 qu'on vient de faire.

### Output

J’ai créé et exécuté [Week3.ipynb](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/notebooks/Week3.ipynb>).

- **Découpage par sujet.** Trois sujets entiers sont réservés au test et exclus de l’entraînement et de la validation. Les tests sur sujets connus et inédits contiennent chacun **4 162 textes humains et 1 429 textes IA**. La validation se fait en trois plis groupés par sujet, avec TF-IDF ajusté dans chaque pli.
- **Lab 6.** Le notebook optimise la régression logistique et Complement Naive Bayes avec `GridSearchCV`, puis les combine par vote souple.

Sur les sujets inédits, le vote obtient un **F1 IA de 0,903** et signale à tort **0,62 %** des textes humains. La régression logistique seule obtient un F1 de **0,896** et **3,10 %** de fausses alertes.

J’ai aussi épinglé les dépendances et mis à jour les journaux du projet.

[Project notebook](../notebooks/Week3.ipynb)

## 2026-11-27 — Annotate the Week 3 notebook

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Explain the main steps in the Week 3 code with short English comments.
- **Checks** — Compared the parsed Python for all six code cells, compiled each cell, and verified the saved notebook and its recorded outputs.

### Exact prompt

> Peux tu annoter le code

### Output

J’ai ajouté des commentaires courts en anglais dans [Week3.ipynb](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/notebooks/Week3.ipynb>). Ils expliquent le rôle de `prompt_name`, la séparation des sujets, la validation et le calcul des faux positifs. J’ai vérifié les six cellules de code et leurs sorties enregistrées.

[Project notebook](../notebooks/Week3.ipynb)

## 2026-11-27 — Explain the Week 3 notebook and leakage controls

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Explain the six Week 3 code cells and how the evaluation protects against leakage.
- **Checks** — Read all six cells in the saved notebook. Verified the split assertions, the topic groups used in validation, the location of TF-IDF inside the pipeline, and the use of test labels for evaluation.

### Exact prompt

> Peux tu m’expliquer simplement le code de Week3, cellule par cellule, surtout comment on évite le data leakage ?

### Output

Dans [Week3.ipynb](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/notebooks/Week3.ipynb>), le **data leakage** serait par exemple d’utiliser les textes du test pour apprendre le vocabulaire. Le notebook sépare les données avant d’entraîner les modèles, puis réserve des sujets entiers pour mesurer leur capacité à traiter une consigne inédite.

1. **Charger et nettoyer les textes.** Le CSV fournit le texte, son label (`0` humain, `1` IA) et son sujet (`prompt_name`). Le code uniformise la casse et les espaces pour repérer les doublons et retire toutes leurs copies. Ainsi, le même essai ne peut pas se retrouver à la fois dans l’entraînement et le test.

2. **Former les deux tests.** `GroupShuffleSplit` réserve trois sujets entiers au **test inédit**. Par exemple, tous les essais conservés de `Phones and driving` vont dans ce test. Le **test connu** prend d’autres essais sur des sujets représentés dans l’entraînement, avec les mêmes nombres de textes humains et IA que le test inédit. Le code vérifie que chaque essai appartient à un seul jeu.

3. **Régler les deux modèles.** `GridSearchCV` essaie plusieurs réglages pour la régression logistique et Complement Naive Bayes. À chaque pli, les sujets de validation sont séparés des sujets d’entraînement. Le `Pipeline` apprend le vocabulaire et les poids TF-IDF **uniquement sur les textes d’entraînement du pli**, puis applique cette transformation aux textes de validation. Les deux tests finaux restent réservés à l’évaluation.

4. **Construire le vote souple.** Le modèle de référence prédit toujours la classe majoritaire. Le vote souple réentraîne les deux modèles optimisés sur le jeu d’entraînement et moyenne leurs probabilités de prédire « IA ».

5. **Évaluer sur les deux tests.** Les modèles déjà entraînés font leurs prédictions. Les labels du test servent à calculer les scores et les matrices de confusion. Le **taux de faux positifs** est le pourcentage de textes humains signalés à tort comme IA.

6. **Afficher les résultats.** Les deux graphiques comparent le F1 de la classe IA et les faux positifs, sur sujets connus et inédits, pour chaque modèle.

[Project notebook](../notebooks/Week3.ipynb)

## 2026-11-27 — Write the Week 3 report

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Write the Week 3 report in English and summarize its contents.
- **Checks** — Read the Week 2 report and project specification. Checked split sizes, selected hyperparameters, and scores against the saved Week 3 notebook. Read back the report and checked the notebook link.

### Exact prompt

> Peux tu faire un rapport pour le week3. dis moi ce que t'as mis

### Output

J’ai rédigé le [rapport Week 3](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/Week 3. Topic-Based Evaluation and Ensemble Learning.md>) en anglais, dans le même format que celui de Week 2.

J’y ai mis le nettoyage du corpus, le découpage en sujets connus et inédits, les étapes qui évitent le *data leakage*, puis l’optimisation des deux modèles et leur combinaison par vote souple. Un tableau compare les quatre modèles sur les deux tests.

Sur les sujets inédits, le vote souple obtient le meilleur **F1 de la classe IA (0,903)** et produit **26 fausses alertes**, contre **129** pour la régression logistique. Le rapport montre aussi que le vote manque davantage de textes IA.

[Week 3 report](<../report/Week 3. Topic-Based Evaluation and Ensemble Learning.md>)

## 2026-11-27 — Verify Lab 6 Part II completion

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Verify the five Lab 6 Part II requirements shown in the attached screenshot.
- **Checks** — Validated the Week 3 notebook and read its saved grid-search results and figures. Compared the project specification and Week 3 report with the screenshot. Queried the live GitHub branch refs and compared the remote main commit and tracked files with the local Git status.

### Exact prompt

> Peux tu vérifier que tous ces points ont été fait?

The attached screenshot shows the five requirements from Lab 6 Part II.

### Output

| Point du Lab 6 | État | Vérification |
| --- | --- | --- |
| Optimiser les modèles avec Grid Search | **Fait** | `GridSearchCV` a sélectionné `C=4.0` pour la régression logistique et `alpha=1.0` pour Complement Naive Bayes. [Notebook Week 3](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/notebooks/Week3.ipynb>) |
| Inclure cette étape dans la proposition finale | **Partiel** | La proposition décrit la validation groupée par sujet. Il reste à y ajouter `GridSearchCV` et les réglages retenus. [Proposition](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/project-specification.md:47>) |
| Appliquer une méthode d’ensemble | **Fait** | Le vote souple combine les deux modèles optimisés. Il obtient un F1 de **0,903** sur les sujets inédits. |
| Discuter les résultats et présenter les figures dans le rapport final | **Partiel** | Le rapport Week 3 compare les deux tests. Les trois figures sont dans le notebook. Il reste à intégrer cette analyse et les figures au rapport final. [Rapport Week 3](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/Week 3. Topic-Based Evaluation and Ensemble Learning.md>) |
| Pousser le code sur GitHub | **À faire** | J’ai vérifié la branche distante `main`. Elle est au commit `7d33b16`. Le notebook et le rapport Week 3 sont encore des fichiers locaux non suivis par Git. [Branche main](https://github.com/IRiyane/Machine-Learning-projet/tree/main) |

## 2026-11-27 — Add Lab 6 evaluation results to the final report

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Add the Week 3 model comparison and figures to the submission source, and explain how to describe the grid search in the proposal.
- **Checks** — Confirmed the submission source in the README. Cross-checked the comparison with the Week 3 report and saved notebook outputs. Verified the three PNG files and their relative links. Confirmed that the existing proposal text remains byte-identical.

### Exact prompt

> Discuter les résultats et présenter les figures dans le rapport final : ok ajoute.
> La proposition décrit la validation groupée par sujet. Il reste à y ajouter `GridSearchCV` et les réglages retenus.   c'est à dire?

### Output

J’ai ajouté le tableau comparatif, l’analyse des résultats et les trois figures au [document source du rapport final](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/project-specification.md>). Sur les sujets inédits, le vote souple obtient le meilleur F1 (**0,903**) et réduit les fausses alertes de **129 à 26** par rapport à la régression logistique. Il détecte aussi moins de textes IA.

Pour la **proposition**, la validation groupée explique **comment on forme les trois plis**. Décrire `GridSearchCV`, c’est préciser **quels réglages ont été essayés et comment on a choisi les meilleurs**. Le critère était le F1 moyen de la classe IA sur les trois plis.

- **Régression logistique** — `C` testé à 0,1, 1 et 4. Valeur retenue **4**.
- **Complement Naive Bayes** — `alpha` testé à 0,1, 0,5 et 1. Valeur retenue **1**.

[Submission source](../report/project-specification.md#lab-6-week-3-evaluation-results)

## 2026-11-27 — Next experiments to improve the essay detector

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Prioritize follow-up experiments and assess whether a neural model could be tested.
- **Checks** — Calculated the current precision and recall from the recorded confusion-matrix counts. Checked the official documentation for threshold tuning, character TF-IDF, and DistilBERT.

### Exact prompt

> Quel serait les next step pour améliorer les précisions? Peut on utiliser un systeme de neural network ?

### Output

Oui, on peut tester un réseau de neurones. Sur les sujets inédits, le vote de Week 3 a déjà une **précision de 97,9 %** parmi les textes signalés comme IA. Son **rappel est de 83,8 %**. Il détecte 1 197 des 1 429 textes IA et déclenche 26 fausses alertes. Je viserais donc à **détecter plus de textes IA en gardant peu de fausses alertes**. [Résultats Week 3](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/project-specification.md:87>)

1. **Ajuster le seuil du vote.** Essaie plusieurs seuils sur les plis de validation groupés par sujet. Un seuil plus strict peut réduire les fausses alertes et faire manquer davantage de textes IA. La courbe précision-rappel permet de choisir le compromis. [Documentation scikit-learn](https://scikit-learn.org/stable/modules/classification_threshold.html)
2. **Tester le TF-IDF sur les caractères.** Garde la régression logistique et essaie des suites de 3 à 5 caractères. Cela donne au modèle accès à des fragments de mots et à des motifs de ponctuation. Vérifie si ça aide sur les sujets de validation. [Documentation TF-IDF](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)
3. **Adapter DistilBERT.** C’est un réseau de neurones préentraîné sur des textes anglais. On peut l’ajuster sur les essais étiquetés « humain » ou « IA », puis comparer son rappel, son F1 et ses fausses alertes à ceux du vote avec les mêmes plis groupés. [Fiche DistilBERT](https://huggingface.co/distilbert/distilbert-base-uncased) · [Guide de classification de textes](https://huggingface.co/docs/transformers/tasks/sequence_classification)

Choisis le modèle et le seuil sur la validation groupée. Pour confirmer un gain après ces essais, l’idéal est d’avoir un nouveau lot de sujets réservé au test final.

## 2026-11-27 — Document the planned Week 4 experiments

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Record the three agreed follow-up experiments in the report.
- **Checks** — Read back both Markdown files and verified that the submission source links to the Week 4 experiment section.

### Exact prompt

> Dans le rapport, indique qu'on testera ça au week4

### Output

J’ai ajouté au [rapport Week 3](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/Week 3. Topic-Based Evaluation and Ensemble Learning.md>) les essais prévus pour Week 4. On testera le réglage du seuil du vote souple, le TF-IDF sur les caractères et l’adaptation de DistilBERT.

L’objectif est de détecter davantage de textes IA tout en gardant peu de fausses alertes. Le [document source du rapport final](<C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/project-specification.md>) renvoie aussi à ce plan.

[Week 4 experiment plan](../report/Week%203.%20Topic-Based%20Evaluation%20and%20Ensemble%20Learning.md#week-4-experiments)

## 2026-11-27 — Embed the Week 3 figures in the report

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Put all three figures inside a single PDF and update the Markdown figure references.
- **Checks** — Rendered and inspected all three PDF pages. Verified the embedded images against the source figures at pixel level and confirmed that the original PNGs are available in the saved Week 3 notebook before removing the separate copies.

### Exact prompt

> Peux tu mettre les screen shot dans le rapport directement au lieu de le placer à part

The attached screenshot shows the report folder and its separate figures directory.

### Output

J’ai intégré les trois figures directement dans le PDF du rapport. J’ai retiré le dossier de copies séparées et les légendes du document Markdown renvoient aux pages correspondantes.

:codex-file-citation{path="C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/Project-Report.pdf" purpose="output"}

[PDF report](../report/Project-Report.pdf)

## 2026-11-27 — Clarify the error increase on unseen topics

- **Student** — Nathan Kang
- **AI model** — Codex (GPT-6 family; exact variant not recorded)
- **Purpose** — Verify and clarify how the report discusses errors on unseen topics.
- **Checks** — Cross-checked the error totals with the confusion matrices, rendered the three updated PDF pages, and verified the updated discussion on page 2 and the three embedded figures.

### Exact prompt

> Tu l'as bien dis dans le rapport?

### Output

Le rapport mentionnait déjà la baisse du F1. J’ai précisé dans la **discussion, page 2**, que les trois modèles font plus d’erreurs sur les sujets inédits.

Pour le vote souple, le total passe de **65 à 258 erreurs**. Les fausses alertes restent à **26** sur chaque test. La hausse vient des textes IA manqués, qui passent de **39 à 232**.

:codex-file-citation{path="C:/Users/natha/Documents/GitHub/Machine-Learning-projet/report/Project-Report.pdf" purpose="output"}

[PDF report](../report/Project-Report.pdf#page=2)
