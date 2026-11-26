# EPISEN — Introduction au Big Data avec Databricks

Supports de TP pour élèves ingénieurs bac+4 débutants. **Données entièrement synthétiques, sans information de santé réelle.**

## Parcours pédagogique

| Notebook | Sujet | Durée indicative |
|---|---|---:|
| `notebooks/00_decouverte_sql_parquet.ipynb` | CSV, schéma, SQL, Parquet | 35–50 min |
| `notebooks/01_transformations_dataframe.ipynb` | DataFrames, `NULL`, filtres, agrégats, partitions | 40–55 min |
| `notebooks/02_qualite_jointures_sante.ipynb` | Qualité, anomalies, jointure de données hospitalières | 55–70 min |
| `notebooks/03_mini_pipeline_bronze_silver_gold.ipynb` | Bronze, Silver, Gold, indicateurs, restitution | 50–70 min |

Les deux notebooks originaux reçus sont archivés dans `legacy/`. Les versions pédagogiques sont dans `notebooks/`.

## Déployer sur GitHub puis synchroniser avec Databricks

1. Créez un dépôt GitHub vide, puis **décompressez et poussez le contenu de ce dossier** à la racine du dépôt (`README.md`, `data/`, `notebooks/`).
2. Dans Databricks : **Settings → Linked accounts / Git credentials** (libellé selon workspace) et connectez GitHub. Voir la documentation Git folders.
3. Dans **Workspace**, choisissez **Create → Git folder**, renseignez l'URL HTTPS du dépôt et le fournisseur GitHub.
4. Ouvrez `notebooks/00_decouverte_sql_parquet.ipynb`, puis connectez un compute Spark. Les notebooks lisent les petits CSV versionnés dans `data/` depuis le driver Python.
5. Pour récupérer de futures modifications GitHub, utilisez **Pull** dans l'interface Git du dossier. Ce n'est pas une synchronisation permanente automatique.

Docs : https://docs.databricks.com/aws/en/repos/git-operations-with-repos et https://docs.databricks.com/aws/en/notebooks/notebook-format

## Prérequis et chemins

- Les notebooks supposent **Databricks Runtime 14+** pour que `Path.cwd()` pointe vers le dossier du notebook dans le Git folder. En environnement différent, adapter `DATA_DIR` dans la première cellule.
- Pour les **étapes d'écriture uniquement**, il faut un **Unity Catalog Volume** existant, accessible en écriture. Modifier `OUTPUT_ROOT` dans chaque notebook : `"/Volumes/<catalog>/<schema>/<volume>/<identifiant_etudiant>"`.
- Créer un dossier **distinct par étudiant** : ne jamais écrire à la racine du volume partagé. `overwrite` remplace le dossier cible.
- Si aucun volume n'est disponible, sauter les cellules de sauvegarde : tous les exercices de nettoyage / jointure / agrégations fonctionnent sans écriture.
- La démonstration utilise `csv.DictReader` côté driver parce que les fichiers du dépôt sont minuscules. En entreprise, Spark lit les fichiers de production directement depuis le Data Lake.

## Structure

```text
EPISEN_Big_Data_TP/
├── README.md
├── data/
│   ├── employes_demo.csv
│   ├── patients_synthetiques.csv
│   └── admissions_synthetiques.csv
├── notebooks/
│   ├── 00_decouverte_sql_parquet.ipynb
│   ├── 01_transformations_dataframe.ipynb
│   ├── 02_qualite_jointures_sante.ipynb
│   └── 03_mini_pipeline_bronze_silver_gold.ipynb
└── legacy/
    ├── spark_tp_0_original.ipynb
    └── spark_tp_1_original.ipynb
```

## Pour l'enseignant

Les notebooks sont **guidés**, pas des examens. Les questions peuvent être transformées en cellules `TODO` avant distribution. Les données contiennent volontairement : un doublon exact, un patient inexistant, un service manquant et une durée négative. Les étudiants doivent compter les anomalies avant de les exclure.

**Limite volontaire :** ce mini-projet est une simulation de couches Bronze/Silver/Gold ; il ne configure pas des jobs orchestrés ni des tables Delta de production. Les notebooks 0 et 1 sont proches des TPs initiaux, avec correction des types et des chemins d'écriture.
