---
title: Staking Precompile
description: Moonbeam Parachain Staking Ethereum Solidity Precompile Interface Demo
---

# Interacting with the Staking Precompile

![Staking Moonbeam Banner](/images/staking/staking-precompile-banner.png)

## Introduction {: #introduction } 

A delegated proof of stake pallet recently debuted called [Parachain-Staking](https://github.com/PureStake/moonbeam/tree/master/pallets/parachain-staking/src), allowing token holders (nominators) to express exactly which collator candidates they would like to support and with what quantity of stake. The design of the Parachain-Staking pallet is such that it enforces shared risk/reward on chain between delegators and collators.

The Staking module is coded in Rust and it is part of a pallet that is normally not accessible from the Ethereum side of Moonbeam. However, a Staking Precompile allows developers to access the staking features using the Ethereum API in a precompiled contract located at address `{{networks.moonbase.staking.precompile_address}}`. The Staking Precompile was first released in the [Moonbase Alpha v8 release](https://moonbeam.network/announcements/testnet-upgrade-moonbase-alpha-v8/).

--8<-- 'text/moonriver-launch/staking-phase-4.md'

This guide will show you how to interact with the Staking Precompile on Moonbase Alpha.

## The Parachain-Staking Solidity Interface {: #the-parachain-staking-solidity-interface } 

[StakingInterface.sol](https://github.com/PureStake/moonbeam/blob/master/precompiles/parachain-staking/StakingInterface.sol) is an interface through which solidity contracts can interact with Parachain-Staking. The beauty is that solidity developers don’t have to learn the Substrate API. Instead, they can interact with staking functions using the Ethereum interface they are familiar with.

The interface includes the following functions:

 - **is_nominator**(*address* collator) — read-only function that checks whether the specified address is currently a staking nominator
 - **is_candidate**(*address* collator) — read-only function that checks whether the specified address is currently a collator candidate
 - **is_selected_candidate**(*address* collator) - read-only function that checks whether the specified address is currently part of the active collator set
 - **points**(*uint256* round) - read-only function that gets the total points awarded to all collators in a given round
 - **min_nomination**() — read-only function that gets the minimum nomination amount
 - **join_candidates**(*uint256* amount, *uint256* candidateCount) — allows the account to join the set of collator candidates with a specified bond amount and the current candidate count
 - **leave_candidates**(*uint256* amount, *uint256* candidateCount) — immediately removes the account from the candidate pool to prevent others from selecting it as a collator and triggers unbonding after BondDuration rounds have elapsed
 - **go_offline**() — temporarily leave the set of collator candidates without unbonding
 - **go_online**() — rejoin the set of collator candidates after previously calling go_offline()
 - **candidate_bond_more**(*uint256* more) — collator candidate increases bond by specified amount
 - **candidate_bond_less**(*uint256* less) — collator candidate decreases bond by specified amount
 - **nominate**(*address* collator, *uint256* amount, *uint256* collatorNominationCount, *uint256* nominatorNominationCount) — if the caller is not a nominator, this function adds them to the set of nominators. If the caller is already a nominator, then it adjusts their nomination amount
 - **leave_nominators**(*uint256* nominatorNominationCount) — leave the set of nominators and revoke all ongoing nominations
 - **revoke_nominations**(*address* collator) — revoke a specific nomination
 - **nominator_bond_more**(*address* collator, *uint256* more) — nominator increases bond to a collator by specified amount
 - **nominator_bond_less**(*address* collator, *uint256* less) — nominator decreases bond to a collator by specified amount

## Checking Prerequisites {: #checking-prerequisites } 

The below example is demonstrated on Moonbase Alpha, however, it is compatible with all networks including Moonriver and Moonbeam.

 - Have MetaMask installed and [connected to Moonbase Alpha](/getting-started/moonbase/metamask/)
 - Have an account with over `{{networks.moonbase.staking.min_nom_amount}}` tokens. You can get this from [Mission Control](/getting-started/moonbase/faucet/)

!!! note
    The example below requires more than `{{networks.moonbase.staking.min_nom_amount}}` tokens due to the minimum nomination amount plus gas fees. If you need more than the faucet dispenses, please contact us on Discord and we will be happy to help you. 

## Remix Set Up {: #remix-set-up } 

1. Get a copy of [StakingInterface.sol](https://github.com/PureStake/moonbeam/blob/master/precompiles/parachain-staking/StakingInterface.sol)
2. Copy and paste the file contents into a Remix file named StakingInterface.sol

![Copying and Pasting the Staking Interface into Remix](/images/staking/staking-precompile-1.png)

## Compile the Contract {: #compile-the-contract } 

1. Click on the Compile tab, second from top
2. Compile [Staking Interface.sol](https://github.com/PureStake/moonbeam/blob/master/precompiles/parachain-staking/StakingInterface.sol)

![Compiling StakingInteface.sol](/images/staking/staking-precompile-2.png)

## Access the Contract {: #access-the-contract } 

1. Click on the Deploy and Run tab, directly below the Compile tab in Remix. **Note**: we are not deploying a contract here, instead we are accessing a precompiled contract that is already deployed
2. Make sure "Injected Web3" is selected in the Environment drop down
3. Ensure “ParachainStaking - StakingInterface.sol” is selected in the Contract dropdown. Since this is a precompiled contract there is no need to deploy, instead we are going to provide the address of the precompile in the “At Address” Field
4. Provide the address of the Staking precompile: `{{networks.moonbase.staking.precompile_address}}` and click “At Address”

![Provide the address](/images/staking/staking-precompile-3.png)

## Nominate a Collator {: #nominate-a-collator } 

For this example, we are going to be nominating a collator. Nominators are token holders who stake tokens, vouching for specific collators. Any user that holds a minimum amount of {{networks.moonbase.staking.min_nom_amount}} tokens as free balance can become a nominator. 

In order to nominate a collator, you'll need to determine the current collator nomination count and nominator nomination count. The collator nomination count is the numner of nominations backing a specific collator. The nominator nomination account is the number of nominations made by the nominator.

### Get the Collator Nominator Count

--8<-- 'text/staking/collator-nominator-count.md'

### Get your Number of Existing Nominations

--8<-- 'text/staking/nominator-nomination-count.md'

### Call Nominate

1. Expand the panel with the contract address. Locate the nominate function and expand the panel to see the parameters
2. Provide the address of a collator such as `{{ networks.moonbase.staking.collators.address1 }}`
3. Provide the amount to nominate in WEI. There is a minimum of `{{networks.moonbase.staking.min_nom_amount}}` tokens to nominate, so the lowest amount in WEI is `5000000000000000000`
4. Provide the nomination count for the collator provided in Step 2
5. Provide your nomination count
6. Press "transact" and confirm the transaction in Metamask

![Nominate a Collator](/images/staking/staking-precompile-4.png)

## Verify Nomination {: #verify-nomination } 

To verify your nomination was successful, you can check the chain state in Polkadot.js Apps. First, add your metamask address to the [address book in Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.testnet.moonbeam.network#/addresses). If you've already completed this step you can skip ahead to the next section. 

### Add Metamask Address to Address Book {: #add-metamask-address-to-address-book } 

1. Navigate to Accounts -> Address Book 
2. Click on "Add contact"
3. Add your Metamask Address
4. Provide a nickname for the account

![Add to Address Book](/images/staking/staking-precompile-5.png)

### Verify Nominator State {: #verify-nominator-state } 
1. To verify your nomination was successful, head to [Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.testnet.moonbeam.network#/chainstate) and navigate to Developer -> Chain State
2. Select the "parachainStaking" pallet
3. Select the "nominatorState" query
4. Click the "Plus" Button to return the results and verify your nomination

![Verify Nomination](/images/staking/staking-precompile-6.png)

## Revoking a Nomination {: #revoking-a-nomination } 

To revoke a nomination and receive your tokens back:

1. Call the `revoke_nomination` method, providing the same address you started the nomination with above
2. Click "transact" and confirm the transaction in MetaMask

![Revoke Nomination](/images/staking/staking-precompile-7.png)

You can check your nominator state again on Polkadot.js Apps to confirm.

