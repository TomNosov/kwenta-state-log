---
kip: 62
title: Escrow V2 (Escrow Transferrability)
status: Approved
created: 2023-01-28
section: 3
---

## Summary

Make escrow transferrability possible in order to enable transferring staked and escrowed KWENTA balances to a different account.

## Abstract
To improve the UX, this KIP proposes that escrowed KWENTA are to be made transferable. However, this KIP additionally proposes that the KWENTA transferred this way should also be escrowed on the new receiving wallet. This KIP intends to make account merging for any escrowed KWENTA (excluding V1 escrow locked KWENTA) balance possible at any time.

## Motivation
KWENTA inflationary rewards and trading rewards are locked for a period of 1-year and currently cannot be transferred without paying an early vest fee. One of the consequences of this is that a staker who has earned rewards in a wallet is forced to continue to maintain this wallet. The purpose of locking these staking rewards is to ensure that they were not able to be transferred, however, this creates a problem if a wallet is compromised or if a user would like to cycle wallets. This proposal aims to find a compromise that allows stakers to transfer their entire vested balance to a different wallet where it will continue to be vested.

## Specification
A new escrow (`RewardEscrowV2.sol`) contract will be deployed that supports additional escrow functionality. It's important to note existing KWENTA in the current escrow contract (V1) will not have access to these features.

For this new escrow to be stake-able, either the old escrowed KWENTA (from V1) loses staking functionality or the staking contract is changed to support multiple escrow contracts (pending feasibility research). This upgrade should be done at the same time as [KIP-58: Staking V2] to ensure compatibility of both new systems.

A function called `transferVestingEntry()` should be created with the parameters of `entryID` and `recipient`. The access control should be restricted to the owner of the vesting entry. Calling this function will move the ownership of a vesting entry from one account to another. If necessary, staked escrowed KWENTA should be unstaked and restaked under the new owner to ensure staked escrowed KWENTA is appropriately accounted for*. To prevent a fragmenting of vesting schedules, only a full transfer of the entry is allowed (no partial transfers). 

Additionally, `stakeEscrowOnBehalf()` and `unstakeEscrowOnBehalf()` will be added as part of [KIP-42](./kip-42.md) to support 3rd party autocompounding of staking rewards.

***Invariant***: Staked escrowed KWENTA of account N should never be greater than total KWENTA in escrow for account N

Copyright and related rights waived via CC0.