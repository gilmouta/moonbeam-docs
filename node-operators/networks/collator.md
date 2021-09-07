---
title: Collators
description: Instructions on how to become a collator in the Moonbeam Network once you are running a node
---

# Run a Collator on Moonbeam

![Collator Moonbeam Banner](/images/fullnode/collator-banner.png)

## Introduction {: #introduction } 

Collators are members of the network that maintain the parachains they take part in. They run a full node (for both their particular parachain and the relay chain), and they produce the state transition proof for relay chain validators.

Users can spin up full nodes on Moonbase Alpha and Moonriver and activate the `collate` feature to participate in the ecosystem as collators.

Moonbeam uses the [Nimbus Parachain Consensus Framework](/learn/features/consensus/). This provides a two-step filter to allocate collators to a block production slot:

 - The parachain staking filter selects the top {{ networks.moonbase.staking.max_collators }} collators on Moonbase Alpha and the top {{ networks.moonriver.staking.max_collators }} collators on Moonriver in terms of tokens staked in each network. This filtered pool is called selected candidates, and selected candidates are rotated every round
 - The fixed size subset filter picks a pseudo-random subset of the previously selected candidates for each block production slot

This guide will take you through the following steps:

 - **[Technical requirements](#technical-requirements)** — shows you the criteria you must meet from a technical perspective
 - **[Accounts and staking requirements](#accounts-and-staking-requirements)** — goes through the process of getting your account set up and bond tokens to become a collator candidate
 - **[Generate session keys](#generate-session-keys)** — explains how to generate session keys, used to map your author ID with your H160 account
 - **[Map author ID to your account](#map-author-id-to-your-account)** — outlines the steps to map your public session key to your H160 account, where block rewards will be paid to

## Technical Requirements {: #technical-requirements } 

From a technical perspective, collators must meet the following requirements:

 - Have a full node running with the collation options. To do so, follow the [spin up a full node tutorial](/node-operators/networks/full-node/), considering the specific code snippets for collators
 - Enable the telemetry server for your full node. To do so, follow the [telemetry tutorial](/node-operators/networks/telemetry/)

## Accounts and Staking Requirements {: #accounts-and-staking-requirements } 

Similar to Polkadot validators, you need to create an account. For Moonbeam, this is an H160 account or basically an Ethereum style account from which you hold the private keys. In addition, you will need a minimum amount of tokens staked to be considered eligible (become a candidate). Only a certain amount of the top collators by nominated stake will be in the active set.

=== "Moonbase Alpha"
    |    Variable     |                          Value                          |
    |:---------------:|:-------------------------------------------------------:|
    |   Bond Amount   | {{ networks.moonbase.staking.collator_bond_min }} DEV   |
    | Active set size | {{ networks.moonbase.staking.max_collators }} collators |

=== "Moonriver"
    |    Variable     |                          Value                           |
    |:---------------:|:--------------------------------------------------------:|
    |   Bond Amount   | {{ networks.moonriver.staking.collator_bond_min }} MOVR  |
    | Active set size | {{ networks.moonriver.staking.max_collators }} collators |

### Account in PolkadotJS {: #account-in-polkadotjs } 

A collator has an account associated with its collation activities. This account mapped to an author ID to identify him as a block producer and send the payouts from block rewards. 

Currently, you have two ways of proceeding in regards having an account in [PolkadotJS](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.testnet.moonbeam.network#/accounts):

 - Importing an existing (or create a new) H160 account from external wallets or services such as [MetaMask](/tokens/connect/metamask/) and [MathWallet](/tokens/connect/mathwallet/)
 - Create a new H160 account with [PolkadotJS](/tokens/connect/polkadotjs/)

Once you have an H160 account imported to PolkadotJS, you should see it under the "Accounts" tab. Make sure you have your public address at hand (`PUBLIC_KEY`), as it is needed to configure your [deploy your full node](/node-operators/networks/full-node/) with the collation options.

![Account in PolkadotJS](/images/fullnode/collator-polkadotjs1.png)

## Become a Collator Candidate {: #become-a-collator-candidate } 

Before getting started, it's important to note some of the timings of different actions related to collation activities:

=== "Moonbase Alpha"
    |               Variable                |       Value        |
    |:-------------------------------------:|:------------------:|
    |    Join/leave collator candidates     | {{ networks.moonbase.collator_timings.join_leave_candidates.rounds }} rounds ({{ networks.moonbase.collator_timings.join_leave_candidates.hours }} hours) |
    |        Add/remove nominations         | {{ networks.moonbase.collator_timings.add_remove_nominations.rounds }} rounds ({{ networks.moonbase.collator_timings.add_remove_nominations.hours }} hours) |
    | Rewards payouts (after current round) | {{ networks.moonbase.collator_timings.rewards_payouts.rounds }} rounds ({{ networks.moonbase.collator_timings.rewards_payouts.hours }} hours) |

=== "Moonriver"
    |               Variable                |       Value        |
    |:-------------------------------------:|:------------------:|
    |    Join/leave collator candidates     | {{ networks.moonriver.collator_timings.join_leave_candidates.rounds }} rounds ({{ networks.moonriver.collator_timings.join_leave_candidates.hours }} hours) |
    |        Add/remove nominations         | {{ networks.moonriver.collator_timings.add_remove_nominations.rounds }} rounds ({{ networks.moonriver.collator_timings.add_remove_nominations.hours }} hours) |
    | Rewards payouts (after current round) | {{ networks.moonriver.collator_timings.rewards_payouts.rounds }} rounds ({{ networks.moonriver.collator_timings.rewards_payouts.hours }} hours) |


!!! note 
    The values presented in the previous table are subject to change in future releases.
### Get the Size of the Candidate Pool {: #get-the-size-of-the-candidate-pool } 

First, you need to get the `candidatePool` size (this can change thru governance) as you'll need to submit this parameter in a later transaction. To do so, you'll have to run the following JavaScript code snippet from within [PolkadotJS](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.testnet.moonbeam.network#/js):

```js
// Simple script to get candidate pool size
const candidatePool = await api.query.parachainStaking.candidatePool();
console.log(`Candidate pool size is: ${candidatePool.length}`);
```

 1. Head to the "Developer" tab 
 2. Click on "JavaScript"
 3. Copy the code from the previous snippet and paste it inside the code editor box 
 4. (Optional) Click the save icon and set a name for the code snippet, for example, "Get candidatePool size". This will save the code snippet locally
 5. Click on the run button. This will execute the code from the editor box
 6. Copy the result, as you'll need it when joining the candidate pool

![Get Number of Candidates](/images/fullnode/collator-polkadotjs2.png)

### Join the Candidate Pool {: #join-the-candidate-pool } 

Once your node is running and in sync with the network, you become a collator candidate (and join the candidate pool). Depending on which network you are connected to, head to PolkadotJS for [Moonbase Alpha](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.testnet.moonbeam.network#/accounts) or [Moonriver](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.moonriver.moonbeam.network#/accounts) and take the following steps:

 1. Navigate to the "Developers" tab and click on "Extrinsics"
 2. Select the account you want to be associated with your collation activities
 3. Confirm your collator account is funded with at least the [minimum stake required](#accounts-and-staking-requirements) plus some extra for transaction fees 
 4. Select `parachainStaking` pallet under the "submit the following extrinsics" menu
 5. Open the drop-down menu, which lists all the possible extrinsics related to staking, and select the `joinCandidates()` function
 6. Set the bond to at least the [minimum amount](#accounts-and-staking-requirements) to be considered a collator candidate. Only collator bond counts for this check. Additional nominations do not count
 7. Set the candidate count as the candidate pool size. To learn how to retrieve this value, check [this section](#get-the-size-of-the-candidate-pool)
 8. Submit the transaction. Follow the wizard and sign the transaction using the password you set for the account

![Join Collators pool PolkadotJS](/images/fullnode/collator-polkadotjs3.png)

!!! note
    Function names and the minimum bond requirement are subject to change in future releases.

As mentioned before, only the top {{ networks.moonbase.staking.max_collators }} collators on Moonbase Alpha and the top {{ networks.moonriver.staking.max_collators }} collators on Moonriver by nominated stake will be in the active set. 

### Stop Collating {: #stop-collating } 

Similar to Polkadot's `chill()` function, to leave the collator's candidate pool, follow the same steps as before but select the `leaveCandidates()` function in step 5.

## Session Keys {: #session-keys } 

With the release of [Moonbase Alpha v8](/networks/testnet/), collators will sign blocks using an author ID, which is basically a [session key](https://wiki.polkadot.network/docs/learn-keys#session-keys). To match the Substrate standard, Moonbeam collator's session keys are [SR25519](https://wiki.polkadot.network/docs/learn-keys#what-is-sr25519-and-where-did-it-come-from). This guide will show you how you can create/rotate your session keys associated to your collator node.

First, make sure you're [running a collator node](/node-operators/networks/full-node/) and you have exposed the RPC ports. Once you have your collator node running, your terminal should print similar logs:

![Collator Terminal Logs](/images/fullnode/collator-terminal1.png)

Next, session keys can be rotated by sending an RPC call to the HTTP endpoint with the `author_rotateKeys` method. For reference, if your collator's HTTP endpoint is at port `9933`, the JSON-RPC call might look like this:

```
curl http://127.0.0.1:9933 -H \
"Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"author_rotateKeys",
    "params": []
  }'
```

The collator node should respond with the corresponding public key of the new author ID (session key).

![Collator Terminal Logs RPC Rotate Keys](/images/fullnode/collator-terminal2.png)

Make sure you write down this public key of the author ID. Next, this will be mapped to an H160 Ethereum-styled address to which the block rewards are paid.

## Map Author ID to your Account {: #map-author-id-to-your-account } 

Once you've generated your author ID (session keys), the next step is to map it to your H160 account (an Ethereum-styled address). Make sure you hold the private keys to this account, as this is where the block rewards are paid out to.

There is a bond that is sent when mapping your author ID with your account. This bond is per author ID registered. The bond set is as follows:

 - Moonbase Alpha - {{ networks.moonbase.staking.collator_map_bond }} DEV tokens 
 - Moonriver - {{ networks.moonriver.staking.collator_map_bond }} MOVR tokens. 

The `authorMapping` module has the following extrinsics programmed:

 - **addAssociation**(*address* authorID) — maps your author ID to the H160 account from which the transaction is being sent, ensuring is the true owner of its private keys. It requires a [bond](#accounts-and-staking-requirements)
 - **clearAssociation**(*address* authorID) — clears the association of an author ID to the H160 account from which the transaction is being sent, which needs to be the owner of that author ID. Also refunds the bond
 - **updateAssociation**(*address* oldAuthorID, *address* newAuthorID) —  updates the mapping from an old author ID to a new one. Useful after a key rotation or migration. It executes both the `add` and `clear` association extrinsics atomically, enabling key rotation without needing a second bond

The module also adds the following RPC calls (chain state):

- **mapping**(*address* optionalAuthorID) — displays all mappings stored on-chain, or only that related to the input if provided

### Mapping Extrinsic {: #mapping-extrinsic } 

To map your author ID to your account, you need to be inside the [candidate pool](#become-a-collator-candidate). Once you are a collator candidate, you need to send a mapping extrinsic (transaction). Note that this will bond tokens per author ID registered. To do so, take the following steps:

 1. Head to the "Developer" tab
 2. Select the "Extrinsics" option
 3. Choose the account that you want to map your author ID to be associated with, from which you'll sign this transaction
 4. Select the `authorMapping` extrinsic
 5. Set the method to `addAssociation()`
 6. Enter the author ID. In this case, it was obtained via the RPC call `author_rotateKeys` in the previous section
 7. Click on "Submit Transaction"

![Author ID Mapping to Account Extrinsic](/images/fullnode/collator-polkadotjs4.png)

If the transaction is successful, you will see a confirmation notification on your screen. If not, make sure you've joined the [candidate pool](#become-a-collator-candidate).

![Author ID Mapping to Account Extrinsic Successful](/images/fullnode/collator-polkadotjs5.png)

### Checking the Mappings {: #checking-the-mappings } 

You can also check the current on-chain mappings by verifying the chain state. To do so, take the following steps:

 1. Head to the "Developer" tab
 2. Select the "Chain state" option
 3. Choose `authorMapping` as the state to query
 4. Select the `mappingWithDeposit` method
 5. Provide an author ID to query. Optinally, you can disable the slider to retrieve all mappings 
 6. Click on the "+" button to send the RPC call

![Author ID Mapping Chain State](/images/fullnode/collator-polkadotjs6.png)

You should be able to see the H160 account associated with the author ID provided. If no author ID was included, this would return all the mappings stored on-chain.
