# CrediSync

### Advanced Decentralized AI-Driven Credit Risk Scoring & Lending Protocol

---

## I. Executive Summary

I have engineered **CrediSync** to serve as a high-fidelity bridge between algorithmic risk assessment and automated on-chain liquidity. Traditional DeFi lending models often suffer from capital inefficiency due to static over-collateralization. I designed CrediSync to solve this by implementing a **Dynamic Weighted Scoring Model**.

By leveraging three primary risk vectors—collateralization depth, historical performance, and repayment velocity—the protocol calculates a real-time "Credit Worthiness" score. This score is not just a metric; it is an active variable that determines maximum loanable amounts, interest rate tiers, and repayment durations. This creates a feedback loop that rewards positive borrower behavior and penalizes default risk with surgical precision.

---

## II. Architectural Overview

The system architecture is divided into three logical layers:

1. **The Profile Layer:** Manages persistent borrower identities, tracking every success and failure to build a long-term reputation.
2. **The AI Governance Layer:** Allows an administrative agent to update model weights () to react to macro-market shifts.
3. **The Execution Layer:** Handles the logic for loan issuance, status transitions, and collateral liquidation.

---

## III. Detailed Function Specifications

### A. Private Functions (Internal Logic)

I have implemented several internal helpers to ensure the protocol remains modular and secure. These functions are encapsulated to prevent external manipulation.

* `check-not-paused`: I use this as a global gatekeeper. It verifies the contract's operational status before allowing state-changing transactions.
* `calculate-enhanced-score`: The heart of the AI engine. It performs the following calculation:
* **Normalizes Collateral:** Scales micro-STX values to a 0–100 range.
* **Scales Repayment:** Rewards users based on the quantity of successfully closed loans.
* **Applies Default Penalty:** Subtracts 20 points per default, ensuring that one failure significantly impacts future borrowing power.


* `create-loan`: An internal factory function that increments the global `next-loan-id` and initializes the `active-loans` map with specific block-height expiration dates.

### B. Public Configuration & Governance

These functions allow the AI Agent or DAO to tune the protocol's risk appetite.

* `set-paused`: Acts as a manual override to stop new loan issuances during black-swan events.
* `set-model-weights`: I designed this to allow the AI to shift focus. For example, in a bull market, it might weight `history` higher; in a bear market, it can shift weight toward `collateral`.
* `update-market-risk`: Modifies the `market-risk-factor`, which acts as a global safety margin applied to all eligibility assessments.

### C. Public User Functions

* `register-borrower`: Entry point for new users. I have set this to initialize a profile with a neutral "base-score" of 50.
* `add-collateral`: Allows users to increase their "Skin in the Game," which immediately improves their risk score and borrowing limit.
* `repay-loan`: A critical lifecycle function. It verifies the active loan, clears the borrower's debt status, and increments the `repayment-count` to boost future credit.

### D. AI Assessment & Issuance

`assess-and-issue-loan-eligibility` is the primary interface for borrowers. Unlike simple "borrow" buttons, this function:

1. **Analyzes:** Fetches the full profile of the caller.
2. **Calculates:** Runs the weighted scoring model against current weights.
3. **Tiering:** Assigns one of three interest rates (2%, 5%, or 8%) based on the calculated score.
4. **Validates:** Compares the requested amount against the `max-loan` limit (Collateral × Score %).

---

## IV. Technical Scoring Model

I utilize the following LaTeX-defined logic for calculating user eligibility:

The **Adjusted Score** () is derived from the **Raw Score** () and the **Market Risk Factor** ():

The **Maximum Loanable Amount** () is then determined by the relationship between collateral () and the adjusted score:

---

## V. Contribution Guidelines

I am actively seeking developers to assist in:

* Integrating Oracles for real-time STX/USD price feeds.
* Developing a front-end dashboard for borrower "Credit Profiles."
* Creating a "Liquidation Bot" framework for the `liquidate-loan` function.

---

## VI. License

### MIT License

Copyright (c) 2026 CrediSync Protocol

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---
