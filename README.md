# Rapport sur la Tâche 3 de DEFT 2009

## Introduction

### Présentation
- Le contexte du TP repose sur l’analyse des débats parlementaires européens issus de DEFT 2009.
- Objectif : classifier les documents pour identifier le parti politique des orateurs en utilisant différentes représentations textuelles.

## Mise en place du système baseline

### Représentation des documents
- Utilisation de la méthode **TF-IDF** pour représenter les discours parlementaires sous forme de vecteurs de poids.

### Pipeline
- Outils : 
  - **Scikit-learn** pour l’implémentation.
  - **SVM** comme classifieur principal.
- Processus : extraction des caractéristiques (TF-IDF), entraînement sur un corpus annoté, évaluation sur un corpus test.

### Résultats baseline
- La baseline a montré des performances modestes avec une F-mesure globale inférieure à 35 %.

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


#### Méthodologie




#### Faiblesses
1. **Dépendance au volume de données :**
    - Performances faibles pour les partis moins représentés (**ELDR** et **Verts/ALE**).
2. **Simplicité des caractéristiques :**
    - Manque de profondeur pour capturer les nuances linguistiques et contextuelles.
3. **Conflits idéologiques et ambiguïtés :**
    - Les confusions étaient fréquentes entre certains partis proches idéologiquement (par exemple, PSE et PPE-DE).
## Représentation par espaces de thèmes (LDA)

### Justification
- Entraînement d’un modèle LDA pour explorer la thématique des discours.
- Les distributions de thèmes sont intégrées comme nouvelles caractéristiques.

### Paramètres et implémentation
- **Nombre de thèmes :** Optimisé pour refléter les différents partis.
- **Alpha, Beta :** Ajustés pour favoriser la spécificité des thèmes.



## Variations et optimisations

### Approches explorées
1. **Optimisation des hyper-paramètres LDA :** Amélioration de la séparation thématique.
2. **Pré-traitement :** 
    -On a pas utiliser un pretraitement car ca degrade les results

### Analyse des performances


## Tableau comparatif des performances

| **Parti politique** | **Meilleur résultat DEFT 2009 (Rappel / Précision)** | **Performance annotateurs humains (Rappel / Précision)** | **Baseline TF-IDF (Rappel / Précision)** | **LDA (Rappel / Précision)** | **XGBoost Word2Vec (Rappel/Précision)** |
|----------------------|-----------------------------------------------------|---------------------------------------------------------|-----------------------------------------|-----------------------------|---------------------------------|
| **ELDR**            | 23,1 % / 23,6 %                                     | 33 % / 42 %                                             | 67 % / 78 %                             | 8 % / 15 %                 | 57.7 % / 0.41                   |
| **GUE-NGL**         | 39,3 % / 34,5 %                                     | 42 % / 44 %                                             | 80 % / 70 %                             | 58 % / 21 %                | 49.1 % / 0.58                  |
| **PPE-DE**          | 49,8 % / 45,2 %                                     | 57 % / 40 %                                             | 74 % / 75 %                             | 21 % / 43 %                | 88.0 % / 0.64                   |
| **PSE**             | 39,4 % / 37,0 %                                     | 63 % / 48 %                                             | 72 % / 69 %                             | 15 % / 35 %                | 53.3 % / 0.57                   |
| **Verts/ALE**       | 24,3 % / 25,5 %                                     | 29 % / 33 %                                             | 68 % / 73 %                             | 32 % / 15 %                | 19.0 % / 0.31                   |

### Description
Ce tableau résume les performances de différents modèles et méthodes appliquées pour la classification de partis politiques. Il compare les résultats obtenus par différentes approches de traitement de texte (DEFT 2009, annotateurs humains, TF-IDF, LDA, et XGBoost avec Word2Vec) en termes de rappel et de précision.

- **Meilleur résultat DEFT 2009** : Résultats obtenus par le modèle DEFT 2009 sur les données de test.
- **Performance annotateurs humains** : Précision et rappel fournis par des annotateurs humains sur les mêmes données.
- **Baseline TF-IDF** : Résultats obtenus en utilisant des caractéristiques TF-IDF pour la classification.
- **LDA** : Modèle Latent Dirichlet Allocation pour la représentation des documents.
- **XGBoost Word2Vec** : Performances obtenues avec un modèle XGBoost entraîné avec des embeddings Word2Vec.

## Interprétation des résultats(a faire)


## Conclusion

La tâche 3 de DEFT 2009 illustre les limites des systèmes automatiques et les défis liés à l’analyse du langage politique. 
Des méthodes avancées, telles que l’utilisation de modèles pré-entraînés (BERT, GPT), et un corpus mieux équilibré pourraient significativement améliorer les performances.
