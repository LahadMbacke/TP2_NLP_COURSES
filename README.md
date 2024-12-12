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

#### Méthodologie
L'équipe de l'Université de Montréal a utilisé une approche supervisée :
1. **Extraction des caractéristiques textuelles :**
    - Représentation des discours sous forme de vecteurs basés sur :
        - La fréquence des mots.
        - Les expressions spécifiques aux partis politiques.
        - Les méta-informations (longueur des discours, mots-clés).
2. **Classification supervisée :**
    - Utilisation d’un algorithme de classification avec un corpus d’apprentissage annoté.
    - Équilibrage des données pour les cinq partis principaux : **PPE-DE, PSE, GUE-NGL, ELDR et Verts/ALE**.
3. **Évaluation :**
    - Mesure des performances en termes de rappel, précision et F-mesure sur un corpus test.

#### Forces
1. **Alignement multilingue :**
    - Permet une meilleure généralisation et des comparaisons entre langues.
    - Base solide pour des analyses ultérieures, même si seules les données françaises ont été utilisées.
2. **Approche centrée sur les principaux partis :**
    - Réduction de la complexité en se concentrant sur les groupes avec des exemples suffisants.
3. **Méthode reproductible :**
    - Utilisation de caractéristiques simples et d’algorithmes classiques pour assurer transparence et reproductibilité.

#### Faiblesses
1. **Dépendance au volume de données :**
    - Performances faibles pour les partis moins représentés (**ELDR** et **Verts/ALE**).
2. **Simplicité des caractéristiques :**
    - Manque de profondeur pour capturer les nuances linguistiques et contextuelles.
    - Absence de techniques avancées comme les embeddings (Word2Vec, BERT).
3. **Conflits idéologiques et ambiguïtés :**
    - Confusions fréquentes entre partis proches (é.g., **PSE** et **GUE-NGL**, ou **PPE-DE** et **ELDR**).

## Représentation par espaces de thèmes (LDA)

### Justification
- Entraînement d’un modèle LDA pour explorer la thématique des discours.
- Les distributions de thèmes sont intégrées comme nouvelles caractéristiques.

### Paramètres et implémentation
- **Nombre de thèmes :** Optimisé pour refléter les différents partis.
- **Alpha, Beta :** Ajustés pour favoriser la spécificité des thèmes.

### Résultats
- L’ajout des thèmes a amélioré la précision pour certains partis comme **Verts/ALE**.
- Comparée à la baseline TF-IDF, cette méthode a montré des gains modérés.

## Variations et optimisations

### Approches explorées
1. **Optimisation des hyper-paramètres LDA :** Amélioration de la séparation thématique.
2. **Pré-traitement :** 
    - Utilisation de la lemmatisation pour réduire la variabilité lexicale.

### Analyse des performances
- Les variantes ont montré des améliorations marginales sur certains partis.
- Les gains restent limités pour les partis avec peu de données (**ELDR**).

## Tableau comparatif des performances

| **Parti politique** | **Meilleur résultat DEFT 2009 (Rappel / Précision)** | **Performance annotateurs humains (Rappel / Précision)** |
|----------------------|-----------------------------------------------------|---------------------------------------------------------|
| **ELDR**            | 23,1 % / 23,6 %                                     | 33 % / 42 %                                             |
| **GUE-NGL**         | 39,3 % / 34,5 %                                     | 42 % / 44 %                                             |
| **PPE-DE**          | 49,8 % / 45,2 %                                     | 57 % / 40 %                                             |
| **PSE**             | 39,4 % / 37,0 %                                     | 63 % / 48 %                                             |
| **Verts/ALE**       | 24,3 % / 25,5 %                                     | 29 % / 33 %                                             |

## Interprétation des résultats

1. **Difficulté globale :**
    - Les résultats automatiques restent faibles pour tous les partis.
    - Les performances humaines montrent aussi des limites, notamment pour les petits partis.

2. **Influence des données disponibles :**
    - Les partis dominants (**PPE-DE**, **PSE**) bénéficient d’une meilleure précision grâce à un volume d’exemples plus important.

3. **Sources d’erreurs :**
    - Confusions entre partis proches idéologiquement ou dans leur discours.
    - Difficulté à interpréter les interventions ambiguës ou génériques.

## Conclusion

La tâche 3 de DEFT 2009 illustre les limites des systèmes automatiques et les défis liés à l’analyse du langage politique. 
Des méthodes avancées, telles que l’utilisation de modèles pré-entraînés (BERT, GPT), et un corpus mieux équilibré pourraient significativement améliorer les performances.
