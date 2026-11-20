# Prompt log

Add an entry for each substantial project request. Include the date, student, purpose, exact prompt, final output, and checks or constraints. Keep the prompt and output in their original language. Link to outputs delivered as project files.

For the course submission, select 4–6 significant prompts from this log, with at least one per student.

## 2026-11-20 — Week 2 notebook review

- **Student** — Nathan Kang
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
- **Purpose** — Evaluate a teammate's observation in the attached screenshot that stratifying the split preserves the minority-class percentage.
- **Checks** — Inspected the split code, counted each label in the exported CSVs, compared the distinct topic lists, and checked the project specification.

### Exact prompt

> Concernant le notebook Week2

### Output

**Ton collègue a raison sur la proportion des classes.** Le découpage est stratifié par label, et j’ai vérifié que la classe IA représente exactement **39 %** des deux jeux : 3 120 textes sur 8 000 dans le train, et 780 sur 2 000 dans le test. [Notebook Week 2](notebooks/Week2.ipynb)

Le point soulevé dans la revue concerne **les sujets des textes**. Les mêmes 15 sujets apparaissent dans les deux jeux. Le test actuel mesure donc la performance sur de nouveaux textes écrits sur des **sujets déjà vus à l’entraînement**.

Votre protocole prévoit **deux tests**. Gardez le principe du découpage stratifié pour le test sur les sujets connus, après avoir réservé tous les textes de trois sujets pour le test sur les sujets inédits. Les deux tests finaux doivent avoir les mêmes effectifs par classe pour que la comparaison se fasse avec la même proportion humain/IA. [Protocole du projet](report/project-specification.md#9-evaluation-strategy-data-splitting-methodology-evaluation-metrics-and-validation-approach)
