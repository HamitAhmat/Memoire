# Une alternative aux modèles financiers du CAPM et de Fama-French : l'approche du Best Subset Regression

[![Mémoire complet](https://img.shields.io/badge/Mémoire-PDF-blue?style=for-the-badge)](#)

[![Note de synthèse](https://img.shields.io/badge/Note%20de%20synthèse-PDF-green?style=for-the-badge)](#)

[![Dashboard R Shiny](https://img.shields.io/badge/Dashboard-R%20Shiny-orange?style=for-the-badge)](#)


---

## Contexte et objectifs

Ce projet s'inscrit dans le cadre d'un mémoire de **Master 1 Économétrie et Statistique Appliquées** (parcours Économétrie Appliquée), Université de Nantes. L'objectif principal est d'appliquer la **Best Subset Regression (BSR)** comme méthode alternative de sélection de facteurs de risque en évaluation des actifs financiers, et d'évaluer sa capacité à identifier un modèle factoriel plus parcimonieux et plus performant que les modèles classiques (CAPM, Fama-French).

La prolifération des facteurs de risque dans la littérature, phénomène qualifié de *factor zoo* par Cochrane (2011), constitue le point de départ de ce travail. En explorant exhaustivement les **4 095 sous-modèles possibles** parmi 12 facteurs candidats, la BSR identifie automatiquement le modèle optimal et compare ses résultats avec ceux de l'approche bayésienne de Chib, Zhao et Zhou (2023, *Management Science*).

---

## Données

### Sources

Les données sont celles de **Chib, Zhao et Zhou (2023)**, mises à disposition par les auteurs :

- **`factor12.RData`** : rendements mensuels des 12 facteurs de risque (janvier 1974 à décembre 2018)
- **`industry49ex.RData`** : rendements excédentaires mensuels des 49 portefeuilles industrie de Kenneth R. French

### Variables clés

| Catégorie | Facteurs | Description |
|-----------|----------|-------------|
| **Fama-French (FF6)** | Mkt, SMB, HML, RMW, CMA, MOM | Marché, taille, valeur, rentabilité, investissement, momentum |
| **Hou-Xue-Zhang (HXZ)** | IA, ROE | Investissement et rentabilité des capitaux propres |
| **Stambaugh-Yuan (SY)** | MGMT, PERF | Mauvaise valorisation liée à la gestion et à la performance |
| **Daniel-Hirshleifer-Sun (DHS)** | PEAD, FIN | Facteurs comportementaux court et long horizon |

### Période et échantillons

- **Période d'estimation** : Janvier 1979 à Décembre 2018 (T = 480 observations mensuelles)
- **Actifs tests** : 49 portefeuilles industrie (Kenneth R. French)

---

## Méthodologie

### 1. Analyse exploratoire

Statistiques descriptives des 12 facteurs, matrice de corrélations et visualisation des rendements cumulés par collection.

### 2. Modèles de référence

Quatre modèles sont estimés sur les 49 portefeuilles industrie par régression MCO et évalués par le **test GRS** (Gibbons, Ross et Shanken, 1989) :

| Modèle | Facteurs |
|--------|----------|
| CAPM | Mkt |
| FF3 | Mkt, SMB, HML |
| FF5 | Mkt, SMB, HML, RMW, CMA |
| FF6 | Mkt, SMB, HML, RMW, CMA, MOM |

### 3. Best Subset Regression

La BSR est appliquée **portefeuille par portefeuille** via les packages R `bestglm` (critères AIC et BIC) et `leaps` (vérification de cohérence). La cohérence entre les deux packages est parfaite : **49/49 portefeuilles donnent les mêmes résultats**.

Deux seuils de sélection sont appliqués :

- **Seuil 50 %** : facteurs sélectionnés dans au moins la moitié des portefeuilles, ce qui est équivalent au FF3
- **Seuil 30 %** : facteurs sélectionnés dans au moins 30 % des portefeuilles, soit 8 facteurs retenus

---

## Résultats

### Performances comparées

| Modèle | Nb fact. | Stat. GRS | P-valeur | \|α\| moyen (%) | Sharpe |
|--------|----------|-----------|----------|-----------------|--------|
| CAPM | 1 | 1.200 | 0.1763 | 0.192 | 0.513 |
| FF3 | 3 | 1.776 | 0.0016 | 0.223 | 0.633 |
| FF6 | 6 | 1.958 | 0.0002 | 0.254 | 1.153 |
| **BSR (30 %)** | **8** | **1.808** | **0.0011** | **0.221** | **1.275** |

### Résultats clés

- Au seuil de 50 %, la BSR retrouve spontanément le **modèle FF3**, confirmant sa robustesse empirique
- Au seuil de 30 %, **5 facteurs sur 8** convergent avec le modèle optimal de Chib et al. (2023) : Mkt, SMB, MOM, MGMT et PERF
- La régression sur le portefeuille moyen équipondéré produit un **R² ajusté de 0,9638** avec un alpha annualisé non significatif (-0,964 %, p-valeur = 0,072)

> 📊 **Accéder au dashboard interactif** : [Lien R Shiny](#)

---

## Limites et perspectives

### Limites

- **Biais de post-sélection** : les erreurs standard sont sous-estimées car le modèle est évalué sur les mêmes données que celles utilisées pour la sélection.
- **Absence d'évaluation hors échantillon** : les facteurs retenus sont optimaux pour 1979-2018 sans garantie sur des données futures.
- **Portée limitée** : analyse restreinte aux portefeuilles industrie du marché américain.

### Perspectives

- Évaluation hors échantillon sur **fenêtres glissantes** (*rolling window*)
- Comparaison avec des méthodes de régularisation (**Lasso**, Elastic Net)
- Extension à d'autres **marchés géographiques** (Europe, Asie, marchés émergents)

---

## Encadrement

**Directeur de mémoire** : Monsieur Olivier Darné  
**Formation** : Master Économétrie et Statistique Appliquées, parcours Économétrie Appliquée, Université de Nantes  
**Année universitaire** : 2025-2026
