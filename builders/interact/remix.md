---
title: Using Remix
description: Learn how to use one of the most popular Ethereum developer tools, the Remix IDE, to interact with a local Moonbeam node.
---

# Using Remix to Deploy to Moonbeam

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/NBOLCGT5-ww' frameborder='0' allowfullscreen></iframe></div>
<style>.caption { font-family: Open Sans, sans-serif; font-size: 0.9em; color: rgba(170, 170, 170, 1); font-style: italic; letter-spacing: 0px; position: relative;}</style>

## Introduction {: #introduction } 

Remix is one of the commonly used development environments for smart contracts on Ethereum. Given Moonbeam’s Ethereum compatibility features, Remix can be used directly with a Moonbeam development node or the Moonbase Alpha TestNet.

This guide walks through the process of creating and deploying a Solidity-based smart contract to a Moonbeam development node using the [Remix IDE](https://remix.ethereum.org/). 

!!! note
    This tutorial was created using the {{ networks.development.build_tag}} tag which is based on the {{ networks.moonbase.parachain_release_tag }} release of [Moonbase Alpha](https://github.com/PureStake/moonbeam/releases/tag/{{ networks.moonbase.parachain_release_tag }}). The Moonbeam platform and the [Frontier](https://github.com/paritytech/frontier) components it relies on for Substrate-based Ethereum compatibility are still under very active development.

## Checking Prerequisites {: #checking-prerequisites } 

This guide assumes that you have a local Moonbeam node running in `--dev` mode and that you have a [MetaMask](https://metamask.io/) installation configured to use this local node. You can find instructions for running a local Moonbeam node [here](/builders/get-started/moonbeam-dev/) and instructions to connect MetaMask to it [here](/tokens/connect/metamask/).

If you followed the guides above, you should have a local Moonbeam node which will begin to author blocks as transactions arrive:

![Local Moonbeam node producing blocks](/images/builders/interact/remix/using-remix-1.png)

And you should have a MetaMask installation connected to your local Moonbeam dev node with at least one account that has a balance. It should look something like this (expanded view):

![MetaMask installation with a balance](/images/builders/interact/remix/using-remix-2.png)

!!! note
    Make sure you are connected to your Moonbeam node and not another network!

## Getting Started with Remix {: #getting-started-with-remix } 

Now, let’s fire up Remix to exercise more advanced functionalities in Moonbeam.

Launch Remix by navigating to [https://remix.ethereum.org/](https://remix.ethereum.org/). In the main screen, under Environments, select Solidity to configure Remix for Solidity development, then navigate to the File Explorers view:

![File explorer](/images/builders/interact/remix/using-remix-3.png)

We will create a new file to save the Solidity smart contract. Hit the + button under File Explorers and enter the name "MyToken.sol" in the popup dialog:

![Create a new file for your Solidity contract](/images/builders/interact/remix/using-remix-4.png)

Next, let's paste the following smart contract into the editor tab that comes up:

```solidity
--8<-- 'code/remix-local/contract.md'
```

This is a simple ERC-20 contract based on the current Open Zeppelin ERC-20 template. It creates MyToken with symbol MYTOK and mints the entirety of the initial supply to the creator of the contract.

Once you have pasted the contract into the editor, it should look like this:

![Paste the contract into the editor](/images/builders/interact/remix/using-remix-5.png)

Now, navigate to the compile sidebar option to press the “Compile MyToken.sol” button:

![Compile MyToken.sol](/images/builders/interact/remix/using-remix-6.png)

You will see Remix download all of the Open Zeppelin dependencies and compile the contract.

## Deploying a Contract to Moonbeam Using Remix {: #deploying-a-contract-to-moonbeam-using-remix } 

Now we can deploy the contract by navigating to the Deployment sidebar option. You need to change the topmost “Environment” dropdown from “JavaScript VM” to “Injected Web3.” This tells Remix to use the MetaMask injected provider, which will point it to your Moonbeam development node. If you wanted to try this using the Moonbase Alpha TestNet, you would have to connect MetaMask to the TestNet instead of your local development node.

As soon as you select "Injected Web3", you will be prompted to allow Remix to connect to your MetaMask account.

![Replace](/images/builders/interact/remix/using-remix-7.png)

Press “Next” in Metamask to allow Remix to access the selected account.

Back on Remix, you should see that the account you wish to use for deployment is now managed by MetaMask. Next to the Deploy button, let’s specify an initial supply of 8M tokens. Since this contract uses the default of 18 decimals, the value to put in the box is `8000000000000000000000000`.

Once you have entered this value, select "Deploy."

![Enter an account balance and deploy](/images/builders/interact/remix/using-remix-8.png)

You will be prompted in MetaMask to confirm the contract deployment transaction.

![Confirm the transaction message](/images/builders/interact/remix/using-remix-9.png)

!!! note
    If you have problems deploying any specific contract, you can try manually increasing the gas limit in MetaMask. Select the colored circle in the top right corner and select **Settings** from the menu. Then click on **Advanced** and toggle the **Advanced Gas Controls** setting to **ON**.

After you press confirm and the deployment is complete, you will see the transaction listed in MetaMask. The contract will appear under Deployed Contracts in Remix.

![Confirmed label on a transaction](/images/builders/interact/remix/using-remix-10.png)

Once the contract is deployed, you can interact with it from within Remix.

Drill down on the contract under “Deployed Contracts.” Clicking on name, symbol, and totalSupply should return “MyToken,” “MYTOK,” and “8000000000000000000000000” respectively. If you copy the address from which you deployed the contract and paste it into the balanceOf field, you should see the entirety of the balance of the ERC-20 as belonging to that user. Copy the contract address by clicking the button next to the contract name and address.

![Interact with the contract from Remix](/images/builders/interact/remix/using-remix-11.png)

## Interacting with a Moonbeam-based ERC-20 from MetaMask {: #interacting-with-a-moonbeam-based-erc-20-from-metamask } 

Now, open MetaMask to add the newly deployed ERC-20 tokens. Before doing so, make sure you have copied the contract's address from Remix. Back in MetaMask, click on “Add Token” as shown below. Make sure you are in the account that deployed the token contract.

![Add a token](/images/builders/interact/remix/using-remix-12.png)

Paste the copied contract address into the “Custom Token” field. The “Token Symbol” and “Decimals of Precision” fields should be automatically populated.

![Paste the copied contract address](/images/builders/interact/remix/using-remix-13.png)

After hitting “Next,” you will need to confirm that you want to add these tokens to your MetaMask account. Hit “Add Token” and you should see a balance of 8M MyTokens in MetaMask:

![Add the tokens to your MetaMask account](/images/builders/interact/remix/using-remix-14.png)

Now we can send some of these ERC-20 tokens to the other account that we have set up in MetaMask. Hit “send” to initiate the transfer of 500 MyTokens and select the destination account.

After hitting “next,” you will be asked to confirm (similar to what is pictured below).

![Confirmation of the token transfer](/images/builders/interact/remix/using-remix-15.png)

Hit “Confirm” and, after the transaction is complete, you will see a confirmation and a reduction of the MyToken account balance from the sender account in MetaMask:

![Verify the reduction in account balance](/images/builders/interact/remix/using-remix-16.png)

If you own the account that you sent the tokens to, you can add the token asset to verify that the transfer arrived.

## Using the Moonbeam Remix Plugin {: #using-the-moonbeam-remix-plugin }

The Moonbeam Team has built a Remix plugin that makes it even easier to develop and deploy your Ethereum smart contracts on Moonbeam.  The Moonbeam Remix Plugin combines all of the important functions needed to compile, deploy, and interact with your smart contracts from one place - no switching tabs needed. 

### Installing the Moonbeam Remix Plugin

To install the Moonbeam Remix Plugin, take the following steps:

 1. Head to the Plugin manager tab
 2. Search for "Moonbeam"
 3. Press "Activate" and the Moonbeam Remix Plugin will be added directly above the Plugin manager tab

![Activating the Moonbeam Remix Plugin](/images/builders/interact/remix/using-remix-17.png)

Once you've added the plugin, a Moonbeam logo will appear on the left hand side, representing the Moonbeam Remix Plugin tab.

### Getting Started with the Moonbeam Remix Plugin

Click on the Moonbeam Logo in your Remix IDE to open the Moonbeam Plugin. This part assumes you already have a contract in Remix ready to be compiled. You can generate an [ERC-20 contract here](https://wizard.openzeppelin.com/). Follow along to deploy an ERC-20 Token to Moonbase Alpha using the Moonbeam Remix Plugin.

 1. Press "Connect" to Connect your Metamask to the Remix IDE
 2. Ensure you're on the correct network. In this example, we're on Moonbase Alpha
 3. Press Compile or choose Auto-Compile if you prefer
 4. Press Deploy and Confirm the Transaction in Metamask

![Compiling and Deploying a Contract with the Moonbeam Remix Plug](/images/builders/interact/remix/using-remix-18.png)

It's that easy! Once the contract is deployed, you'll see the address and all available read/write methods to interact with it.

The Moonbeam Remix Plugin works seamlessly with Remix so you can freely switch between using the traditional Remix compile and deploy tabs and the Moonbeam Remix Plugin. 



