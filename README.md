# Wallet-Risk-Scoring-From-Scratch
# 🛡️ Ethereum Wallet Risk Scoring – Rank-Based Model (0–1000 Scale)

This project assigns a **risk score** to Ethereum wallets using only publicly available **Etherscan transaction data**. 
It combines behavioral, transactional, and interaction-based features to classify wallets by trustworthiness and potential risk.

---
The list of wallet addresses used for scoring is also uploaded as a csv file.

## 🔍 Overview

The risk scoring algorithm evaluates wallets based on:

- **Transaction frequency**
- **Gas usage**
- **Failed transactions**
- **Wallet age**
- **Interaction diversity**
- **ETH transfer volume**
- **Interaction entropy**

### ✅ Higher Score = Lower Risk

| Score Range | Risk Level  | Meaning                              |
|-------------|-------------|--------------------------------------|
| 701–1000    | 🟢 Low Risk  | Trusted wallet, low failure, aged    |
| 301–700     | 🟡 Medium    | Some red flags, moderate usage       |
| 0–300       | 🔴 High Risk | Risky behavior, new, frequent fails  |

---

## ⚙️ Features Extracted from Etherscan

| Feature                 | Description                                     |
|------------------------|-------------------------------------------------|
| `wallet_age_days`      | How long ago the wallet first made a txn        |
| `num_txns`             | Total transaction count                         |
| `total_value_eth`      | Total ETH sent/received                         |
| `total_gas_used`       | Sum of gas used across all transactions         |
| `failed_txns`          | Count of failed transactions                    |
| `unique_contracts`     | Unique contracts the wallet interacted with     |
| `interaction_entropy`  | Entropy of contract interactions (diversity)    |

All features are normalized using **rank-based percentile scaling**, which ensures robust comparisons even when data is skewed.

---

## 🧮 Scoring Formula (weights)

```python
score = (
    0.15 * wallet_age_rank +
    0.15 * txn_count_rank +
    0.20 * total_value_eth_rank +
    0.10 * gas_used_rank +
    0.20 * failed_txns_rank +
    0.10 * unique_contracts_rank +
    0.10 * interaction_entropy_rank
) * 1000
