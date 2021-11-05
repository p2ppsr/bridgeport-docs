# Bridgeport Node API

Interact with the Bridges on a single Bridgeport Node

Note that you must **never** hard-code the URL of a Bridgeport node directly in your application. Instead, use BHRP to search for and resolve URLs for nodes that host the bridges you need every time. For how this works, check out the [Parapet reference client](https://github.com/p2ppsr/parapet).

Once you have used BHRP to resolve the URL for a Bridgeport node that advertises it hosts the bridge you need, you can reference these docs for how to talk to the Bridgeport node over HTTPS.

Each Bridgeport Node hosts an identical API. The only difference is which Bridges it makes available, based on what it has committed to hosting. Commitments are advertised over certain timespans by the node via BHRP.

## API Documentation

The Bridgeport Node itself exposes a simple API, with the following three endpoints:

### GET `/`

Web interface where you can see a list of bridges hosted by this Bridgeport node

### GET `/bridges`

Returns an array of objects, each containing information about one of the bridges hosted by this Bridgeport node

Each object contains `name`, `description`, `id` and `version`.

### POST `/processTransaction`

Allows you to tell the Bridgeport node about a new transaction that you think would be relevant to one or more bridges hosted on this node.

The body of the request should be an Everett-style transaction envelope proving the transaction in the chain. It may be unconfirmed.

An additional `bridges` field should be appended to the envelope, indicating which Bridge ID(s) this transaction is related to.

After validating the transaction and processing it through the relevant Bridge Transformer(s), the Bridgeport node will use BHRP to look up other nodes that host the same bridge(s), and gossip the new transaction across the network to each of them. This keeps all nodes in sync about new transactions.

## Default Reader API

For every bridge, all routes under `/<id>` are associated with and served by the Bridge Reader for that bridge.

Note that `<id>` is the Bridge ID corresponding to the Bridge being queried.

The *default* Bridge Reader will expose the following routes. **However**, this can be customized on a bridge-by-bridge basis.

### `/<id>`

By default, renders an HTML version of the file in `reader/README.md`. This is the bridge "home page".

### `/<id>/q`

HTTP API endpoint for running queries on the MongoDB Bridge database

### `/<id>/s`

SSE socket endpoint where you can receive live events (with change streams) from the MongoDB Bridge database

### `/<id>/query`

Web interface where you can write queries (using `/q`) that interact with the Bridge

### `/<id>/socket`

Web interface where you can listen to the live SSE socket (using `/s`) for the Bridge
