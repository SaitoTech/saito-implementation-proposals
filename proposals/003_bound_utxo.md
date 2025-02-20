
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

The most common way to add support for tradeable-tokens within networks is to have a state machine (VM) which can execute smart contracts. Those smart contracts can be programmed to manage the distribution and trading of assets. This proposal provides a way to publish and trade NFTs on Saito without the need for a supporting VM.

## 2. Bound UTXO

We introduce a new type of transaction slip (UTXO) that we refer to as a BoundUTXO. This transaction slip repurposes the core variables:

 * type - BoundUTXO 
 * amount - the amount of tokens/assets
 * address - the unique ID of the NFT/asset 

Every BoundUTXO is implicitly paired with a normal UTXO in the block. By standard rules, the UTXO that immediately follows a BoundUTXO is the UTXO that is bound to it. Validation rules speciry that:

1. transactions that move BoundUTXO must also move the UTXO which are immediately after them in the transaction.
2. ATR rebroadcasting moves the BoundUTXO together with the UTXO immediately after it
3. BoundUTXO that are missing their paired UTXO are removed from the chain on rebroadcasting

These changes limit the ability to move the NFT UTXO to transactions which can legitimately move the paired UTXO. This provides a guarantee that only the NFT owner can move the NFT. 

The id for a BoundUTXO is determined by the first UTXO that is spent in the transaction that creates it. This ID contains the block_id, transaction_id, and slip_id of the UTXO that created it, ensuring uniqueness across the network. This information remains encoded in the NFT in perpetuity. As the NFT moves around, its own unique key in the hashmap changes along with the normal UTXO that accompanies it on its journey.


