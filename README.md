
# AI Engineer Project 3 – Nettoyage & Exploration des données Open Food Facts

## 1. Contexte

L’agence **Santé Publique France** souhaite améliorer la base de données open‑source **Open Food Facts**.  
Le besoin : réduire les erreurs de saisie en proposant un système de **suggestion / auto‑complétion** lors de la création de fiches produit.

Ce dépôt contient **l’étude de faisabilité** : nettoyage, exploration et imputation des données, réalisée avec Python.

## 2. Jeu de données

| | Valeur |
|---|---|
| Source | Dump Open Food Facts (TSV) |
| Taille brute | 320 772 lignes × 162 colonnes |
| Variable cible | `pnns_groups_1` (71,5 % de valeurs manquantes) |

## 3. Pipeline de traitement

| Étape | Description | Référence |
|-------|-------------|-----------|
| Pré‑traitement | Filtre *France*, suppression valeurs manquantes & doublons → **55 313 lignes / 11 features** | `preprocess_data()` |
| Nettoyage qualité | Corrections nutritionnelles (graisses, sucres, sodium), vérification somme ≤ 100 g | `detect_…`, `correct_…` |
| Valeurs aberrantes | Règles métier + IQR automatisé | `handle_outliers()` |
| Analyse EDA | Histogrammes, box/violin plots, pairplot, corrélations | section *EDA* |
| Tests ANOVA | 6 nutriments vs `pnns_groups_1`, p \< 0 ,05 | `statsmodels` |
| PCA | Standardisation puis ACP (PC1 29 %, PC2 23 %) | `sklearn.PCA` |
| Imputation KNN | Sélection features corrélées ≥ 0,5 ; optimisation *k* → **MSE 0,0325** à _k_=4, **Accuracy 0,612 / F1 0,458** à _k_=20 | `KNNImputer` |
| Jeu final | 50 382 lignes × 12 colonnes prêtes pour la modélisation | — |

## 4. Résultats et enseignements

* Pipeline **100 % automatisé** : il s’adapte aux mises à jour du dump.  
* Tests statistiques & PCA confirment la pertinence d’un modèle de suggestion basé sur la similarité nutritionnelle.  
* Imputation KNN réduit drastiquement les valeurs manquantes sans dégrader la distribution cible.  

## 5. Structure du dépôt

```
.
├── .gitignore
├── notebook_avec_fonction.ipynb   # Notebook principal
├── Projet 3.pdf                   # Slides de présentation
├── variables.txt                  # Dictionnaire des variables clés
└── README.md
```

## 6. Installation rapide

```bash
git clone https://github.com/Krock13/AI_Engineer_Project_3_Open_Food_Facts.git
cd AI_Engineer_Project_3_Open_Food_Facts
python -m venv .venv && source .venv/bin/activate  # ou `venv\Scripts\activate` sous Windows
pip install pandas numpy matplotlib seaborn scikit-learn statsmodels plotly
```

## 7. Reproduire l’étude

1. Télécharger le dernier dump Open Food Facts (`*.tsv`) et le placer à la racine.  
2. Ouvrir `notebook_avec_fonction.ipynb` et exécuter toutes les cellules.  
3. Les chemins vers le fichier peuvent être ajustés dans la 1ʳᵉ cellule.

## 8. Bonnes pratiques & conformité

* Code factorisé en fonctions ré‑utilisables.  
* Aucune donnée personnelle n’est manipulée : conformité de principe RGPD.

---

*Merci pour votre intérêt.*  
