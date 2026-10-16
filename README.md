# Projet de Deep Learning : Classification de Corpus Wikipédia par Thème

Ce dépôt regroupe une série de travaux pratiques (TP) visant à constituer un corpus textuel automatisé à partir de Wikipédia, puis à concevoir, évaluer et optimiser un réseau de neurones artificiels pour classifier automatiquement ces textes selon leur thématique.

---

##  Vue d'ensemble du projet

Le projet est divisé en trois étapes principales :
1. **TP A (`tpa-collecter.ipynb`)** : Collecte automatisée et polie de données textuelles à l'aide de trois approches complémentaires : **API**, **Crawling** et **Scraping**.
2. **TP B (`b-premier-reseau.ipynb`)** : Construction, entraînement et évaluation d'un premier réseau de neurones multicouche pour prédire le thème d'un texte.
3. **TP C (`c-optimiser_2.ipynb`)** : Analyse des hyperparamètres, gestion du surapprentissage (Dropout, arrêt précoce) et quantification pour alléger le modèle en vue d'une mise en production.

---

##  1. Collecte des Données (TP A)

Le script `tpa-collecter.ipynb` permet de constituer un corpus propre et structuré (`corpus.csv`) à partir de l'encyclopédie Wikipédia en anglais, autour de **4 thèmes principaux** :
* Astronomie (`Astronomy`)
* Cuisine (`Cooking`)
* Peinture (`Painting`)
* Football (`Association football`)

### Techniques utilisées :
* **L'API Wikipédia** : Interrogation officielle et structurée au format JSON pour lister les articles d'une catégorie et gérer la pagination (`categorymembers`).
* **Le Crawling** : Exploration récursive de l'arbre des catégories de Wikipédia en suivant les liens hypertextes du HTML (`#mw-subcategories`).
* **Le Scraping** : Extraction ciblée du premier paragraphe significatif d'un article (`#mw-content-text`) tout en filtrant les infoboxes, les tableaux et les notes de bas de page (`[1]`, etc.).

### Bonnes pratiques respectées :
* Lecture et respect rigoureux du fichier `robots.txt` via `RobotFileParser`.
* Utilisation d'un `User-Agent` descriptif incluant un contact.
* Pauses régulières (`time.sleep`) pour éviter toute surcharge des serveurs.

---

##  2. Premier Réseau de Neurones (TP B)

Le carnet `b-premier-reseau.ipynb` introduit les bases du Deep Learning pour le traitement du langage naturel (NLP) :
* **Représentation (TF-IDF)** : Transformation des textes en vecteurs numériques de 2 000 dimensions (les 2 000 mots les plus discriminants), calculés strictement sur le jeu d'apprentissage pour éviter les fuites de données (*data leakage*).
* **Architecture du réseau** : 
  * Couche d'entrée (vecteur TF-IDF)
  * 1ère couche cachée Dense de **64 neurones** (activation `ReLU`)
  * 2ème couche cachée Dense de **32 neurones** (activation `ReLU`)
  * Couche de sortie à **4 neurones** (activation `softmax` pour obtenir une distribution de probabilités sur les thèmes).
* **Résultats** : Le réseau de neurones s'avère très performant (précision avoisinant les 90% sur le jeu de test), surpassant nettement les méthodes de référence basées sur le hasard, et se positionnant de manière compétitive face à une régression logistique.

---

##  3. Optimisation et Allègement (TP C)

Le carnet `c-optimiser_2.ipynb` aborde l'amélioration méthodique des performances et l'optimisation des modèles :
* **Hyperparamétres vs Paramètres** : Distinction claire entre les poids ajustés par le réseau et les réglages choisis par l'utilisateur (taux d'apprentissage, nombre de neurones, dropout, etc.).
* **Lutte contre le surapprentissage** : Mise en place du **Dropout** (extinction aléatoire de neurones) et de l'**Arrêt Précoce** (*Early Stopping*) pour stopper l'entraînement lorsque la perte de validation stagne.
* **Allègement par quantification (TFLite)** : Conversion du modèle au format TensorFlow Lite avec quantification des poids sur 8 bits entiers. 
  * **Résultat** : La taille du modèle est **divisée par 4** (passant d'environ 509 Ko à 131 Ko) sans perte notable de précision sur les textes de test, ce qui le rend idéal pour un déploiement embarqué ou sur le cloud.

---

##  Structure du dépôt

```text
├── tpa-collecter.ipynb       # Notebook de collecte (API, crawling, scraping)
├── b-premier-reseau.ipynb    # Notebook de création du premier réseau de neurones
├── c-optimiser_2.ipynb       # Notebook d'optimisation et d'allègement du modèle
├── corpus.csv                # Corpus final généré (239 textes classés)
└── corpus-secours.csv        # Corpus de secours par défaut
```

##  Lancement rapide

Pour exécuter les notebooks, assurez-vous d'avoir installé les dépendances nécessaires :

```
pip install pandas scikit-learn matplotlib tensorflow
```

Ouvrez ensuite Jupyter Lab ou Jupyter Notebook pour exécuter les cellules de manière séquentielle.
```
jupyter notebook
```
