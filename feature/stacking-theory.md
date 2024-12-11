# 🎓 Théorie du Stacking en Machine Learning

## 1. Qu'est-ce que le Stacking ?

Le stacking est une méthode d'ensemble learning qui combine plusieurs modèles de prédiction. Imaginez une analogie avec un diagnostic médical :

```
                 ┌─── Cardiologue ───┐
Patient ────────┼─── Radiologue ────┼──> Médecin Senior ──> Diagnostic Final
                 └─── Généraliste ───┘
                    (Experts)           (Combinaison)
```

De la même manière, en stacking :
```
                 ┌─── Modèle 1 ───┐
Données ────────┼─── Modèle 2 ────┼──> Méta-Modèle ──> Prédiction Finale
                 └─── Modèle 3 ───┘
               (Modèles de Base)     (Combinaison)
```

## 2. Architecture du Stacking

La structure se compose de deux niveaux :

```
Niveau 1 (Base Learners):             Niveau 2 (Meta Learner):
┌─────────────────┐
│ Ridge           │
├─────────────────┤     ┌────────────────┐
│ Lasso           │────>│                │
├─────────────────┤     │   Meta-Modèle  │─── Prédiction
│ RandomForest    │────>│                │    Finale
├─────────────────┤     │                │
│ GradientBoost   │────>│                │
└─────────────────┘     └────────────────┘
```

## 3. Processus de Validation Croisée

Pour éviter le surapprentissage, on utilise la validation croisée :

```
Données d'entraînement complètes:
[==========================================]

Fold 1:
[🔵🔵🔵🔵🔴][🔵🔵🔵🔵🔵]
Training    Test

Fold 2:
[🔵🔵🔵🔵🔵][🔴🔵🔵🔵🔵]
Training    Test

Fold 3:
[🔵🔵🔴🔵🔵][🔵🔵🔵🔵🔵]
Training    Test

...

🔵 = Données d'entraînement
🔴 = Données de validation
```

## 4. Génération des Meta-Features

Le processus de création des meta-features :

```
Données Originales    Prédictions        Meta-Features
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│ Feature 1    │   │ Pred_Model1  │   │ Feature 1    │
│ Feature 2    │   │ Pred_Model2  │   │ Feature 2    │
│ Feature 3    │──>│ Pred_Model3  │──>│ Pred_Model1  │
│ ...          │   │ Pred_Model4  │   │ Pred_Model2  │
└──────────────┘   └──────────────┘   │ ...          │
                                      └──────────────┘
```

## 5. Flux des Données dans le Stacking

```
┌─────────────┐
│   Données   │
│  Originales │
└─────┬───────┘
      │
      ▼
┌─────────────┐    ┌─────────────┐
│  Modèle 1   │───>│Prédiction 1 │
└─────────────┘    └──────┬──────┘
                          │
┌─────────────┐    ┌─────▼──────┐
│  Modèle 2   │───>│   Meta-    │
└─────────────┘    │  Features  │
                   └──────┬──────┘
┌─────────────┐          │
│  Modèle 3   │───>     │
└─────────────┘         ▼
                  ┌──────────────┐
                  │Meta-Modèle   │
                  └──────┬───────┘
                         │
                         ▼
                  ┌──────────────┐
                  │  Prédiction  │
                  │    Finale    │
                  └──────────────┘
```

## 6. Avantages du Stacking

```
Forces du Stacking:
┌────────────────────┐
│   Diversité des    │     ┌────────────────┐
│      Modèles       │────>│                │
└────────────────────┘     │   Meilleures   │
                          │  Prédictions   │
┌────────────────────┐    │                │
│   Réduction du     │────>│                │
│  Surapprentissage  │     └────────────────┘
└────────────────────┘
```

## 7. Types de Modèles Utilisés

```
Modèles de Base:
┌───────────────────────┐
│ Ridge (Régression)    │  - Régularisation L2
├───────────────────────┤
│ Lasso (Régression)    │  - Régularisation L1
├───────────────────────┤
│ ElasticNet            │  - L1 + L2
├───────────────────────┤
│ Random Forest         │  - Arbres de décision
├───────────────────────┤
│ Gradient Boosting     │  - Boosting séquentiel
└───────────────────────┘

Meta-Modèle:
┌───────────────────────┐
│ Régression Linéaire   │  - Combinaison finale
└───────────────────────┘
```

## 8. Cycle d'Apprentissage

```
   ┌──────────── Entraînement ───────────┐
   │                                     │
   ▼                                     │
Données ──> Prétraitement ──> Modèles ───┘
                                │
                                ▼
                          Prédictions
                                │
                                ▼
                          Meta-Features
                                │
                                ▼
                          Meta-Modèle
                                │
                                ▼
                       Prédiction Finale
```

