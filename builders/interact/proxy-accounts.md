---
title: Proxy Accounts
description: Learn how to set up a proxy account on Moonbeam-based networks.
---

# Setting up a Proxy Account

![Setting up a Proxy Account](/images/builders/interact/proxy-accounts/proxies-banner.png)

## Introduction {: #introduction } 

Proxy accounts can be set up to perform a limited number of actions on behalf of users and are useful for keeping the underlying accounts safe. They allow users to keep their primary account secured safely in cold storage while enabling the proxy to actively perform functions and participate in the network with the weight of the tokens in the primary account.

Proxy accounts can be set up to perform specific Substrate functions such as author mapping, staking, balances, and more. This can allow you to, for example, grant a trusted individual access to perform collator or delegator functions on your behalf. A proxy could also be used to keep a staking account safe in cold storage.

This guide will show you how to set up a proxy account on the Moonbase Alpha TestNet for balance transfers and how to execute a proxy transaction.

## Checking Prerequisites

To follow along with this tutorial, you will need to have:

- [Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.api.moonbase.moonbeam.network#/explorer){target=_blank} open and connected to Moonbase Alpha
- Create or have two accounts on Moonbase Alpha
- At least one of the accounts will need to be funded with `DEV` tokens. You can obtain tokens for testing purposes from the Moonbase Alpha [faucet](/builders/get-started/moonbase/#get-tokens/)

If you need help importing your accounts into Polkadot.js Apps, please check out the [Interacting with Moonbeam using Polkadot.js Apps](/tokens/connect/polkadotjs/#creating-or-importing-an-h160-account) guide.

## General Definitions

When setting up a proxy account, a bond for the proxy is taken out of your free balance and moved to your reserved balance. The bond is required as adding a proxy requires on-chain storage space, and it is recalculated for each proxy you add or remove. After all proxies are removed from your account, the bond is returned to your free balance.

The deposit is calculated based on a deposit base and a deposit factor:

- **Deposit base** - the amount to be reserved for an account to have a proxy list
- **Deposit factor** - the additional amount to be reserved for every proxy the primary account has

The equation for calculating the deposit is: 

```
deposit base + deposit factor * number of proxies
```

=== "Moonbeam"
    |    Variable    |                       Value                       |
    |:--------------:|:-------------------------------------------------:|
    |  Deposit base  |  {{ networks.moonbeam.proxy.deposit_base }} GLMR  |
    | Deposit factor | {{ networks.moonbeam.proxy.deposit_factor }} GLMR |
    |  Max proxies   | {{ networks.moonbeam.proxy.max_proxies }} proxies |

=== "Moonriver"
    |    Variable    |                       Value                        |
    |:--------------:|:--------------------------------------------------:|
    |  Deposit base  |  {{ networks.moonriver.proxy.deposit_base }} MOVR  |
    | Deposit factor | {{ networks.moonriver.proxy.deposit_factor }} MOVR |
    |  Max proxies   | {{ networks.moonriver.proxy.max_proxies }} proxies |

=== "Moonbase Alpha"
    |    Variable    |                       Value                       |
    |:--------------:|:-------------------------------------------------:|
    |  Deposit base  |  {{ networks.moonbase.proxy.deposit_base }} DEV   |
    | Deposit factor | {{ networks.moonbase.proxy.deposit_factor }} DEV  |
    |  Max proxies   | {{ networks.moonbase.proxy.max_proxies }} proxies |

## Proxy Types

When creating a proxy account, you must choose a type of proxy which will define how the proxy can be used. The available options are:

- **`AuthorMapping`** - this type of proxy account is used by collators to migrate services from one server to another
- **`CancelProxy`** - allows the proxy account to reject and remove any announced proxy calls
- **`Staking`** - allows the proxy account to perform staking-related transactions, such as collator or delegator functions, including `authorMapping()`
- **`Governance`** - allows the proxy account to make transactions related to governance, such as voting or proposing democracy proposals
- **`NonTransfer`** - this type of proxy account is allowed to submit any type of transaction with the exception of balance transfers
- **`Balances`** - allows the proxy account to only make transactions related to sending funds
- **`Any`** - allows the proxy account to use any function supported by the proxy pallet

For the purposes of this guide, you will be setting up a proxy account using the balances proxy type. Since this type enables the proxy to spend funds on behalf of the primary account, you should exercise caution and only provide access to accounts you trust. The proxy will have access to transfer all of the funds within the primary account, and if not trusted, the proxy could drain the primary account. Also make sure not to forget to remove the proxy as needed.

## Creating a Proxy Account

There are a couple of ways you can create proxy accounts in Polkadot.js Apps, either from the **Extrinsics** page or the **Accounts** page. However, to create a time-delayed proxy, you will need to use the **Extrinsics** page. A time delay provides an additional layer of security to proxies by specifying a delay period based on a number of blocks. This will prevent the proxy account from executing a transaction until the delay period ends. The delay allows time for the primary account that controls the proxy to review pending transactions, potentially for malicious actions, and cancel if necessary before execution. 

To get started creating your proxy account, head to the **Developer** tab and select **Extrinsics** from the dropdown. Next, you will need to take the following steps:

1. Select the primary account
2. From the **submit the following extrinsic** dropdown, select **proxy**
3. Choose the **addProxy** extrinsic
4. Select the **delegate** account for the proxy
5. From the **proxyType** dropdown, choose **Balances**
6. Optionally you can add a time delay using a specified number of blocks to add an additional layer of security for the primary account to review the pending transaction
7. Click **Submit Transaction** 

![Creating a Proxy Account](/images/builders/interact/proxy-accounts/proxies-1.png)

You will then be prompted to authorize and sign the transaction. Go ahead and click **Sign and Submit** to create the proxy relationship. 

![Submit Transaction to Create a Proxy Account](/images/builders/interact/proxy-accounts/proxies-2.png)

Once the transaction has been successfully submitted, you will receive some notifications confirming the transaction.

As previously mentioned, you can also create a proxy from the **Accounts** page. To do so, navigate to the **Accounts** page, select the 3 vertical dots next to the primary account and select **Add proxy**.

![Add Proxy](/images/builders/interact/proxy-accounts/proxies-3.png)

!!! note
    If the account already has a proxy, **Manage proxies** will be displayed as an option instead of **Add proxy**.

A pop-up will appear and you will be able to enter in the required information, such as the proxied/primary account, the proxy account, and type of proxy in order to create a proxy account.

![Add Proxy from Accounts Page](/images/builders/interact/proxy-accounts/proxies-4.png)

In the next section, you will learn how to verify that your proxy account was set up successfully. 

## Verifying your Proxy Account

There are a couple of ways that you can verify your proxy account has been successfully set up. Either through the **Accounts** page or via the **Chain state** page.

To check your proxy accounts from the **Chain state** page, you can take the following steps:

1. Click on the **Developer** tab
2. From the dropdown, select **Chain state**
3. From the **selected state query** dropdown, select **proxy**
4. Choose the **proxies** extrinsic
5. Select your primary/proxied account
6. Click on the **+** button to send the query

![Verify your Proxy Accounts](/images/builders/interact/proxy-accounts/proxies-5.png)

The result will appear on the page showing you information about all of your proxies, including the delegate/proxy account address, the proxy type, the delay period if one was specified, and the total bond amount for all of your proxies in Wei.

As previously mentioned, you can also check your proxy accounts from the **Accounts** page. To do so, you can navigate to the **Accounts** page and there should be a Proxy symbol next to the primary account. Hover over the icon and click on **Proxy overview** to review your proxies.

![Proxy Overview Button](/images/builders/interact/proxy-accounts/proxies-6.png)

A pop-up will appear where you can view an overview of all of your proxy accounts.

![Proxy Overview Pop-up](/images/builders/interact/proxy-accounts/proxies-7.png)

## Executing a Proxy Transaction

Now that you have created a proxy account and verified that it was successfuly set up, you can execute a transaction using the proxy account on behalf of the primary account.

To execute a transaction, you can navigate back to the **Extrinsics** page and take the following steps: 

1. Select the proxy account to submit the transaction from the **using the select account** dropdown
2. From the **submit the following extrinsic** menu, select **proxy**
3. Choose the **proxy** extrinsic
4. Select the primary account from the **real** dropdown
5. Select the **balances** call
6. Choose the **transfer** extrinsic
7. In the **dest** field, enter the address you would like to send funds to
8. In the **value** field, enter the amount of funds to send in Wei. For this example, you can send 2 DEV tokens, which will be `2000000000000000000` in Wei
9. Click **Submit Transaction**

![Execute a Proxy Transaction](/images/builders/interact/proxy-accounts/proxies-8.png)

A pop-up will appear for you to authorize and sign the transaction. Enter your password for the proxy account and click **Sign and Submit**.

If the transaction successfully went through, you should see a couple of notifications pop-up and if you head over to the **Accounts** page, you'll see that the balance of your primary account has decreased. If you check the balance of the account where you sent the funds to, you'll notice the balance there has increased.

That's it! You've successfully executed a transaction using a proxy account on behalf of your primary account. 

## Removing a Proxy Account

Similarly to adding a proxy account, there are a couple of ways that you can remove a proxy account, either from the **Extrinsics** page or the **Accounts** page. Regardless of which page you use, you can elect to remove a single proxy account or all proxies associated with your primary account.

To remove a proxy from the **Extrinsics** page, you can take the following steps:

1. From the **using the selected account** dropdown, select your primary account
2. Then select **proxy**
3. Choose **removeProxy** to remove a single proxy or **removeProxies** to remove all associated proxies
4. If removing a single proxy, enter the proxy account to remove in the **delegate** field
5. Select the **proxyType** to remove, in this case choose **Balances**
6. Optionally, select a delay period in block numbers
7. Click **Submit Transaction**

![Remove a Proxy Account](/images/builders/interact/proxy-accounts/proxies-9.png)

A pop-up will appear for you to authorize and sign the transaction. There is the option of signing and sending the transaction from the primary account or the proxy account, but in order to remove the proxy, the transaction must be sent from the primary account. Enter your password and click **Sign and Submit**.

You can follow the steps in the [Verifying your Proxy Account](#verifying-your-proxy-account) section to check that the proxy or proxies have been removed. 

As previously mentioned, you can also remove a proxy from the **Accounts** page. To do so, on the **Accounts** page, select the 3 vertical dots next to the primary account and select **Manage Proxies**. 

![Manage Proxies](/images/builders/interact/proxy-accounts/proxies-10.png)

A pop-up will appear showing an overview of your proxy accounts. To remove a single proxy you can select the **X** button next to the proxy to remove. The proxy will disappear from the list, then you will need to click **Submit**. Next you will be able to enter your password and submit the transaction. Or to remove all proxies you can click on **Clear all**, then you will automatically be prompted to enter your password and submit the transaction.

![Remove a Proxy Account from the Accounts Page](/images/builders/interact/proxy-accounts/proxies-11.png)
 
Once the transaction has successfully been submitted, you can review your current proxies or if you removed all proxies you will notice the proxy icon is no longer being displayed next to the primary account. 

And that's it! You've successfully created a proxy, reviewed all proxy accounts associated with your primary account, executed a proxy transaction, and removed a proxy account!
