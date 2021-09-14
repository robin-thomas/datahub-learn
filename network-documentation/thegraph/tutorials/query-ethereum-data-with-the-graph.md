# Introduction

In this tutorial, you will learn how to create a subgraph from an already deployed smart contract on Rinkeby testnet, deploy it to the Subgraph Studio, and then use the Subgraph Studio Playground to query the subgraph.

![Subgraph Studio](../../../.gitbook/assets/graph.png)

# Prerequisites

To successfully master this tutorial, you will need to have some basic understanding of Ethereum, Metamask and npm/node ecosystem.

# Requirements

- You will need Metamask installed in your browser.
- You need to have a recent version of Node.js installed. We recommend using v14.17.6 LTS version for compatibility.

Topics covered in this tutorial:

- Creating a subgraph in the Subgraph Studio
- Downloading the contract abi and then creating a subgraph from it
- Deploying the subgraph to Subgraph Studio
- Querying the subgraph using Subgraph Studio Playground
- Using metamask to interact with the Rinkeby smart contract

Topics not covered in this tutorial:

- Writing the smart contract and then deploying it to Rinkeby testnet

# Project setup

Run the below commands to install the npm dependencies globally. These are required to build and deploy your subgraph.

```console
$ npm i -g yarn
$ npm i -g @graphprotocol/graph-cli
```

# Creating the graph project in Subgraph Studio

First, headover to the Subgraph Studio at https://thegraph.com/studio/.

![Login to Subgraph Studio](../../../.gitbook/assets/graph_connect.png)

Click on the **Connect Wallet** button. Choose a Metamask wallet to login with. Once you are authenticated, you shall see the below screen, where you can create your first subgraph.

![Create your first subgraph](../../../.gitbook/assets/graph_create_subgraph.png)

Next, you need to give your subgraph a name. Give the name as **vending**. Once that's done, you shall see the below screen.

![Subgraph dashboard](../../../.gitbook/assets/graph_subgraph_created.png)

On this screen, you can see details like your deploy key, subgraph slug and status.

# Creating and deploying the subgraph

- Run the below command to create the subgraph by downloading the contract abi from rinkeby testnet. A new directory called `vending` shall be created, and all node dependencies will be installed automatically.

```console
$ graph init --contract-name VendingMachine --index-events --studio --from-contract 0x4006c82FfB71933160948626dB3Ff8D8aaad6510 --network rinkeby vending
```

This will fetch the abi from rinkeby smart contract passed as an argument to `--from-contract`, and store it under `./vending/abis` directory. The Graph CLI will auto-generate the mappings from the smart contract abi. You can see the mapping under `./vending/src/mapping.ts`.

> By passing in --index-events the CLI will automatically populate some code for us both in schema.graphql as well as src/mapping.ts based on the events emitted from the contract.

Output:

```console
✔ Subgraph slug · vending
✔ Directory to create the subgraph in · vending
✔ Ethereum network · rinkeby
✔ Contract address · 0x4006c82FfB71933160948626dB3Ff8D8aaad6510
✔ Fetching ABI from Etherscan
✔ Contract Name · VendingMachine
———
  Generate subgraph from ABI
  Write subgraph to directory
✔ Create subgraph scaffold
✔ Initialize subgraph repository
✔ Install dependencies with yarn
✔ Generate ABI and schema types with yarn codegen

Subgraph vending created in vending

Next steps:

  1. Run `graph auth` to authenticate with your deploy key.

  2. Type `cd vending` to enter the subgraph.

  3. Run `yarn deploy` to deploy the subgraph.

Make sure to visit the documentation on https://thegraph.com/docs/ for further information.
```


- Run the below command to set the deploy key. Replace <DEPLOY_KEY> in the below command with the key you got from https://thegraph.com/studio/subgraph/vending/

```console
$ graph auth --studio <DEPLOY_KEY>
Deploy key set for https://api.studio.thegraph.com/deploy/
```

- Run the below command to create the subgraph. Your new subgraph will be created in `subgraph.yaml` file.

```console
$ cd vending
$ graph codegen && graph build
```

This will generate a `build` directory under `vending/build`.

Output:

```console
Skip migration: Bump mapping apiVersion from 0.0.1 to 0.0.2
Skip migration: Bump mapping apiVersion from 0.0.2 to 0.0.3
Skip migration: Bump mapping apiVersion from 0.0.3 to 0.0.4
Skip migration: Bump mapping specVersion from 0.0.1 to 0.0.2
✔ Apply migrations
✔ Load subgraph from subgraph.yaml
Load contract ABI from abis/VendingMachine.json
✔ Load contract ABIs
Generate types for contract ABI: VendingMachine (abis/VendingMachine.json)
Write types to generated/VendingMachine/VendingMachine.ts
✔ Generate types for contract ABIs
✔ Generate types for data source templates
✔ Load data source template ABIs
✔ Generate types for data source template ABIs
✔ Load GraphQL schema from schema.graphql
Write types to generated/schema.ts
✔ Generate types for GraphQL schema

Types generated successfully

Skip migration: Bump mapping apiVersion from 0.0.1 to 0.0.2
Skip migration: Bump mapping apiVersion from 0.0.2 to 0.0.3
Skip migration: Bump mapping apiVersion from 0.0.3 to 0.0.4
Skip migration: Bump mapping specVersion from 0.0.1 to 0.0.2
✔ Apply migrations
✔ Load subgraph from subgraph.yaml
Compile data source: VendingMachine => build/VendingMachine/VendingMachine.wasm
✔ Compile subgraph
Copy schema file build/schema.graphql
Write subgraph file build/VendingMachine/abis/VendingMachine.json
Write subgraph manifest build/subgraph.yaml
✔ Write compiled subgraph to build/

Build completed: /temp/vending/build/subgraph.yaml
```

