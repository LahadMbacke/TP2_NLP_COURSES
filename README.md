# Rapport sur la Tâche 3 de DEFT 2009

## Introduction

### Présentation
- Le contexte du TP repose sur l’analyse des débats parlementaires européens issus de DEFT 2009.
- Objectif : classifier les documents pour identifier le parti politique des orateurs en utilisant différentes représentations textuelles.

## Mise en place du système baseline

### Pipeline
- Outils : 
  - **Scikit-learn** pour l’implémentation.
  - **SVM** comme classifieur principal.
- Processus : extraction des caractéristiques (TF-IDF), entraînement sur un corpus annoté, évaluation sur un corpus test.

## Analyse de l’approche de DEFT 2009

### Approche adoptée pour la Tâche 3 : Détermination du parti politique

L'approche proposée pendant la campagne DEFT 2009 pour la tâche de prédiction du parti politique se décompose en plusieurs étapes clés, principalement centrées sur l'expérimentation avec les paramètres du modèle et les algorithmes de classification. 

#### Phase d'apprentissage :
- **Paramètres d'expérimentation :**
  - Le nombre de traits discriminants variait entre 1 000 et 20 000 mots, avec des incréments de 1 000 ou 5 000 mots.
  - Les mots sont représentés par leur fréquence ou leur fréquence pondérée par l'IDF (Inverse Document Frequency).
  - Les algorithmes utilisés étaient les k plus proches voisins (k=1, k=2, k=3, k=4, k=5) et le classifieur bayésien naïf.
  
- **Meilleures performances :**
  - Les meilleurs résultats ont été obtenus en utilisant 10 000 mots discriminants, représentés par leur fréquence, et l'algorithme des k plus proches voisins avec k=2. Cette configuration a permis d'atteindre un taux de rappel de 94,12% et un taux de précision de 94,53%.

- **Pires performances :**
  - Les pires performances ont été observées avec 100 mots discriminants, ce qui a donné un taux de rappel de 33,57% et un taux de précision de 36,34%.

#### Phase de test :
- **Exécutions de test :**
  - Trois exécutions ont été réalisées avec des variations dans le nombre de traits discriminants et la valeur du paramètre k. 
  - Les résultats des tests montrent des performances globales faibles par rapport à la phase d'apprentissage, avec des taux de rappel et de précision autour de 33%. 
  - Les meilleures performances pour la catégorie PPE-DE ont atteint un rappel de 49,80% et une précision de 45,20%.

#### Forces :
- L'algorithme des k plus proches voisins a montré de bonnes performances globales, surtout pour les valeurs faibles de k (k=1 ou k=2).
- L'utilisation de 10 000 mots discriminants a permis d'obtenir de très bons résultats lors de la phase d'apprentissage.

#### Faiblesses :
- Les performances de la phase de test sont nettement inférieures à celles de la phase d'apprentissage, ce qui suggère un surapprentissage (overfitting) des modèles.
- Le filtrage des mots discriminants pendant l'apprentissage n’a pas permis de maintenir les performances lors des tests, certains mots discriminants devenant moins efficaces dans les données de test.
- Le classifieur bayésien naïf s’est avéré moins performant que l'algorithme des k plus proches voisins pour cette tâche.

En conclusion, bien que l'approche ait montré de bons résultats en phase d'apprentissage, elle a souffert de problèmes de généralisation lors de la phase de test, principalement dus au surapprentissage.


# Représentation par espaces de thèmes (LDA)
## Choix du Corpus
Le corpus choisi remplit parfaitement les critères nécessaires pour entraîner un modèle LDA efficace. Il capture la diversité des opinions politiques, reflétant ainsi les différences idéologiques entre les partis, tout en étant représentatif du contexte linguistique des documents de test, grâce à des textes en français issus de discours et débats parlementaires. Ce corpus couvre une large variété de sujets, tels que l’économie, la santé ou l’environnement, garantissant des thèmes latents pertinents et diversifiés. De plus, son volume important, composé de plusieurs milliers de documents, permet de modéliser des thèmes cohérents et fiables, tout en respectant le style formel caractéristique des textes politiques.
## Analyse des Hyperparamètres

### 1. **CountVectorizer**
Le `CountVectorizer` est utilisé pour transformer les documents en matrices de comptage de mots, avec les hyperparamètres suivants :
- **`max_df=0.95`** : Ignore les mots qui apparaissent dans plus de 95% des documents, afin de se concentrer sur les termes discriminants.
- **`min_df=2`** : Ne garde que les mots qui apparaissent dans au moins 2 documents, ce qui permet de réduire le bruit.
- **`stop_words=french_stop_words`** : Supprime les mots vides (comme "le", "la", etc.) qui n'apportent pas de valeur discriminante.

## 2. **Latent Dirichlet Allocation (LDA)**
LDA est utilisé pour extraire des thèmes à partir des documents. Les paramètres définis sont :
- **`n_components=5`** : Le nombre de thèmes à identifier dans les documents (ici, 5 thèmes).
- **`doc_topic_prior=1.0`** : Ce paramètre uniformise la répartition des thèmes dans chaque document.
- **`topic_word_prior=0.8`** : Permet une certaine diversité dans les mots associés aux thèmes, évitant une trop forte concentration sur quelques mots spécifiques.

