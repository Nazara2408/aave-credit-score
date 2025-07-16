Credit Score Analysis — Aave V2 Wallets
This document presents the analysis of the credit scores assigned to Aave V2 user wallets using both **rule-based logic** and **machine learning prediction**. The goal is to understand the **distribution** of scores and the **behavioral patterns** of wallets across different score brackets.

Summary of Scoring Methodologies

Implemented two scoring strategies:

### 1. Rule-Based Model
- Custom-designed scoring function based on:
  - Total deposits, repaid borrowings, liquidations, inactivity
  - Time gaps between transactions to detect bot-like behavior
- Scaled scores between **0 and 1000**

### 2. ML-Based Model
- Trained a **RandomForestRegressor** using the rule-based scores as pseudo-labels
- Features: total transactions, deposit/borrow counts, repay ratios, avg txn gap
- Objective: mimic the rule logic with generalization
- Split: 80% training, 20% testing

Score Distribution (0–1000)
Interpretation of Rule based approach:

We divided the scores into 10 equal buckets and analyzed wallet counts in each.

| Score Range | Approx. # of Wallets | Remarks |
|-------------|----------------------|---------|
| 0–100       | ~5                   | Rare, extremely poor behavior |
| 100–200     | ~10                  | Very few, likely bots or spam wallets |
| 200–300     | ~20                  | Minimal interaction or immediate liquidation |
| 300–400     | ~40                  | Somewhat risky users |
| 400–500     | ~550                 | Inconsistent or inactive behavior |
| 500–600     | ~180                 | Normal/average users |
| 600–700     | ~750                 | Most common — reliable users |
| 700–800     | ~400                 | Good participation & responsibility |
| 800–900     | ~320                 | Very responsible users, low risk |
| 900–1000    | ~200                 | Top-tier — ideal wallet behavior |

Observation: 
The majority of wallets scored between 600 and 900, suggesting most users are responsible with their funds.  
Very few wallets are in the 0–300 danger zone, indicating bot-like or exploitative patterns are rare.



Interpretation of ML Distribution
-Smoother Transitions:
   Unlike the rule-based model, which showed hard spikes (especially at 600–700), the ML model creates gradual transitions and fills the gaps between score clusters.

-Flexibility:
   ML captured patterns in the features and generalized scoring logic, making it adaptable for unseen data.

-Top-end Bias Correction:
   ML avoids giving too many perfect (900–1000) scores, helping reduce false positives for high-credit users.


visual comparision between two:
| Credit Score Range | Rule-Based (Approx) | ML-Based (Approx) | Notes                                        |
| ------------------ | ------------------- | ----------------- | -------------------------------------------- |
| 0–300              | Very few (\~<50)    | Slightly higher   | ML model is more forgiving to sparse wallets |
| 400–500            | Spike (\~550)       | Similar spike     | ML matches this group well                   |
| 600–700            | Strong peak (\~750) | Similar shape     | Peak preserved by ML                         |
| 700–900            | Smooth, slight dip  | More distributed  | ML fills gaps between scores                 |
| 900–1000           | Flat (\~200)        | Lower than Rule   | ML avoids giving top score too easily        |


### Common Traits:
- Only 1–2 transactions
- Mostly withdrawals or liquidations
- No repayments
- Often zero time gap between txns (scripted bots)
- Very low total USD involvement