- Run the below command to deploy the subgraph to Subgraph Studio.

```console
$ graph deploy --studio vending
```

You shall be prompted for a version label. You can choose `1.0.1`.

Output:

```console
✔ Version Label (e.g. v0.0.1) · v1.0.1
  Skip migration: Bump mapping apiVersion from 0.0.1 to 0.0.2
  Skip migration: Bump mapping apiVersion from 0.0.2 to 0.0.3
  Skip migration: Bump mapping apiVersion from 0.0.3 to 0.0.4
  Skip migration: Bump mapping specVersion from 0.0.1 to 0.0.2
✔ Apply migrations
✔ Load subgraph from subgraph.yaml
  Compile data source: VendingMachine => build/VendingMachine/VendingMachine.wasm
✔ Compile subgraph
  Copy schema file build/schema.graphql
  Write subgraph file build/VendingMachine/abis/VendingMachine.json
  Write subgraph manifest build/subgraph.yaml
✔ Write compiled subgraph to build/
  Add file to IPFS build/schema.graphql
                .. QmWEbGspWEL95ipBPxDCFFrddswSBsprwx1Ktm35Xk2t5M
  Add file to IPFS build/VendingMachine/abis/VendingMachine.json
                .. QmPvqMksRmuchK4Q7kL2KpKg4ZZEsBXwHE34PE55YZUd1Y
  Add file to IPFS build/VendingMachine/VendingMachine.wasm
                .. QmYR3nJToYt9HeuLhXeZ4rw11JrNJyu1NE5n4LQ8DCYsyi
✔ Upload subgraph to IPFS

Build completed: QmZMbjNuaEm1KvAhVaqE29Hz4EJfdNcDwtHgmGifWNrutV

Deployed to https://thegraph.com/studio/subgraph/vending

Subgraph endpoints:
Queries (HTTP):     https://api.studio.thegraph.com/query/8676/vending/v1.0.1
Subscriptions (WS): https://api.studio.thegraph.com/query/8676/vending/v1.0.1
```

Subgraph Studio might take few minutes to sync the graph from rinkeby testnet. Wait until the syncing process is complete.

Once the sync is complete, you shall have access to the Playground to run your queries.

# Querying the graph

Head over to https://thegraph.com/studio/subgraph/vending/ to start querying data with the Playground.

![Subgraph Studio Playground](../../../.gitbook/assets/graph_playground.png)

```graphql
{
  purchases {
    buyer
    amount
    remaining
    timestamp
  }
}
```

We can set the order direction using `orderBy` and `orderDirection`:

```graphql
{
  purchases(
    orderBy: timestamp
    orderDirection: desc
  ) {
    buyer
    amount
    remaining
    timestamp
  }
}
```

Or get the first `x` results using the `first` clause:

```graphql
{
  purchases(
    orderBy: timestamp
    orderDirection: desc
    first: 2
  ) {
    buyer
    amount
    remaining
    timestamp
  }
}
```

Or skip forward results using the `skip` clause:

```graphql
{
  purchases(
    orderBy: timestamp
    orderDirection: desc
    skip: 2
  ) {
    buyer
    amount
    remaining
    timestamp
  }
}
```

Or filter the results by certain conditions using the `where` clause:

```graphql
{
  purchases(
    orderBy: timestamp
    orderDirection: desc
    where: { amount_gt: 5 }
  ) {
    buyer
    amount
    remaining
    timestamp
  }
}
```

### Creating data and querying it

Headover to https://rinkeby.etherscan.io/address/0x4006c82ffb71933160948626db3ff8d8aaad6510#writeContract to see the rinkeby smart contract.

![Rinkeby Smart Contract](../../../.gitbook/assets/graph_rinkeby_contract.png)

Click on the **Connect to Web3** button. Choose **Metamask** from the wallet options. After authenticatng with Metamask, you shall see **Connected - Web3[address]** instead of the earlier button. Make sure you are on the **Rinkeby** test network.

Click on the **purchase** row. You need to fill in **purchase** and **amount** fields.

![Rinkeby Smart Contract Purchase](../../../.gitbook/assets/graph_rinkeby_contract_purchase.png)

Fill in **purchase** as **0.2** and **amount** as **20**. Click on the **Write** button.

It will open up the metamask prompt. Make sure you are on the **Rinkeby** test network.

![Metamask](../../../.gitbook/assets/graph_metamask.png)

Once you click on the **Confirm** button, wait for your transaction to be mined.

Then come back to Playground, and run the below query (after replacing `<YOUR_ADDRESS>` with your Metamask wallet address):

```graphql
{
  purchases(
    orderBy: timestamp
    orderDirection: desc
    where: { buyer: "<YOUR_ADDRESS>" }
  ) {
    buyer
    amount
    remaining
    timestamp
  }
}
```

You shall get purchase you just created!

```json
{
  "data": {
    "purchases": [
      {
        "amount": "20",
        "buyer": "<YOUR_ADDRESS>",
        "remaining": "84",
        "timestamp": "1631577213"
      }
    ]
  }
}
```

# Conclusion

Congratulations on finishing this tutorial! You have now learnt how to create and deploy your first subgraph, as well as query the subgraph using Subgraph Studio Playground.

# About the Author

I'm Robin Thomas, a blockchain enthusiast with few years of experience working with various blockchain protocols. Feel free to connect with me on [GitHub](https://github.com/robin-thomas).
