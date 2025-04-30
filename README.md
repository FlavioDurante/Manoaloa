# Manoaloa 

**Manoaloa** utilise des techniques de machine pour prévoir l'évolution du co2 dans l'autmosphere. 

Les données prevenant de l'observatoir astronomique du gouvernement Américain. 

Dans ce projet nous esseyerons 3 Models hybrides differents. Certeins avec la technique iterative forecast d'autre à travers la technique du rolling window.

## Table des matières
1. [Présentation](#présentation)
2. [Données](#données)
3. [Structure du projet](#structure-du-projet)
4. [Installation & Usage](#installation--usage)
5. [Modèles entraînés](#modèles-entraînés)
6. [Résultats](#résultats)
7. [Licence](#licence)

## Données
- **Source** : Global Monitoring Laboratory, accessible via ce lien https://gml.noaa.gov/ccgg/trends/data.html

| Nom de la variable             | Type de donnée | Unité / Mesure                 | Description                                                               |
|--------------------------------|----------------|--------------------------------|---------------------------------------------------------------------------|
| `year_month_day`               | Qualitatif     | Date (YYYY-MM-DD)               | Date précise de l’observation                                           |
| `year`                         | Quantitatif    | —                              | Année de l’observation (ex. 1975)                                        |
| `month`                        | Quantitatif    | —                              | Mois de l’observation (1 = janvier, …, 12 = décembre)                    |
| `day`                          | Quantitatif    | —                              | Jour du mois de l’observation                                            |
| `year_decimal`                 | Quantitatif    | Années décimales               | Année exprimée en fraction décimale (p. ex. 1975.3959)                   |
| `co2`                          | Quantitatif    | ppm                            | Concentration de CO₂ mesurée (parties par million)                       |
| `days`                         | Quantitatif    | Jours                          | Nombre de jours écoulés depuis la mesure précédente                      |
| `1 yr ago`                     | Quantitatif    | ppm                            | Valeur de CO₂ mesurée exactement un an avant                             |
| `since 1800`                   | Quantitatif    | ppm                            | Écart de la concentration de CO₂ par rapport à la valeur de 1800 (baseline) |


## Structure du projet
├── manoaloa_durante_flavio.ipynb # Full project, EDA + Models 

## Installation & Usage
```bash
git clone https://…/Manoaloa.git

```

## Modèles entraînés
Logistic Regression

AR + RF sur Residus (iterative forecast)

ES + LR -> Meta model (LR)

Prophet + light gbm (rolling window)

## Résultats

Model 1

![1](https://github.com/user-attachments/assets/e0ecb7ff-1b8b-4706-bd13-6734e2915f8d)

Model 2

![2](https://github.com/user-attachments/assets/88e9e521-9191-48d6-8d91-9676ab5af81c)

Model 3

![3](https://github.com/user-attachments/assets/6d1d6657-7ba5-41b5-ab5c-e9c00218b139)

## Licence
Ce projet est sous licence MIT.

