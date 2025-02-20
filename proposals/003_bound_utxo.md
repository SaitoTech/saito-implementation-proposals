
# Saito Implementation Protocol -- 003 Bound UTXO

## Abstract 

The following document describes proposed changes to the current implementation of the UTXO set to add support for NFTs and coloured coins.

| Field   | Value                   |
| ------- | ----------------------- |
| Author  | David Lancashire        |
| Status  | Published               |
| Type    | Implementation Proposal |
| Created | February 20, 2025       |


## 1. **Introduction to Bound UTXO**

The most common way to add support for tradeable-tokens within networks is to have a state machine (VM) which can execute smart contracts. Those smart contracts can be programmed to manage the distribution and trading of assets. They create arrays that create large in-memory maps which associated the addresses of hodlers to th eamountof contract-managed tokens that they own, and determine under what conditions those tokens can move.

This proposal provides an alternate way to publish and trade NFTs on Saito without the need for a supporting VM.

## 2. Bound UTXO

We introduce a new type of transaction slip (UTXO) that we refer to as a BoundUTXO. This transaction slip has the exact same data-structure as other transactions, to make it entirely compatible with existing code. But it repurposes these variables to contain data that is specific to use intended use managing non-Saito assets on the blockchain.

 * type - BoundUTXO 
 * amount - the amount of tokens/assets
 * address - the unique ID of the NFT/asset 

The id for a BoundUTXO is determined by the first UTXO that is spent in the transaction that creates it. This ID contains the block_id, transaction_id, and slip_id of the UTXO that created it and likely the remainder of the data-field is the address of the originating sender. This ensures uniqueness across the network -- not even the user who created the NFT can create a second one with the same ID as they will need to use a different UTXO to do so. As the NFT moves around, its own unique ID remains the same by consensus rules.

Every BoundUTXO is implicitly paired with a normal UTXO. By standard rules, the UTXO that immediately follows a BoundUTXO is the UTXO that is bound to it. Validation rules speciry that:

1. transactions that move BoundUTXO must also move the UTXO which are immediately after them in the transaction.
2. ATR rebroadcasting moves the BoundUTXO together with the UTXO immediately after it
3. BoundUTXO that are missing their paired UTXO are removed from the chain on rebroadcasting

These changes limit the ability to move the NFT UTXO to transactions which can legitimately move the paired UTXO. This provides a guarantee that only the NFT owner can move the NFT. Updates are required to block validation rules and the wallet to prevent users from accidentally spending the UTXO which are bound to NFTs.



