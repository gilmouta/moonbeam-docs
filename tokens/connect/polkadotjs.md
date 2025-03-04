---
title: Using Polkadot.js Apps
description: Follow this quick tutorial to learn how to use Moonbeam’s Ethereum-standard H160 addresses with Substrate-based apps like Polkadot.js.
---

# Interacting with Moonbeam Using Polkadot.js Apps

![Intro diagram](/images/tokens/connect/polkadotjs/polkadotjs-banner.png)

## Introduction {: #introduction } 

With the [release of the v3 upgrade](https://www.purestake.com/news/moonbeam-network-upgrades-account-structure-to-match-ethereum/) for the Moonbase Alpha TestNet, we have made significant updates to the underlying account system on Moonbeam, replacing the default Substrate-style accounts and keys with Ethereum-style accounts and keys.

The Polkadot.js Apps interface was updated as well so that it natively supports H160 addresses and ECDSA keys. So, in this tutorial lets check this new integration of Ethereum-based accounts on the Polkadot.js Apps site.

## Connecting to Moonbase Alpha {: #connecting-to-moonbase-alpha } 

First, we need to connect it to the Moonbase Alpha TestNet by clicking the top left corner logo and selecting Moonbase Alpha (under Test Networks).

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-1.png)

After switching, the Polkadot.js site will not only connect to Moonbase Alpha, but also change its styling to make a perfect match.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-2.png)

## Creating or Importing an H160 Account {: #creating-or-importing-an-h160-account } 

Let's see how we can create a new account, or import an already existing MetaMask account to Polkadot.js Apps. First, navigate to the accounts section, and click in the add account button.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-3.png)

This will open a wizard pop-up that will guide you through the process of adding an account to the Polkadot.js Apps interface. Make sure you click on the drop-down menu and change from Mnemonic to Raw seed, this allows you to add an account through a private key.

!!! note
    Currently, you can only create or import accounts in Polkadot.js via a private key. Doing so with the mnemonic will result in a different public address if you later try to import this account to an Ethereum wallet such as MetaMask. This is because Polkadot.js uses BIP39, whereas Ethereum uses BIP32 or BIP44.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-4.png)

Next, if you want to create a new account make sure you store the private key displayed by the wizard. If you want to import an existing account, enter your private key that you can export from MetaMask, in this case we are importing the following account:

- Private key: `28194e8ddb4a2f2b110ee69eaba1ee1f35e88da2222b5a7d6e3afa14cf7a3347`
- Public address: `0x44236223aB4291b93EEd10E4B511B37a398DEE55` 

!!! note
    Never reveal your private keys as they give direct access to your funds. The steps in this guide are for demostration purposes only. 
    
Make sure to include the prefix in the private key, i.e., `0x`. If you entered the information correctly, the corresponding public address should appear in the upper left corner of the window.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-5.png)

Click next and finish the wizard by setting an account name and password. After a confirmation message, you should see in the main Accounts tab the address with the corresponding balance: in our case, Bob's address. Moreover, we can overlay the MetaMask extension to see that both balances are the same.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-6.png)

## Sending a Transaction Through Substrate's API {: #sending-a-transaction-through-substrates-api } 

Now, let's demonstrate the potential of Moonbeam's Unified Accounts scheme by making a transfer through the Substrate API using the Polkadot.js Apps. Remember that we are interacting with Substrate using an Ethereum-style H160 address. To do so, we've imported another account named Charley with 5 `DEV` tokens.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-7.png)

Next, click on Bob's send button, which opens another wizard that guides you through the process of sending a transaction. Set the send to address and the amount, which for our example is 5 DEV tokens. When ready, click on the "Make Transfer" button.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-8.png)

After the transaction is signed using the password, Polkadot.js will display some messages on the top right corner while it's being processed. Once confirmed, you should see the balances updated for each account.

![Connect to Moonbase Alpha](/images/tokens/connect/polkadotjs/polkadotjs-app-9.png)

And that is it! We are excited about being able to support H160 accounts in Polkadot.js Apps, as we believe this will greatly enhance the user experience in the Moonbeam Network and its Ethereum compatibility features.
