# Zeru-Assignment


 Overview
This project builds a credit scoring system for wallets interacting with the Aave V2 protocol, using raw historical transaction-level data. Each wallet receives a credit score from 0 to 1000, reflecting its on-chain behavior — where higher scores represent more reliable, responsible, and human-like usage, while lower scores suggest riskier, bot-like, or exploitative patterns.

How It Works
1. Input Data
The system accepts a single JSON file:
user-wallet-transactions.json
Each entry represents a transaction with fields like:

userWallet: wallet address

action: deposit, borrow, repay, redeemunderlying, liquidationcall

actionData.amount: amount in raw units

actionData.assetPriceUSD: price in USD

timestamp

2. Feature Engineering
For each wallet, we extract features such as:

Total number of transactions

Total and average USD volume

Max transaction value

Action distribution (e.g., how many borrows vs repays)

Number of unique action types

Number of liquidation calls (penalized)

3. Scoring Logic
A weighted scoring heuristic is applied after min-max scaling the features:

Higher score: more deposits, repays, stable volume

⚠Lower score: frequent liquidations, only borrowing, low diversity

The score is computed using a dot product of normalized features and weights, then clipped to [0–1000].

 Output: wallet_scores.csv

A file with each wallet and its corresponding credit score.

📊 (Optional) A visualization of score distribution in analysis.md.

📂 Project Structure
graphql
Copy
Edit
aave-wallet-credit-scoring/
├── user-wallet-transactions.json        # Input JSON data
├── score_generator.py                   # One-step script to generate wallet scores
├── wallet_scores.csv                    # Output CSV with scores
├── analysis.md                          # Behavioral insights and distribution graph
└── README.md                            # This file
 Why This Matters
DeFi protocols have no credit systems. This scoring mechanism enables:

Early detection of risky actors or bots

Ranking users by reliability

Integrating on-chain credit into lending logic, trust-based access, and reputation systems

▶Usage (In Colab or Locally)
Place your user-wallet-transactions.json in the same directory

Run the script:

bash
Copy
Edit
python score_generator.py
Outputs: wallet_scores.csv with all scored wallets.

📌 Requirements
Python 3.8+

Libraries: pandas, numpy, scikit-learn, matplotlib, seaborn, tqdm

Install dependencies:

bash
Copy
Edit
pip install -r requirements.txt
