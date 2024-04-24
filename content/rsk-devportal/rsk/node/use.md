---

menu_title: Use
menu_order: 3
layout: rsk
title: Using RSK Public Nodes (Mainnet & Testnet) | Rootstock (RSK)
tags: rsk, networks, versions, rpc, mainnet, testnet, cUrl
description: "Rootstock Nodes: Public nodes (Mainnet, Testnet), Versioning, RPC Methods, and cUrl example"
---

## Public Nodes

RootstockLabs currently provides two public nodes that you can use
for testing purposes, and you will find that information below.

Alternatively, follow the [installation instructions](/rsk/node/install/),
to run your own Rootstock node or use an [alternative node provider](#rpc-node-providers).
This is highly recommended for production environments,
and in accordance with the bitcoiners' maxim: **Don't trust. Verify.**


### Testnet

```
https://public-node.testnet.rsk.co
```

### Mainnet

```
https://public-node.rsk.co
```

## Supported RPC methods

List of supported JSON-RPC methods for each module can be found in the [JSON-RPC documentation](/rsk/node/architecture/json-rpc/).

### Example using `cURL`

Here's an example request using `cURL` to get the Mainnet block number:

`"Content-Type: application/json"`

```shell
curl https://public-node.rsk.co \
    -X POST -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

## RPC Node Providers

> Note that [Rootstock public nodes](/rsk/node/architecture/json-rpc/)
> do not expose WebSockets, they are HTTP only.
> To work around this, you may either run your own Rootstock node,
> or use a third-party node provider, such as [Getblock](/solutions/getblock/) or [NowNodes](/solutions/nownodes/).
