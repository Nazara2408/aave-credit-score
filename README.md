# aave-credit-score
Credit scoring system for Aave V2 DeFi wallets

Overview:   
The goal was to assign a credit score between 0 and 1000 to each wallet interacting with the Aave V2 DeFi protocol, based on historical transaction behavior.  
A high score indicates trustworthy, consistent, and responsible usage, while a low score suggests risky, bot-like, or exploitative behavior.

Step-by-Step Workflow:
1️) Downloaded & Parse the Data

- Raw transaction data (~100K records) was provided in a large JSON file (87 MB).
- Each entry represents one transaction.
- Download link:  
  [user_transactions.json](https://drive.google.com/file/d/1ISFbAXxadMrt7Zl96rmzzZmEKZnyW7FS/view?usp=sharing)

2) Convert JSON → Pandas DataFrame
   The JSON data was parsed and converted into a pandas DataFrame using `json.load()` and `json_normalize()` methods. Each row represents one transaction performed by a wallet.

3) Feature Engineering
   
   Transactions were grouped by wallet address to extract user behavior metrics. The following features were generated for each wallet:
- `total_txns`
- `num_deposits`
- `num_borrows`
- `num_repays`
- `num_withdrawals`
- `num_liquidations`
- `total_deposit_usd`
- `total_borrow_usd`
- `total_repay_usd`
- `borrow_to_repay_ratio`
- `avg_txn_gap_seconds`

4) Method 1: Rule-Based Scoring:
   A rule-based credit score was calculated using a custom function that rewards or penalizes wallets based on their behavior. Example logic:

- More deposits → higher score
- High repayment rate → higher score
- More liquidations or one-time usage → penalty

Final scores were scaled between 0–1000 and saved to:

-> `wallet_scores.csv`

5) Method 2: ML Regressor Model

To automate and improve scalability, a supervised regression model (`RandomForestRegressor`) was trained using:

- Features: Wallet-level engineered metrics
- Labels: Rule-based credit scores (pseudo-labels)

Model evaluation was done using:

- MAE (Mean Absolute Error)
- RMSE (Root Mean Squared Error)
- R² Score

The ML model's predicted scores were saved in:

-> `wallet_scores_ml.csv`

6) Score Comparison:

A histogram comparison was plotted between:

- Rule-based scores
- ML-predicted scores

This helped in visually verifying the ML model’s effectiveness in replicating the rule logic.

PROJECT STRUCTURE: 

aave-credit-score/
├── data/
│ └── user_transactions.json   # Raw transaction data  
├── wallet_credit_score.ipynb  # Jupyter notebook with full code
├── wallet_scores.csv          # Rule-based scores
├── wallet_scores_ml.csv       # ML-based predicted scores

SUMMARY: 
- The rule-based model provides explainable scores (Better in this case as labelled data is not available).
- The ML model generalizes the rule logic and automates the scoring pipeline.
- The system can be extended with unsupervised models or real labels in future versions.

