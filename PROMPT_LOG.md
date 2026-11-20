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
