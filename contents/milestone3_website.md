# Milestone 3: Modeling and Evaluation

## Modeling question

**Can a machine-learning model predict a player's next fantasy football score more accurately than ESPN, and does training separate models for groups of positions outperform one global model?** Our broader goal is to help fantasy managers make more informed weekly start/sit decisions.

We predict **fantasy points in the player's next game played**, not necessarily the next calendar week. A game's feature row uses information from the player's prior game and context associated with the game being predicted.

## Methods and rationale

We evaluated three approaches:

| Approach | Model and inputs | Why compare it? |
|---|---|---|
| **ESPN** | Published projected fantasy points | A practical benchmark fantasy managers already use |
| **Global XGBoost** | One boosted-tree regressor for QB, RB, WR, TE and K; **97 features** | Tests whether players benefit from one shared model with richer historical patterns |
| **Position-grouped XGBoost** | Three saved regressors: **RB (29 features)**, **QB + TE (40)**, **WR + K (47)** | Tests whether focusing on related positions improves predictions |

The three position groups were informed by exploratory multiple linear regression (MLR) and hierarchical clustering of five positions using similarities in their standardized game-context coefficients. These clusters are exploratory: the MLR relationships were weak, and the WR + K grouping has limited football intuition.

XGBoost was chosen for its ability to work with mixed numerical and categorical features, missing values, and nonlinear relationships. The global model includes season-to-date and career averages plus recent three-game averages. The grouped models primarily use position-specific recent box-score statistics and next-game context. **Because the feature sets differ, this evaluation compares two existing modeling pipelines; it does not isolate clustering as the only cause of performance differences.**

## Evaluation design

We preserved chronological order instead of randomly splitting player-games:

| Partition | Seasons / weeks | Use |
|---|---|---|
| Training | 2024 and 2025 weeks 1–9 | Fit XGBoost models |
| Validation | 2025 weeks 10–18 | Tune settings and stop boosting |
| Testing | 2026, predicting weeks 2–9 | Evaluate saved models on later games |

Using `analysis.ipynb`, we matched predictions on **player ID and predicted week** and verified that both model pipelines agreed on actual fantasy points. The final comparison includes **3,163 player-games from 562 players**: WR 1,151; TE 750; RB 740; QB 286; K 236. All three approaches were scored on the **same observations**, and no models were retrained for the analysis.

We report **MAE** (average absolute error in fantasy points; primary metric), **RMSE** (more sensitive to large errors), and **R²** (variance explained). Lower MAE and RMSE and higher R² are better.

## Key results

| Predictor | Player-games | MAE ↓ | RMSE ↓ | R² ↑ |
|---|---:|---:|---:|---:|
| **ESPN projections** | 3,163 | **3.868** | **5.504** | **0.496** |
| Global XGBoost | 3,163 | 4.376 | 5.947 | 0.412 |
| Combined position-grouped XGBoost | 3,163 | 4.623 | 6.130 | 0.375 |

![Bar chart comparing the mean absolute errors of ESPN, Global XGBoost, and position-grouped XGBoost](milestone3_model_comparison.png)

*Figure: Mean absolute error on the same 3,163 matched 2026 player-games. Lower is better.*

**ESPN is the strongest predictor in this test.** Its MAE is 0.508 fantasy points lower than Global XGBoost. Among our learned models, **Global XGBoost is better**: its MAE is 0.247 points lower than the combined grouped approach, about a **5.3% reduction relative to the grouped MAE**.

A player-game head-to-head comparison also favors ESPN: Global XGBoost was closer than ESPN on **1,265 of 3,163 games (40.0%)**; grouped XGBoost was closer on **1,206 (38.1%)**. The global model achieved a lower weekly MAE than the grouped models in every evaluated 2026 week, according to the saved analysis.

## Interpretation and examples

The global model's richer historical features may help smooth noisy week-to-week production. The grouped models instead rely more heavily on one game's usage statistics. Feature-importance results from the saved notebooks also show that **player identity** is highly influential in all four models; the global model additionally relies heavily on season and career scoring averages. These are explanations of model behavior, **not evidence of causation**.

The models often predict near a player's typical score and struggle with unusually high or low performances. For example, Jahmyr Gibbs scored **41.4** points in 2026 Week 3, while Global XGBoost predicted about **18.3**. However, individual examples are illustrative rather than a basis for ranking models; the matched test set supports the overall conclusion.

**Model choice:** Among the trained models, we favor **Global XGBoost** because it performs better overall and requires only one model and one input pipeline. Nevertheless, **neither learned approach beats ESPN**, so it would be premature to recommend either as a replacement for ESPN's projections in real start/sit decisions.

## Limitations and next steps

- **Not a controlled clustering experiment:** The global and grouped models differ in historical features and early-stopping settings. To determine whether clustering alone helps, train each approach with comparable features and tuning.
- **Potential information leakage:** The grouped models use season-level defensive ranking features that incorporate later games. Both pipelines also use recorded game-day weather rather than necessarily available pregame forecasts. These inputs need to be rebuilt or removed for a deployable pregame system.
- **Availability is not modeled:** The target jumps to a player's next appearance, excluding inactive or missed games. The evaluation therefore does not assess the risk of starting a player who will not play.
- **Limited generalization and uncertainty:** The results cover one 2026 test period, with no confidence intervals, new-player evaluation, or start/sit ranking metric. Future work should evaluate ranking accuracy and use a player-level paired bootstrap for uncertainty.
- **Model-version consistency:** The source report notes a mismatch between the global model used in `analysis.ipynb` (450 boosting rounds, MAE 4.376) and another committed model version (411 rounds, reported MAE 4.410). These website results reproduce the **450-round analysis version** and should be rerun after the team selects one final model artifact.

**Reproducibility:** See `analysis.ipynb`, `notebooks/XGBOOST.ipynb`, `notebooks/XGBoost_cluster.ipynb`, and `notebooks/Modeling_data_prep.ipynb` for the saved-model evaluation, training and exploratory grouping methods.
