---
title: Moonbeam
description: Learn how to get started and connect to Moonbeam, the deployment on Polkadot, via RPC and WSS endpoints.
---

# Get Started with Moonbeam

--8<-- 'text/moonbeam/connect.md'

## Block Explorers {: #block-explorers }

For Moonbeam, you can use any of the following block explorers:

 - **Ethereum API (Etherscan Equivalent)** — [Moonscan](https://moonbeam.moonscan.io/){target=_blank}
 - **Ethereum API with Indexing** — [Blockscout](https://blockscout.moonbeam.network/){target=_blank}
 - **Ethereum API JSON-RPC based** — [Moonbeam Basic Explorer](https://moonbeam-explorer.netlify.app/?network=Moonbeam){target=_blank}
 - **Substrate API** — [Subscan](https://moonbeam.subscan.io/){target=_blank} or [Polkadot.js Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fwss.api.moonbeam.network#/explorer){target=_blank}

For more information on each of the available block explorers, please head to the [Block Explorers](/builders/tools/explorers) section of the documentation.

## Connect MetaMask {: #connect-metamask }

If you already have MetaMask installed, you can easily connect MetaMask to Moonbeam:

<div class="button-wrapper">
    <a href="#" class="md-button connectMetaMask" value="moonbeam">Connect MetaMask</a>
</div>

!!! note
    MetaMask will popup asking for permission to add Moonbeam as a custom network. Once you approve permissions, MetaMask will switch your current network to Moonbeam.

If you do not have MetaMask installed, or would like to follow a tutorial to get started, please check out the [Interacting with Moonbeam using MetaMask](/tokens/connect/metamask/) guide.

If you want to connect MetaMask by providing the network information, you can use the following data:

 - Network Name: `Moonbeam`
 - RPC URL: `{{ networks.moonbeam.rpc_url }}`
 - ChainID: `{{ networks.moonbeam.chain_id }}` (hex: `{{ networks.moonbeam.hex_chain_id }}`)
 - Symbol (Optional): `GLMR`
 - Block Explorer (Optional): `{{ networks.moonbeam.block_explorer }}` 
