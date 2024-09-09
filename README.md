# Projet 3 : Prédiction de la Consommation d'Énergie et des Émissions de Gaz à Effet de Serre des Bâtiments à Seattle

## Introduction
Ce projet porte sur l'analyse et la modélisation de données du jeu **Seattle Building Energy Benchmarking**. Ce dataset contient des informations détaillées sur des bâtiments à Seattle, telles que leur surface, leur usage, leur localisation ainsi que leur consommation énergétique et leurs émissions de gaz à effet de serre.

### Objectifs du projet
L'objectif principal du projet est d'entraîner et d'évaluer différents modèles de **machine learning** pour prédire :
- La **consommation d'énergie** des bâtiments non résidentiels.
- Les **émissions de gaz à effet de serre**.

Le projet inclut également une évaluation de la pertinence du score **ENERGY STAR Score** dans la prédiction des émissions de gaz à effet de serre, afin de déterminer si ce score, difficile à calculer, est réellement utile.

## Démarche

### 1. Nettoyage des données et Feature Engineering

#### Nettoyage des données
Le jeu de données contient plusieurs variables avec des valeurs manquantes et aberrantes. Les opérations de nettoyage effectuées incluent :
- **Suppression des doublons.**
- **Traitement des valeurs manquantes** :
  - Suppression des variables avec plus de 1% de valeurs manquantes.
  - Remplacement des valeurs manquantes dans certaines colonnes avec des valeurs par défaut appropriées.
- **Identification et suppression des valeurs aberrantes** à l'aide de la méthode du **z-score**.

#### Feature Engineering
Certaines variables fortement corrélées avec les cibles de prédiction (comme `SiteEUI` ou `Electricity(kwh)`) ont été retirées pour éviter le **data leakage**. En outre, un encodage **one-hot** des variables catégorielles (telles que `BuildingType` et `Neighborhood`) a été réalisé.

### 2. Analyse exploratoire des données (EDA)

#### Analyse univariée
- Étude de la répartition des bâtiments selon leur **ENERGY STAR Score**, montrant que la majorité des bâtiments sont relativement efficaces en termes d'émissions de gaz à effet de serre (score médian ≈ 75).

#### Analyse bivariée
- Exploration des relations entre la **surface des bâtiments** et leur consommation d'énergie, révélant une corrélation positive entre la taille des bâtiments et leur consommation.

### 3. Modélisation

Plusieurs modèles ont été entraînés pour prédire la consommation d'énergie et les émissions de gaz à effet de serre :

#### Modèles linéaires
- **Régression linéaire**
- **Régression Ridge**
- **Régression Lasso**

#### Modèles non-linéaires
- **Random Forest Regressor**
- **XGBoost**

Un **Dummy Regressor** a également été utilisé comme modèle de référence pour comparer les performances.

#### Fonction d'entraînement et validation croisée
Une fonction a été mise en place pour automatiser l'entraînement des modèles avec une recherche des meilleurs hyperparamètres via **GridSearch** et validation croisée. Les résultats incluent :
- **Score R²** pour évaluer les performances du modèle.
- **SHAP values** pour interpréter l'importance des features dans les prédictions.

### 4. Résultats

#### Modèles linéaires
- La **régression linéaire** a montré des signes d'overfitting sur l'ensemble d'entraînement, surtout après l'encodage one-hot. Cependant, les régressions **Ridge** et **Lasso** ont réduit cet overfitting, améliorant ainsi les résultats.

#### Modèles non-linéaires
- Les **Random Forest** et **XGBoost** ont obtenu de meilleures performances, en particulier pour la consommation d'énergie avec un score R² de **0.85** pour XGBoost.
- Les **SHAP values** ont montré que certaines variables (comme la surface et le type de bâtiment) étaient cruciales pour les prédictions.

### 5. Importance des Features
- L'importance des features a été évaluée à l'aide des **valeurs SHAP** pour identifier les variables ayant le plus d'impact sur les prédictions globales. 

### 6. Temps de calcul
Bien que les modèles plus complexes comme **XGBoost** offrent de meilleures performances, leur temps d'entraînement est significativement plus long (jusqu'à plusieurs milliers de fois plus élevé que celui des modèles linéaires).

## Conclusion
Le modèle **XGBoost** a offert les meilleures performances pour la prédiction de la consommation d'énergie avec un score R² de **0.85**. 
Pour la prédiction des émissions de gaz à effet de serre, la performance est plus faible (**R² = 0.42**), mais le modèle reste pertinent. 
Enfin, l'étude a montré que l'**ENERGY STAR Score** n'est pas indispensable pour prédire la consommation d'énergie, mais peut être utile pour la prédiction des émissions.

## Outils utilisés
- **Python**
- **Pandas** pour le traitement des données
- **Scikit-learn** pour les modèles linéaires et l'évaluation
- **XGBoost** pour les modèles non linéaires
- **SHAP** pour l'interprétation des modèles
- **Matplotlib** et **Seaborn** pour les visualisations
