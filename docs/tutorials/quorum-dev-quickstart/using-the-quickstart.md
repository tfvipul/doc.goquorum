---
title: Use the quickstart
description: Helps to generate local GoQuorum and Ethereum blockchain networks.
sidebar_position: 1
---

import TestAccounts from '../../global/\_test_accounts.md';

# Use Quorum Developer Quickstart

The Quorum Developer Quickstart uses the GoQuorum Docker image to run a private [IBFT](../../configure-and-manage/configure/consensus-protocols/ibft.md) network of GoQuorum nodes managed by Docker Compose.

:::warning

This tutorial runs a private network suitable for education or demonstration purposes and is not intended for running production networks.

:::

## Prerequisites

- [Node.js and NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) version 14 or higher
- [Docker and Docker-compose](https://docs.docker.com/compose/install/)
- [Hardhat](https://hardhat.org/hardhat-runner/docs/getting-started#overview) development framework
- [Solc](https://github.com/ethereum/solidity/releases/tag/v0.5.17)
- [`curl` command line](https://curl.haxx.se/download.html)
- [MetaMask](https://metamask.io/)

:::caution

Allow Docker up to 4G of memory or 6G if running the privacy examples. Refer to the **Resources** section in [Docker for Mac](https://docs.docker.com/docker-for-mac/) and [Docker Desktop](https://docs.docker.com/docker-for-windows/) for details.

:::

## Generate the tutorial blockchain configuration files

To create the tutorial `docker-compose` files and artifacts, run:

```bash
npx quorum-dev-quickstart
```

Follow the prompts displayed to run GoQuorum and [logging with ELK](../../configure-and-manage/monitor/elastic-stack.md). Enter `n` for [Codefi Orchestrate](https://docs.orchestrate.consensys.net/en/stable/) and `y` for [private transactions](../../concepts/privacy-index.md).

## Start the network

To start the network, go to the installation directory (`quorum-test-network` if you used the default value) and run:

```bash
./run.sh
```

The script builds the Docker images, and runs the Docker containers.

Four GoQuorum IBFT validator nodes and a non-validator node are created to simulate a base network. In addition, there are three member pairs (GoQuorum and Tessera sets) to simulate private nodes on the network.

When execution is successfully finished, the process lists the available services:

```
*************************************
Quorum Dev Quickstart
*************************************
----------------------------------
List endpoints and services
----------------------------------
JSON-RPC HTTP service endpoint                 : http://localhost:8545
JSON-RPC WebSocket service endpoint            : ws://localhost:8546
Web block explorer address                     : http://localhost:25000/
Prometheus address                             : http://localhost:9090/graph
Cakeshop toolkit address                       : http://localhost:8999
Grafana address                                : http://localhost:3000/d/a1lVy7ycin9Yv/goquorum-overview?orgId=1&refresh=10s&from=now-30m&to=now&var-system=All
Collated logs using Grafana Loki               : http://localhost:3000/d/Ak6eXLsPxFemKYKEXfcH/quorum-logs-loki?orgId=1&var-app=quorum&var-search=

For more information on the endpoints and services, refer to README.md in the installation directory.
****************************************************************
```

- Use the **JSON-RPC HTTP service endpoint** to access the RPC node service from your dapp or from cryptocurrency wallets such as MetaMask.
- Use the **JSON-RPC WebSocket service endpoint** to access the WebSocket node service from your dapp.
- Use the [**Web block explorer address**](http://localhost:25000) to display the block explorer web application.
- Use the [**Prometheus address**](http://localhost:9090/graph) to access the Prometheus dashboard and [monitor nodes and view metrics](../../configure-and-manage/monitor/metrics.md).
- Use the [**Grafana address**](http://localhost:3000/d/a1lVy7ycin9Yv/goquorum-overview?orgId=1&refresh=10s&from=now-30m&to=now&var-system=All) to access the Grafana dashboard to [monitor nodes and view metrics](../../configure-and-manage/monitor/metrics.md)
- Use the [**Loki address**](http://localhost:3000/d/Ak6eXLsPxFemKYKEXfcH/quorum-logs-loki?orgId=1&var-app=quorum&var-search=) to view the container [logs](../../configure-and-manage/monitor/loki.md).
- Use the [**Kibana logs address**](http://localhost:5601/app/kibana#/discover) to [access and manage logs in Kibana](../../configure-and-manage/monitor/elastic-stack.md).

To display the list of endpoints again, run:

```bash
./list.sh
```

## Use a block explorer

The quickstart supports a modified version of the [Alethio Ethereum Lite Explorer](#alethio-ethereum-lite-explorer) and [BlockScout](#blockscout).

### Alethio Ethereum Lite Explorer

Access the [Alethio Ethereum Lite Explorer](https://github.com/Alethio/ethereum-lite-explorer) at [`http://localhost:25000`](http://localhost:25000) as displayed when starting the private network.

The block explorer displays a summary of the private network, indicating four peers.

Click the block number to the right of **Best Block** to display the block details:

![Block Details](../../images/quickstart/ExplorerBlockDetails.png)

You can explore blocks by clicking on the blocks under **`Bk`** on the left-hand side.

You can search for a specific block, transaction hash, or address by clicking the :mag: in the top left-hand corner.

![Explorer Search](../../images/quickstart/ExplorerSearch.png)

### BlockScout

At the prompt **Do you wish to enable support for monitoring your network with BlockScout?**, enter `Y` to start BlockScout at [`http://localhost:26000`](http://localhost:26000).

:::note

BlockScout's Docker image is resource heavy when running. Ensure you have adequate CPU resources dedicated to the container.

:::

The [quickstart BlockScout configuration](https://github.com/ConsenSys/quorum-dev-quickstart/blob/master/templates/goquorum/docker-compose.yml) is available as a reference for your own network.

## Monitor nodes with Prometheus and Grafana

The sample network also includes Prometheus and Grafana monitoring tools to let you visualize node health and usage. You can directly access these tools from your browser at the addresses displayed in the endpoint list.

- [Prometheus dashboard](http://localhost:9090/graph).
- [Grafana dashboard](http://localhost:3000/d/a1lVy7ycin9Yv/goquorum-overview?orgId=1&refresh=10s&from=now-30m&to=now&var-system=All).
- [Grafana Loki logs dashboard](http://localhost:3000/d/Ak6eXLsPxFemKYKEXfcH/quorum-logs-loki?orgId=1&var-app=quorum&var-search=).

For more details on how to configure and use these tools for your own nodes, see our [performance monitoring documentation](../../configure-and-manage/monitor/metrics.md), the [Prometheus documentation](https://prometheus.io/docs/introduction/overview/) and [Grafana documentation](https://grafana.com/docs/).

![Grafana](../../images/dashboard_grafana_1.png)

and collated logs via Grafana Loki

![Loki](../../images/dashboard_grafana_loki.png)

## Run JSON-RPC requests

You can run JSON-RPC requests on:

- HTTP with `http://localhost:8545`.
- WebSockets with `ws://localhost:8546`.

### Run with `curl`

This tutorial uses [`curl`](https://curl.haxx.se/download.html) to send JSON-RPC requests over HTTP.

### Request the node version

Run the following command from the host shell:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":1}' -H 'Content-Type: application/json' http://localhost:8545
```

The result displays the client version of the running node:

<!--tabs-->

# Result example

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "Geth/node5-istanbul/v1.9.20-stable-1d7926a1(quorum-v21.4.2)/linux-amd64/go1.15.5"
}
```

# Result explanation

- `"jsonrpc" : "2.0"` indicates that the JSON-RPC 2.0 spec format is used.
- `"id" : 1` is the request identifier used to match the request and the response. This tutorial always uses 1.
- `"result"` contains the running GoQuorum information:
  - `v1.9.20-stable-1d7926a1` is the Geth build that has been used for GoQuorum
  - `quorum-v21.4.2` is the running GoQuorum version number. This may be different when you run this tutorial.
  - `linux-amd64` is the architecture used to build this version.
  - `go1.15.5` is the Go version used. This may be different when you run this tutorial.

<!--/tabs-->

Successfully calling this method shows that you can connect to the nodes using JSON-RPC over HTTP.

From here, you can walk through more interesting requests demonstrated in the rest of this section, or skip ahead to [Create a transaction using MetaMask](#create-a-transaction-using-metamask).

### Count the peers

Peers are the other nodes connected to the node receiving the JSON-RPC request.

Poll the peer count using `net_peerCount`:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}' -H 'Content-Type: application/json'  http://localhost:8545
```

The result indicates seven peers (our validators):

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x7"
}
```

### Request the most recent block number

Call `eth_blockNumber` to retrieve the number of the most recently synchronized block:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' -H 'Content-Type: application/json' http://localhost:8545
```

The result indicates the highest block number synchronized on this node.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x2a"
}
```

Here the hexadecimal value `0x2a` translates to decimal as `42`, the number of blocks received by the node so far, about two minutes after the new network started.

## Public transactions

This example uses the [web3.js](https://www.npmjs.com/package/web3) library to make the API calls, using the `rpcnode`
accessed on `http://localhost:8545`.

Navigate to the `smart_contracts` directory and deploy the public transaction:

```bash
cd smart_contracts
npm install
node scripts/public/public_tx.js
# or via ethers
node scripts/public/public_tx_ethers.js
```

This deploys the contract and sends an arbitrary value (`47`) from `Member1` to `Member3`. The script then performs:

1. A read operation on the contract using the `get` function and the contract's ABI, at the specified address.
1. A write operation using the `set` function and the contract's ABI, at the address and sets the value to `123`.
1. A read operation on all events emitted.

The script output is as follows:

```bash
{
  address: '0x03e034D03b04A348143F2C25884e1E4946fa6196',
  privateKey: '0xca3a6f8b83ed5876201605ae8507490d0a0205c0748e6376ed9661c9fecb98d7',
  signTransaction: [Function: signTransaction],
  sign: [Function: sign],
  encrypt: [Function: encrypt]
}
create and sign the txn
sending the txn
tx transactionHash: 0x6e62d7db1ec001a127130da6434358f44af0f6adee0466902e3eb8160f31c950
tx contractAddress: 0x1fBDd454d617E21c7121161b154B764Dc5844fc9
Contract deployed at address: 0x1fBDd454d617E21c7121161b154B764Dc5844fc9
Use the smart contracts 'get' function to read the contract's constructor initialized value .. 
Obtained value at deployed contract is: 47
Use the smart contracts 'set' function to update that value to 123 .. 
Set value on contract at : 0xdcaf84bbdd35811820dbb13453e1532d5cd3e3ef718e12a49878de67948e0b9b
Verify the updated value that was set .. 
Obtained value at deployed contract is: 123
Obtained all value events from deployed contract : [47,123]
```

We also have a second example that shows how to transfer ETH between accounts. Navigate to the `smart_contracts` directory
and deploy the `eth_tx` transaction:

```bash
cd smart_contracts
npm install
node scripts/public/eth_tx.js
```

The output is as follows:

```bash
Account A has balance of: 90000
Account B has balance of: 0
create and sign the txn
sending the txn
tx transactionHash: 0x8b9d247900f2b50a8dded3c0d73ee29f04487a268714ec4ebddf268e73080f98
Account A has an updated balance of: 89999.999999999999999744
Account B has an updated balance of: 0.000000000000000256
```

## Private transactions

This example uses the [web3.js](https://www.npmjs.com/package/web3) library to make the API calls, creating three member nodes pairs (a GoQuorum node which has a corresponding Tessera node for privacy) that can be accessed using APIs on the following ports:

```bash
Member1Quorum RPC: http://localhost:20000
Member1Tessera: http://localhost:9081

Member2Quorum RPC: http://localhost:20002
Member1Tessera: http://localhost:9082

Member3Quorum RPC: http://localhost:20004
Member1Tessera: http://localhost:9083
```

Navigate to the `smart_contracts` directory and deploy the private transaction:

```bash
cd smart_contracts
npm install
node scripts/private/private_tx.js
```

This deploys the contract and sends an arbitrary value (`47`) from `Member1` to `Member3`.

Once done, it performs a read operation on the contract using the `get` function and the contract's ABI, at the address specified.

It then performs a write operation using the `set` function and the contract's ABI, at the address and sets the value to `123`.

Lastly, it performs a read operation on all three members to verify that this is private between `Member1` and `Member3` only, and you should see that only `Member1` and `Member3` return the result of `123`, and `Member2` has an undefined value.

```bash
node scripts/private/private_tx.js
The transaction hash is: 0x4d796b2ccac109fc54006105df44c519341696fa88e004ce5c614239cb9f92a2
Address of transaction:  0x695Baaf717370fcBb42aB45CD83C531C27D79eF1
Use the smart contracts 'get' function to read the contract's constructor initialized value ..
Member1 obtained value at deployed contract is: 47
Use the smart contracts 'set' function to update that value to 123 .. - from member1 to member3
Verify the private transaction is private by reading the value from all three members ..
Member1 obtained value at deployed contract is: 123
Member3 obtained value at deployed contract is: 123
Member2 obtained value at deployed contract is: undefined
```

### Inspect the member nodes with `geth attach`

You can inspect any of the GoQuorum nodes by using `attach.sh` to open the geth JavaScript console.

Use a separate terminal window for each of Member1, Member2, and Member3. In each terminal, go to the main directory where `docker-compose.yml` is located, then:

- In terminal 1, run `./attach.sh 1` to attach to Member1.
- In terminal 2, run `./attach.sh 2` to attach to Member2.
- In terminal 3, run `./attach.sh 3` to attach to Member3.

To view the private transaction, run the following command in one of the terminals:

<!--tabs-->

# geth console request

```js
eth.getTransaction(
  "0x4d796b2ccac109fc54006105df44c519341696fa88e004ce5c614239cb9f92a2",
); // replace with your transaction hash
```

# JSON result

```json
{
  "blockHash": "0x3d69d2eb2a50a96072c549805f0ba04ce364b68ef7c16cd0ddac8e6c184e599e",
  "blockNumber": 823,
  "from": "0xf0e2db6c8dc6c681bb5d6ad121a107f300e9b2b5",
  "gas": 150050,
  "gasPrice": 0,
  "hash": "0x4d796b2ccac109fc54006105df44c519341696fa88e004ce5c614239cb9f92a2",
  "input": "0xe619b9d1469c34735145be181a28d18c09b575ef1a8fdbdcb0fe3934c2de5a8c62814e93b087ee918cfa294a0023aa6d42ef360ccf4997f1b94ae1e6c9145a3a",
  "nonce": 6,
  "r": "0x2660131d78ccd80773e8094d9fbf7d030f9753ddb1496af5b12f643ba95f900b",
  "s": "0x50f3e787595b88e5738adba373971d61394c9710d1a0dbee7287d10085d2fef5",
  "to": null,
  "transactionIndex": 0,
  "v": "0x26",
  "value": 0
}
```

<!--/tabs-->

:::note

The `v` field value of `"0x25"` or `"0x26"` (37 or 38 in decimal) indicates this transaction has a private payload (input).

:::

### Read the contract with `get()`

For each of the three nodes, create a variable called `address` using the geth console, and assign to it the address of the contract created by Member1. The contract address can be found:

- In Member1's log file `data/logs/1.log`.
- Using [`eth.getTransactionReceipt(txHash)`](https://web3js.readthedocs.io/en/v1.4.0/web3-eth.html#gettransactionreceipt), where `txHash` is the hash printed to the terminal after sending the transaction. The contract address is found in the result parameter `contractAddress`. It is also printed in the terminal when the private transaction is processed.

After identifying the contract address, run the following command in each terminal:

```js
var address = "0x695Baaf717370fcBb42aB45CD83C531C27D79eF1"; // replace with your contract address
```

Use [`eth.contract`](https://web3js.readthedocs.io/en/v1.4.0/web3-eth-contract.html#eth-contract) to define a contract class with the simpleStorage ABI definition in each terminal:

```js
var address = "0x695baaf717370fcbb42ab45cd83c531c27d79ef1"; // replace with your address
var abi = [
  {
    constant: true,
    inputs: [],
    name: "storedData",
    outputs: [{ name: "", type: "uint256" }],
    payable: false,
    type: "function",
  },
  {
    constant: false,
    inputs: [{ name: "x", type: "uint256" }],
    name: "set",
    outputs: [],
    payable: false,
    type: "function",
  },
  {
    constant: true,
    inputs: [],
    name: "get",
    outputs: [{ name: "retVal", type: "uint256" }],
    payable: false,
    type: "function",
  },
  { inputs: [{ name: "initVal", type: "uint256" }], type: "constructor" },
];
var private = eth.contract(abi).at(address);
```

The function calls are available on the contract instance, and you can call those methods on the contract.

Get the value of the contract to confirm that only Member1 and Member3 can see the value.

- In terminal window 1 (Member1):

  ```js
  private.get();
  123;
  ```

- In terminal window 2 (Member2):

  ```js
  private.get();
  undefined;
  ```

- In terminal window 3 (Member3):

  ```js
  private.get();
  123;
  ```

Member2 can't read the state. Look in `smart_contracts/privacy/node scripts/private_tx.js` to confirm that `123` was the value set when the contract was updated.

### Write to the contract with `set()`

Have Member1 set the state to the value `200` and confirm that only Member1 and Member3 can view the new state.

In terminal window 1 (Member1):

```js
# send to Member3
private.set(200,{from:eth.accounts[0],privateFor:["1iTZde/ndBHvzhcl7V68x44Vx7pl8nwx9LqnM/AfJUg="]});
"0xacf293b491cccd1b99d0cfb08464a68791cc7b5bc14a9b6e4ff44b46889a8f70"
```

You can check the log files in `data/logs/` to see each node validating the block with this new private transaction. Once the block containing the transaction is validated, you can check the state from each of the members.

- In terminal window 1 (Member1):

  ```js
  private.get();
  200;
  ```

- In terminal window 2 (Member2):

  ```js
  private.get();
  undefined;
  ```

- In terminal window 3 (Member3):

  ```js
  private.get();
  200;
  ```

Member2 can't read the state.

All nodes are validating the same blockchain of transactions, with the private transactions containing only a 512-bit hash in place of the transaction data, and only the parties to the private transactions can view and update the state of the private contracts.

## Permissions

GoQuorum supports two types of permissions:

- [Basic permissioning](../../configure-and-manage/configure/permissioning/basic-permissions.md): Controls which peers can connect to a given node. This permissioning method is specific to a given node, and is configured by placing a file with a list of allowed peers (similar to `static-nodes.json`), called `permissioned-nodes.json` in the `data` directory of a GoQuorum node.
- [Enhanced Permissions](../../concepts/permissions-overview.md#enhanced-network-permissioning) is a smart-contract-based permissioning model designed for enterprise-level needs and is applicable to all peers on the network.

This example deploys permissioning contracts then uses API calls to set appropriate permissions.

First we'll select a `GuardianAccount` or an admin account. In this example we'll use the account of Validator1 which has an address of `0xed9d02e382b34818e88b88a309c7fe71e65f419d`, and we'll connect to Validator1's RPC endpoint `http://127.0.0.1:21001`.

The next step is to compile the contracts and deploy them in a specific order. The `contracts` directory (`smart_contracts/permissioning/contracts`) has two versions of contracts.

- `version 1`: Permissioning rules are applied during transaction entry using the permissioning data stored in node memory.
- `version 2`: Permissioning rules are applied at the during transaction entry and block minting using the data stored in the permissioning contracts. This tutorial uses `version 2` because it is more robust and suited for enterprise use.

The folder contains the following contracts:

- `PermissionsInterface.sol`: Provides an external interface, and internal proxy to `PermissionsImplementation.sol`.
- `PermissionsImplementation.sol`: Contains the logic of the permissions-related functionality.
- `OrgManager.sol`: Implements logic for all organization management functionality.
- `AccountManager.sol`: Implements logic for all account management functionality.
- `NodeManager.sol`: Contains the node management functionality.
- `RoleManager.sol`：Implements logic for all role management functionality.
- `VoterManager.sol`: Implements account voter and voting functionality.

Navigate to the `smart_contracts/permissioning` directory and then compile the contracts:

```bash
cd smart_contracts/permissioning
npm install
./scripts/compile.sh
```

Next, deploy the contracts and once complete, execute the `init` function of `PermissionsUpgradable.sol` with the `GuardianAccount` address specified earlier. This is wrapped up into a single script that you can run:

```bash
node ./scripts/deploy.js
```

When completed, a `permission-config.json` file is created which contains the addresses of the contracts and any accounts that serve as admins. In this example we've used all accounts, if you deploy these contracts in a production network, please select only those which need to be admins.

The file looks similar to:

```json
{
    "permissionModel": "v2",
    "upgradableAddress": "0x1932c48b2bf8102ba33b4a6b545c32236e342f34",
    "interfaceAddress": "0x4d3bfd7821e237ffe84209d8e638f9f309865b87",
    "implAddress": "0xfe0602d820f42800e3ef3f89e1c39cd15f78d283",
    "nodeMgrAddress": "0x8a5e2a6343108babed07899510fb42297938d41f",
    "accountMgrAddress": "0x9d13c6d3afe1721beef56b55d303b09e021e27ab",
    "roleMgrAddress": "0x1349f3e1b8d71effb47b840594ff27da7e603d17",
    "voterMgrAddress": "0xd9d64b7dc034fafdba5dc2902875a67b5d586420",
    "orgMgrAddress" : "0x938781b9796aea6376e40ca158f67fa89d5d8a18",
    "nwAdminOrg": "ADMINORG",
    "nwAdminRole" : "ADMIN",
    "orgAdminRole" : "ORGADMIN",
    "accounts":["0xed9d02e382b34818e88b88a309c7fe71e65f419d", "0xca843569e3427144cead5e4d5999a3d0ccf92b8e", ....],
    "subOrgBreadth" : 3,
    "subOrgDepth" : 4
}
```

Where:

- `permissionModel` is the Permission model used (v1 or v2).
- `upgradableAddress` is the address of the deployed `PermissionsUpgradable.sol `contract.
- `interfaceAddress` is the address of the deployed `PermissionsInterface.sol` contract.
- `implAddress` is the address of the deployed `PermissionsImplementation.sol` contract.
- `nodeMgrAddress` is the address of the deployed `NodeManager.sol` contract.
- `accountMgrAddress` is the address of the deployed `AccountManager.sol` contract.
- `roleMgrAddress` is the address of the deployed `RoleManager.sol` contract.
- `voterMgrAddress` is the address of the deployed `VoterManager.sol` contract.
- `orgMgrAddress` is the address of the deployed `OrgManager.sol` contract.
- `nwAdminOrg` - Name of the initial organization to be created as a part of the network boot up with a new permissions model. This organization owns all the initial nodes and network administrator accounts at network boot up.
- `nwAdminRole` - Role ID assigned to the network administrator accounts.
- `orgAdminRole` - Role ID assigned to the organization administrator account.
- `accounts` - Initial list of accounts linked to the network administrator organization and assigned the network administrator role. These accounts have complete control of the network and can propose and approve new organizations into the network.
- `subOrgBreadth` - Number of sub-organizations that any organization can have.
- `subOrgDepth` - Maximum depth of the sub-organization hierarchy allowed in the network.

The last step is to copy the above `permission-config.json` into the `data` directory of each GoQuorum node and restart the nodes. This can be done by running the script:

```bash
./scripts/copyAndRestart.sh
```

Once the network starts up you can use the [API methods](../../configure-and-manage/manage/enhanced-permissions.md) to add or remove permissions.

:::warning Important considerations for production

During network initialization a few things happen:

- A network admin organization is created with the `nwAdminOrg` name that was specified in the `permission-config.json`. All nodes which are part of the `static-nodes.json` will be automatically assigned to this organization
- A network admin role is created with the `nwAdminRole` name specified in the `permission-config.json`
- All accounts given in the `accounts` array of the `permission-config.json` file are assigned the network admin role. These accounts can propose and approve new organizations in the network.

:::

## Use Remix

You can connect your nodes to [Remix](http://remix.ethereum.org) by using the [GoQuorum Plugin](remix.md). Follow the instructions for activating the remix plugin in [Getting Started](remix.md), using the GoQuorum and Tessera URLs in the [Private transactions](#private-transactions) section.

## Use Cakeshop

[Cakeshop](../../configure-and-manage/monitor/cakeshop.md) allows you to perform transactions directly using the UI.

1. Open [`http://localhost:8999`](http://localhost:8999) in your browser.
2. Select the **Contracts** tab and **Deploy** the contract registry.
3. Go to the **Sandbox**, select the `SimpleStorage` sample contract from the Contract Library, and deploy with `Private For` set to the second node's public key (`QfeDAys9MPDs2XHExtc84jKGHxZg/aj52DTh0vtA3Xc=`).
4. Return to the main Cakeshop page, go to the **Contracts** tab again, and you should be able to see the contract you just deployed.
5. Interact with it from there, and switch between nodes using the dropdown in the top right corner of the page.

## Create a transaction using MetaMask

You can use [MetaMask](https://metamask.io/) to send a transaction on your private network.

- Open MetaMask and connect it to your private network RPC endpoint by selecting **Localhost 8545** in the network list.
- Choose one of the following test accounts and [import it into MetaMask by copying the corresponding private key](https://metamask.zendesk.com/hc/en-us/articles/360015489331-How-to-import-an-Account).

<TestAccounts />

After importing an existing test account, [create another test account from scratch] to use as the recipient for a test Ether transaction.

In MetaMask, select the new test account and [copy its address](https://metamask.zendesk.com/hc/en-us/articles/360015289512-How-to-copy-your-MetaMask-Account-Public-Address).

In the [Block Explorer](http://localhost:25000), search for the new test account by clicking on the :mag: and pasting the test account address into the search box.

The new test account displays with a zero balance.

[Send test Ether](https://metamask.zendesk.com/hc/en-us/articles/360015488931-How-to-send-ETH-and-ERC-20-tokens-from-your-MetaMask-Wallet) from the first test account (containing test Ether) to the new test account (which has a zero balance).

:::tip

You can use a zero gas price here as this private test network is a free gas network, but the maximum amount of gas that can be used (the gas limit) for a value transaction must be at least 21000.

:::

Refresh the Block Explorer page in your browser displaying the target test account.

The updated balance reflects the transaction completed using MetaMask.

## Smart contract and dapp usage

You can use a demo dapp called QuorumToken which uses an ERC20 token that is deployed to the network.

We'll use [Hardhat](https://www.npmjs.com/package/hardhat), [Ethers](https://www.npmjs.com/package/ethers) and
[MetaMask](https://metamask.io/) to interact with the network, which involves the following steps:

1. Deploy the contract and **save the contract's address**.
2. Start the dapp, and read and transact with the deployed token.

The `dapps/quorumToken` directory is structured as follows (only relevant paths shown):

```bash
quorumToken
├── hardhat.config.ts       // hardhat network config
├── contracts               // the QuorumToken.sol
├── scripts                 // handy scripts eg: to deploy to a chain
├── test                    // contract tests
└── frontend                // dapp done in Next.js
  ├── public
  ├── src
  ├── styles
  ├── tsconfig.json
```

### Deploy the contract 

Once the network is up and running, enter the `quorumToken` directory and run the following:

```bash
# install dependencies
npm i
# compile the contract
npm run compile
npm run test
# deploy the contract to the quickstart network
npm run deploy-quorumtoken
```

The output is similar to the following:

```bash

# compile
> quorumToken@1.0.0 compile
> npx hardhat compile

Generating typings for: 5 artifacts in dir: typechain-types for target: ethers-v6
Successfully generated 24 typings!
Compiled 5 Solidity files successfully

# test
> quorumToken@1.0.0 test
> npx hardhat test

  QuorumToken
    Deployment
      ✔ Should have the correct initial supply (1075ms)
      ✔ Should token transfer with correct balance (78ms)


  2 passing (1s)

# deploy
Contract deploy at: 0x5FbDB2315678afecb367f032d93F642f64180aa3
```
This will deploy the contract to the network and return the address. **Please save this address for the next step.**

### Run the dapp

The dapp runs a local website using Next.js, and uses the contract deployed on the network in the previous step.

With the blockchain running, and MetaMask connected to `localhost` on port `8545`, import one of
[our test accounts via private key](../../reference/accounts-for-testing.md), and run the following command:

```bash
cd frontend
npm i
npm run dev
```
This starts the dapp, binding it to port `3001` on your machine.

```text

> webapp@0.1.0 dev
> next dev -p 3001

- ready started server on [::]:3001, url: http://localhost:3001
- event compiled client and server successfully in 270 ms (18 modules)
- wait compiling...
- event compiled client and server successfully in 173 ms (18 modules)
```

In the browser where you have MetaMask enabled and one of the test accounts loaded, open a new tab and navigate to
[the QuorumToken dapp](http://localhost:3001). Connect to MetaMask and input the address from the previous step. For
example our contract above deployed to `0x5FbDB2315678afecb367f032d93F642f64180aa3`. 

The dapp will then read the balance of the account from MetaMask and get details of the contract. You can then send funds
to another address (any of the other test accounts) on the network and MetaMask will sign and send the transaction.

You can also search for the transaction and view its details in the [Block Explorer](http://localhost:25000/).

![Dapp UI](../../images/quickstart/dapp-explorer-tx.png)

The MetaMask UI also keeps a record of the transaction.

![MM Dapp UI](../../images/quickstart/dapp-metamask-tx.png)

### Deploy your own dapp

You can deploy your own dapp to the Quorum Developer Quickstart by configuring your dapp to point to the Quickstart network.

We recommend using [Hardhat](https://hardhat.org/hardhat-runner/docs/guides/project-setup), and you can use the sample
`hardhat.config.js` to configure the `networks` object in the [Hardhat configuration file](https://hardhat.org/hardhat-network/docs/reference#config)
to specify which networks to connect to for deployments and testing. The Quickstart's RPC service endpoint is `http://localhost:8545`.

For example, the following is the Hardhat configuration file for the QuorumToken dapp used in the Quickstart GoQuorum network:

```js
module.exports = {
  networks: {
    // in built test network to use when developing contracts
    hardhat: {
      chainId: 1337
    },
    quickstart: {
      url: "http://127.0.0.1:8545",
      chainId: 1337,
      // test accounts only, all good ;)
      accounts: [
        "0x8f2a55949038a9610f50fb23b5883af3b4ecb3c3bb792cbcefbd1542c692be63",
        "0xc87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3",
        "0xae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f"
      ]
    }
  },  
  defaultNetwork: "hardhat",
  ...
  ...
```

Deploy the contract using:

```bash
npx hardhat run ./scripts/deploy_quorumtoken.ts --network quickstart
```

## Stop and restart the private network without removing containers

To shut down the private network without deleting the containers:

<!--tabs-->

# Linux/MacOS

```bash
./stop.sh
```

This command stops the containers related to the services specified in the `docker-compose.yml` file.

To restart the private network:

# Linux/MacOS

```bash
./resume.sh
```

<!--/tabs-->

## Stop the private network and remove containers

To shut down the private network and delete all containers and images created from running the sample network and the QuorumToken dapp:

<!--tabs-->

# Linux/MacOS

```bash
./remove.sh
```

<!--/tabs-->

<!-- Links -->

[create another test account from scratch]: https://metamask.zendesk.com/hc/en-us/articles/360015289452-Creating-Additional-MetaMask-Wallets-New-UI-

## Add a new node to the network

New nodes joining an existing network require the following:

- The same genesis file used by all other nodes on the running network.
- A list of nodes to connect to; this is done by specifying [bootnodes], or by providing a list of [static nodes].
- A node key pair and optionally an account. If the running network is using permissions, then you need to add the new node's enode details to the [permissions file] used by existing nodes, or update the onchain permissioning contract.

The following steps describe the process to add a new node to the Quorum Dev Quickstart.

### 1. Create the node key files

Create a node key pair and account for a new node, by running the following script:

```bash
cd ./extra
npm install
node generate_node_keys.js --password "Password"
```

:::note

The `--password` parameter is optional.

:::

### 2. Create new node directory

In the [`config/nodes`](https://github.com/ConsenSys/quorum-dev-quickstart/tree/master/files/common/config/nodes) directory, create a subdirectory for the new node (for example, `newnode`), and move the `nodekey`, `nodekey.pub`, `address` and `accountkey` files from the previous step into this directory.

### 3. Update docker-compose

Add an entry for the new node into the docker-compose file:

```yaml
newnode:
  << : *quorum-def
  container_name: newnode
  ports:
    - 18545:8545/tcp
    - 18546:8546/tcp
    - 30303
    - 9545
  environment:
    - GOQUORUM_GENESIS_MODE=standard
    - PRIVATE_CONFIG=ignore
  volumes:
    - ./config/goquorum:/quorum
    - ./config/nodes/newnode:/config/keys
    - ./logs/quorum:/var/log/quorum/
  networks:
    quorum-dev-quickstart:
      ipv4_address: 172.16.239.41
```

:::caution

Select an IP address and port map that aren't being used for the other containers. Additionally mount the newly created folder `./config/nodes/newnode` to the `/config/keys` directory of the new node.

:::

### 4. Update files with the enode address

Add the new node's enode address to the [static nodes] file and [permissions file].

The enode uses the format `enode://pubkey@ip_address:30303?discport=0&raftport=53000`. where raftport is only required for the [Raft](../../configure-and-manage/configure/consensus-protocols/raft.md) consensus algorithm

If the `nodekey.pub` is `4540ea...9c1d78` and the IP address is `172.16.239.41`, then the enode address would be `"enode://4540ea...9c1d78@172.16.239.41:30303?discport=0&raftport=53000"`, which must be added to both files.

#### 5. Start the network

Once complete, start the network up with `./run.sh`

On a live network the process is the same when using local permissions with the `permissioned-nodes.json` file. You don't need to restart the network and subsequent changes to the files are picked up by the servers.

When using the smart contract you can either make changes via a [dapp](https://github.com/ConsenSys/permissioning-smart-contracts) or via RPC [API](../../reference/api-methods.md) calls.

[bootnodes]: ../create-permissioned-network.md#2-setup-bootnode
[permissions file]: https://github.com/ConsenSys/quorum-dev-quickstart/blob/master/files/goquorum/config/goquorum/data/permissioned-nodes.json
[static nodes]: https://github.com/ConsenSys/quorum-dev-quickstart/blob/master/files/goquorum/config/goquorum/data/static-nodes.json