## 3. **SVC (Support Vector Classifier)**
Le classifieur SVM est utilisé pour la classification finale des documents avec les paramètres suivants :
- **`kernel='rbf'`** : Utilise un noyau Radial Basis Function (RBF), qui est adapté pour traiter des données non linéaires.
- **`class_weight='balanced'`** : Ajuste les poids des classes pour compenser un éventuel déséquilibre entre les classes cibles.


## Variations et optimisations

### Approches explorées
1. **Optimisation des hyper-paramètres LDA :** Amélioration de la séparation thématique.
2. **Pré-traitement :** 
    ### Tests effectués :  
1. **Lemmatisation** :  
   - Résultat : Perte de précision, probablement liée à la simplification excessive des termes spécifiques aux débats parlementaires.  

2. **Suppression des stopwords** :  
   - Résultat : Réduction des informations pertinentes, notamment les termes fonctionnels qui jouent un rôle discriminant dans les discours politiques.  

3. **Conversion en minuscules** :  
   - Résultat : Impact limité mais négatif, probablement dû à la perte de distinction entre noms propres et autres termes.  

### Analyse des performances


## Tableau comparatif des performances

| **Parti politique** | **Meilleur résultat DEFT 2009 (Rappel / Précision)** | **Performance annotateurs humains (Rappel / Précision)** | **Baseline TF-IDF (Rappel / Précision)** | **LDA  (Rappel / Précision)** | **LDA équilibré (Rappel / Précision)** | **XGBoost Word2Vec (Rappel/Précision)** |
|----------------------|-----------------------------------------------------|---------------------------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|-----------------------------------------|
| **ELDR**            | 23,1 % / 23,6 %                                     | 33 % / 42 %                                             | 67 % / 78 %                             | 8 % / 15 %                              | 18 % / 27 %                             | 24 % / 90 %                         |
| **GUE-NGL**         | 39,3 % / 34,5 %                                     | 42 % / 44 %                                             | 80 % / 70 %                             | 58 % / 21 %                             | 62 % / 37 %                             | 45 % / 71 %                         |
| **PPE-DE**          | 49,8 % / 45,2 %                                     | 57 % / 40 %                                             | 74 % / 75 %                             | 21 % / 43 %                             | 19 % / 27 %                             | 88.0 % / 51 %                         |
| **PSE**             | 39,4 % / 37,0 %                                     | 63 % / 48 %                                             | 72 % / 69 %                             | 15 % / 35 %                             | 24 % / 27 %                             | 54 % / 63 %                         |
| **Verts/ALE**       | 24,3 % / 25,5 %                                     | 29 % / 33 %                                             | 68 % / 73 %                             | 32 % / 15 %                             | 28 % / 27 %                             | 20 % / 96 %                         |


## Variations des approches

### 1. **XGBoost Word2Vec**
- Nous avons exploré l'utilisation des embeddings **Word2Vec** pour représenter les documents parlementaires. 
- Ces embeddings sont ensuite utilisés comme entrée dans un classifieur **XGBoost**, qui combine robustesse et adaptabilité pour capturer des relations complexes dans les données.  

### 2. **LDA avec des donnees équilibré**
- Contrairement à l'approche standard, nous avons modifié les données d'entraînement et de test pour obtenir un corpus **équilibré** en termes de représentation des partis politiques.  
- Cette méthode vise à atténuer l'impact des classes majoritaires sur les performances globales, en fournissant un volume de données comparable pour chaque parti politique.

### Analyse détaillée

#### 1. **ELDR**  
- **XGBoost Word2Vec** offre une précision exceptionnelle (90 %), mais un rappel faible (24 %), suggérant une identification très sûre de certains discours, mais une couverture limitée.  
- **Baseline TF-IDF** propose un équilibre solide (67 % / 78 %), ce qui en fait une méthode plus fiable pour ce parti.  

#### 2. **GUE-NGL**  
- La **baseline TF-IDF** atteint les meilleurs résultats (80 % / 70 %), équilibrant rappel et précision.  
- **LDA équilibré** montre une légère amélioration par rapport à LDA déséquilibré, mais reste inférieur à TF-IDF, indiquant une faiblesse des modèles thématiques pour ce parti.  

#### 3. **PPE-DE**  
- Les meilleures performances en rappel proviennent de **XGBoost Word2Vec** (88 %), mais avec une précision moyenne (51 %), ce qui montre une forte sur-représentation de ce parti dans les prédictions.  
- **Baseline TF-IDF** maintient un excellent équilibre (74 % / 75 %), surpassant les autres méthodes.  

#### 4. **PSE**  
- La **baseline TF-IDF** performe le mieux (72 % / 69 %), avec des scores stables et équilibrés.  
- **LDA**, même avec équilibrage, reste peu performant avec un rappel bas (15 % à 24 %).  

#### 5. **Verts/ALE**  
- **XGBoost Word2Vec** atteint une précision remarquable (96 %), mais un rappel très faible (20 %), indiquant une identification précise de certains discours, mais une incapacité à généraliser.  
- La **baseline TF-IDF** (68 % / 73 %) offre une solution plus homogène et fiable.  


### Perspectives  
Pour améliorer les performances, des approches modernes comme les modèles pré-entraînés (BERT, GPT) ou une meilleure gestion du déséquilibre des données devraient être explorées.
