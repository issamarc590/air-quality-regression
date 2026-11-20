# Prédiction de la pollution de l'air à partir de capteurs à bas coût

Régression multi-sorties (CO, benzène, NOx, NO2) à partir des réponses de capteurs chimiques bon marché et de la météo, sur le jeu de données [UCI Air Quality](https://archive.ics.uci.edu/dataset/360/air+quality) (De Vito et al., 2008).

Projet réalisé dans le cadre du cours *Bases de l'apprentissage statistique* (M1 Mathématiques et applications, Sorbonne Université).

## Ce que montre ce projet

- **Méthodes implémentées avec NumPy** (sans bibliothèque d'apprentissage) :
  - moindres carrés et régression Ridge (forme fermée, intercept non pénalisé) ;
  - Ridge à noyau gaussien (noyau vectorisé, résolution par Cholesky) ;
  - réseau de neurones dense (rétropropagation, optimiseur Adam, arrêt précoce).
- **Vérification des implémentations** : comparaison avec scikit-learn (écart de l'ordre de 1e-14) et vérification numérique du gradient du réseau par différences finies.
- **Protocole d'évaluation rigoureux** : découpage chronologique train / validation / test, standardisation apprise sur le train uniquement, hyperparamètres choisis sur la validation, test utilisé une seule fois. Le notebook montre aussi qu'un découpage aléatoire surestime les performances sur ces séries temporelles.

## Lancer le notebook

```bash
pip install numpy pandas matplotlib scipy scikit-learn jupyter
```

Le fichier `AirQualityUCI.csv` est inclus dans le dépôt (jeu de données sous licence CC BY 4.0, De Vito et al., UCI Machine Learning Repository). Ouvrir `air_quality.ipynb` et exécuter toutes les cellules (moins d'une minute).

## Résultats principaux (ensemble de test, R²)

| Modèle | CO | Benzène | NOx | NO2 |
|---|---|---|---|---|
| Moindres carrés | 0,86 | 0,95 | 0,84 | 0,51 |
| Ridge | 0,86 | 0,95 | 0,84 | 0,51 |
| **Ridge à noyau gaussien** | **0,89** | **0,996** | **0,89** | **0,58** |
| Réseau de neurones | 0,89 | 0,99 | 0,87 | 0,57 |

NO2 est le plus difficile à prédire : son niveau augmente fortement en hiver, période absente des données d'entraînement (découpage chronologique).

scikit-learn sert uniquement à vérifier les implémentations (section 5).

## Structure du notebook

1. Chargement et nettoyage (valeurs manquantes codées par -200)
2. Analyse exploratoire
3. Préparation : variables, découpage chronologique, standardisation
4. Modèles
5. Vérification des implémentations
6. Choix des hyperparamètres
7. Évaluation finale sur le test
8. Conclusion, limites et pistes
