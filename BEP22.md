
# BEP-22: A Decentralized Single-Asset Fee Token Standard

## 1. Summary

This BEP proposes an interface extension for BEP-21 tokens to create an open market to decide transaction fees for each BEP-21 token where any user can create a pool of BNB and submit a required transfer fee in the asset.

## 2. Abstract

BEP-22's goal is to expand on the BEP-21 token interface by decentralizing the management of the minimum transfer fee to the community.

A pool is defined as a number of BNB locked in a contract tied to the BEP-22 token, along with a dedicated transfer fee.

![Execution of BEP-22](https://i.imgur.com/2RwV0uY.png)

Any externally owned address can set up a pool of BNB and submit a required transfer fee in the asset itself (e.g., "BUSD"). Pools are aggregated into a "Super Pool" that is defined for each BEP-22 asset. Token-holders wishing to transact would be able to obtain the best execution available from the set of existing pools.

![Example of transactions for pools](https://i.imgur.com/nMenfba.png)


## 3. Motivation

Through the creation of a distributed market for setting up fees for BEP-22 tokens, this token standard would allow greater price efficiency and would enable these tokens to fully maintain their decentralized properties without a single point of failure (e.g., BEP-21 contract admin keys being compromised).

Furthermore, in the BEP-21 proposal, the contract owner must actively manage and re-assess actively the required transaction fee (denominated in the token e.g., "BUSD") for transfer. As a result, the owner must fix the required transfer fee for the token above the gas fee in BNB.

The creation of a competitive market for transaction costs in the token (e.g., "BUSD") would offer opportunities for a wider range of participants to earn a positive spread. It would result in lower transaction costs for end users while limiting the risk of a single point of failure due to the existence of a unique contract owner.

While this standard could technically allow third parties to offer lower than normal transfer fees for assets (e.g., _"a 0.005 BUSD token transfer campaign"_), it is **not recommended** since malicious validators could collude to profit from this reverse spread by sending a large number of transactions with such asset and consequently empty the pool from the third-party.

## 4. Status

This BEP is under draft.

## 5. Specification

### 5.1 Token

### 5.2 Implementation

## 6. License

The content is licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
