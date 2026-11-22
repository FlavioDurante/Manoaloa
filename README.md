# Manoaloa — prévision hebdomadaire du CO₂ atmosphérique (Mauna Loa)

Projet d'école (2025). Trois modèles hybrides de prévision sur la série hebdomadaire de CO₂ mesurée à l'observatoire de Mauna Loa (NOAA), comparés à leurs composantes seules par MAE et RMSE.

Tout le travail est dans un seul notebook : [`manoaloa_durante_flavio.ipynb`](manoaloa_durante_flavio.ipynb) (exploration, modèles, évaluation).

## Résultats

Erreurs en ppm de CO₂, lues dans les sorties du notebook. Plus bas = mieux.

| Modèle | Découpage | Mode de prévision | MAE | RMSE |
|---|---|---|---|---|
| **1.** AR(14) saisonnier seul | 70 % train / 30 % test | multi-pas sur toute la période de test | 5,54 | 6,49 |
| **1.** AR(14) + Random Forest sur les résidus | idem | idem | 5,74 | 6,66 |
| **2.** Lissage exponentiel (Holt-Winters, saison 52 semaines) seul | 50 % / 25 % validation / 25 % test | multi-pas | **1,89** | **2,27** |
| **2.** Régression linéaire (mois en one-hot, termes polynomiaux) seule | idem | multi-pas | 4,34 | 5,05 |
| **2.** Méta-modèle linéaire sur les deux précédents | idem | multi-pas | 3,50 | 4,04 |
| **3.** LightGBM sans les sorties de Prophet | 50 % / 25 % / 25 % | **un pas en avant**, réentraîné à chaque semaine | 2,28 | 2,52 |
| **3.** LightGBM avec la tendance et la prévision de Prophet en variables | idem | idem | 0,76 | 0,98 |

Ce que ces chiffres montrent :

- **Modèle 1** : corriger l'AR par une forêt aléatoire entraînée sur ses résidus n'améliore pas la prévision (5,74 contre 5,54).
- **Modèle 2** : le méta-modèle fait mieux que la régression linéaire, mais **moins bien que le lissage exponentiel seul** (3,50 contre 1,89). Sur cette série, combiner n'a pas payé.
- **Modèle 3** : ajouter la tendance de Prophet en variable divise l'erreur du LightGBM par trois (0,76 contre 2,28).

Prévisions comparées à la série observée, une figure par modèle :

| Modèle 1 | Modèle 2 | Modèle 3 |
|---|---|---|
| ![Modèle 1](https://github.com/user-attachments/assets/e0ecb7ff-1b8b-4706-bd13-6734e2915f8d) | ![Modèle 2](https://github.com/user-attachments/assets/88e9e521-9191-48d6-8d91-9676ab5af81c) | ![Modèle 3](https://github.com/user-attachments/assets/6d1d6657-7ba5-41b5-ab5c-e9c00218b139) |

## Limites

- **Les lignes ne sont pas comparables entre modèles.** Les découpages diffèrent (le test du modèle 1 commence plus tôt que celui des modèles 2 et 3), et le modèle 3 prévoit une seule semaine à la fois en voyant les valeurs réelles jusqu'à la veille, alors que les modèles 1 et 2 prévoient toute la période d'un coup. Le 0,76 du modèle 3 ne veut donc pas dire qu'il est le meilleur.
- Aucune référence naïve (même semaine l'année précédente, ou dernière valeur) n'a été calculée : on ne sait pas de combien chaque modèle bat la solution la plus simple.
- Un seul découpage train / test par modèle, sans validation glissante ni intervalle de confiance sur les erreurs.
- Le notebook a été exécuté dans le désordre par endroits (la comparaison avec et sans Prophet réutilise une prévision calculée plus tôt) : une ré-exécution de haut en bas peut donner des chiffres légèrement différents.

## Données

- **Source** : NOAA Global Monitoring Laboratory, [Trends in Atmospheric CO₂](https://gml.noaa.gov/ccgg/trends/data.html), fichier hebdomadaire de Mauna Loa (`co2_weekly_mlo.txt`), téléchargé par le notebook.
- **Période** : à partir de mai 1975, une valeur par semaine.
- **Nettoyage** : valeurs manquantes complétées par la valeur précédente, colonne `10 yr ago` retirée.

| Variable | Unité | Description |
|---|---|---|
| `year`, `month`, `day` | — | Date de la mesure |
| `year_decimal` | années | Date en année décimale (ex. 1975,3959) |
| `co2` | ppm | Concentration moyenne de la semaine (variable prévue) |
| `days` | jours | Nombre de jours de mesure dans la semaine |
| `1 yr ago` | ppm | Valeur un an plus tôt |
| `since 1800` | ppm | Écart avec le niveau préindustriel |

## Reproduire

```bash
git clone https://github.com/FlavioDurante/Manoaloa.git
cd Manoaloa
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn lightgbm prophet
jupyter lab manoaloa_durante_flavio.ipynb
```

Les versions des bibliothèques n'ont pas été figées au moment du projet.

## Licence

MIT, voir [`LICENSE`](LICENSE). Données : NOAA GML, libres de réutilisation avec citation de la source.
