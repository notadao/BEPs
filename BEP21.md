# BEP-21: A Single-Asset Fee Token Standard

## 1. Summary

This BEP proposes an interface extension for [BEP-20 tokens](https://github.com/binance-chain/BEPs/blob/master/BEP20.md) to allow payment of transfer fees in the asset itself (e.g., "BUSD") instead of relying on the native chain asset ("BNB") through the use of a contract owner system acting as a delegator party to cover fees in the native asset.

## 2. Abstract

BEP-21's goal is to provide a token interface that does not require transfer fees to be paid in the native asset of Binance Smart Chain ("BNB") but instead in the token itself (e.g., "BUSD").

BEP-21 is heavily influenced by the existing work from TomoChain development teams (["TRC-21" and "TomoZ"](https://docs.tomochain.com/developer-guide/standards-and-specification/trc21-specification)).

BEP-21 relies on a contract owner who is responsible for paying transfer fees and maintaining a sufficient balance of the native asset ("BNB") while collecting fees in the asset (e.g., "BUSD").

In addition, the contract owner is responsible for setting up the appropriate transaction fee (in the asset itself) for token transfers.

![description of BEP-21 token](https://i.imgur.com/INy6KiC.png)


## 3. Motivation

BEP-21 would allow greater accessibility for assets like stablecoins for a non-crypto set of users. Ths standard also allows users not to get price exposure to BNB, without impacting the core economics of the network since gas fees are ultimately paid in BNB by the contract owner.

## 4. Status

This BEP is under draft.

## 5. Specification

### 5.1 Token methods

#### 5.1.1 Core BEP-20 methods

Similar to other BEP-20 tokens, BEP-21 tokens incorporate the following set of methods.

##### 5.1.1.1 name
```
function name() public view returns (string)
```
- Returns the name of the token - e.g., "MyBEP21Token".
- **OPTIONAL** - This method can be used to improve usability, but interfaces and other contracts MUST NOT expect these values to be present.

##### 5.1.1.2 symbol
```
function symbol() public view returns (string)
```
- Returns the symbol of the token. E.g., “COOL”.
- This method can be used to improve usability.
- While this method is optional in EIP-20, it is a **required method for BEP-20 token interface**. Tokens that do not implement this method **will not be able to move** across the Binance Chain and Binance Smart Chain.

##### 5.1.1.3 decimals
```
function decimals() public view returns (uint8)
```
- Returns the number of decimals the token uses - e.g., 8, means to divide the token amount by 100000000 to get its user representation.
- This method can be used to improve usability.
- While this method is optional in EIP-20, it is a **required method for BEP-20 token interface**. Tokens that do not implement this method **will not be able to move** across the Binance Chain and Binance Smart Chain.

##### 5.1.1.4 totalSupply
```
function totalSupply() public view returns (uint256)
```
- Returns the total token supply.
- If the token is bound to flow across the Binance Chain and Binance Smart Chain, the number should be the total of circulation across the two blockchains.

##### 5.1.1.5 balanceOf
```
function balanceOf(address _owner) public view returns (uint256 balance)
```
- Returns the account balance of another account with address `_owner`.

##### 5.1.1.6 getOwner
```
function getOwner() external view returns (address);
```
- Returns the BEP-20 token owner which is necessary for binding with BEP-2 token.
- While this method is optional in EIP-20, it is a **required method for BEP-20 token interface**. Tokens that do not implement this method will **not be able to move** across the Binance Chain and Binance Smart Chain.

##### 5.1.1.7 transfer
```
function transfer(address _to, uint256 _value) public returns (bool success)
```
- Transfers `_value` amount of tokens to address `_to`, and MUST fire the `Transfer` event. The function SHOULD throw if the message caller’s account balance does not have enough tokens to spend.
- **NOTE** - Transfers of 0 values MUST be treated as normal transfers and fire the Transfer event.

##### 5.1.1.8 transferFrom
```
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
```
- Transfers `_value` amount of tokens from address `_from` to address `_to`, and MUST fire the `Transfer` event.
- The `transferFrom` method is used for a withdraw workflow, allowing contracts to transfer tokens on your behalf. This can be used for example to allow a contract to transfer tokens on your behalf and/or to charge fees in sub-currencies. The function SHOULD throw unless the `_from` account has deliberately authorized the sender of the message via some mechanism.
- **NOTE** - Transfers of 0 values MUST be treated as normal transfers and fire the `Transfer` event.

##### 5.1.1.9 approve
```
function approve(address _spender, uint256 _value) public returns (bool success)
```
- Allows `_spender` to withdraw from your account multiple times, up to the `_value` amount. If this function is called again it overwrites the current allowance with `_value`.
- **NOTE** - To prevent attack vectors like the one described here and discussed here, clients SHOULD make sure to create user interfaces in such a way that they set the allowance first to 0 before setting it to another value for the same spender. **However, the contract itself should not enforce it**, to allow backward compatibility with contracts deployed before.

##### 5.1.1.10 allowance
```
function allowance(address _owner, address _spender) public view returns (uint256 remaining)
```
- Returns the amount which `_spender` is still allowed to withdraw from `_owner`.

#### 5.1.2  New and modified methods (IN PROGRESS)

##### 5.1.2.1 setTransactionFee

 ```
 function setTransactionFee(address _owner) public returns (bool)
  ```

 - Return whether the new transaction fee has been set. It can only be called by the contract owner.



##### 5.1.2.2 viewTransactionFee

 ```
 function viewTransactionFee() external view returns (uint256)
  ```

- Returns the current transfer fee denominated in the asset itself (e.g., BUSD)


##### 5.1.2.3 viewOwner

```
function viewOwner() external view returns (address);
```

- Returns the current contract owner address.


### 5.2 Events

#### 5.2.1 BEP-20 events

##### 5.1.2.1 Transfer
```
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```
- **MUST** trigger when tokens are transferred, including zero value transfers.
- A token contract which creates new tokens SHOULD trigger a Transfer event with the `_from` address set to 0x0 when tokens are created.

##### 5.1.2.2 Approval
```
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```
- **MUST** trigger on any successful call to `approve(address _spender, uint256 _value)`.

#### 5.2.2 New events

##### 5.2.2.1 Fee

 ```
 event Fee(address indexed _from, address indexed _to, address indexed _owner, uint256 value);
  ```
  - **MUST** trigger on any successful fee payment to notify users of the transfer and its associated cost.


## 6. License
The content is licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## 7. References

- [Griffith, A.T. (2018). Ethereum Meta Transactions](https://medium.com/@austin_48503/ethereum-meta-transactions-90ccf0859e84)
- [TomoChain. TRC21 Specification. Accessed at September 18th 2020](https://docs.tomochain.com/developer-guide/standards-and-specification/trc21-specification)
- [TomoChain R&D Team (2019). TomoZ: TomoChain’s Protocol Proposal for Paying Transaction Fees by Tokens](https://docs.google.com/document/d/1jxD3DsU7GWhxQhs0R8hCmqIQvfQfJjAQaioBoRrVGIA/edit?ts=5cf09ed4)
