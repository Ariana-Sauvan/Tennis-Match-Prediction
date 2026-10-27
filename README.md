# Tennis Match Prediction (ML)

Predicting the winner of ATP matches with machine learning, using only
information available before the match. Compares a Decision Tree
(scikit-learn) with LightGBM against two baselines.

## Key result
Both models reach ~70.7% accuracy on 2026 matches, which matches the
"bet on the bookmaker's favourite" baseline (70.7%). Most of the predictive
signal is already in the betting odds; errors concentrate on evenly matched games.

## Data
Yearly Excel files from [Tennis-Data](http://www.tennis-data.co.uk/) (ATP, 2023-2026).
Download them and place them in `data/` (see "How to run").

- Train: matches up to 2025 — Test: matches of 2026 (temporal split, no leakage)

## Method
1. Cleaning: removal of rows with missing rankings/odds; removal of
   post-match columns to prevent data leakage.
2. Restructuring: each match as "Player 1 vs Player 2" with random assignment.
3. Features: `rank_diff`, `pts_diff`, `prob_diff` (implied probabilities from odds),
   surface, tournament series, court, best-of.
4. Models: Decision Tree and LightGBM, tuned with GridSearchCV (3-fold).

## Results
| Model | Accuracy | AUC-ROC |
|---|---|---|
| Majority class | 0.532 | – |
| Bookmaker favourite | 0.707 | – |
| Decision Tree | [0.7xx] | 0.765 |
| LightGBM | [0.7xx] | 0.767 |

![Feature importance](results/feature_importance.png)

## Error analysis
Error rate by series: Grand Slam 22.5%, ATP 500 27.3%, ATP 250 32.4%,
Masters 1000 33.1%. Wrong predictions concentrate where `prob_diff ≈ 0`.

## Limitations & next steps
Odds already encode most of the information. Possible improvements:
recent form per surface, fatigue/rest days, head-to-head, injuries.

## How to run
pip install -r requirements.txt
Place `2023.xlsx` … `2026.xlsx` in `data/`, then run the notebook.

## Repository structure
├── tennis_prediction.ipynb
├── report/Tennis_Match_Prediction_Report.pdf
├── requirements.txt
└── README.md
