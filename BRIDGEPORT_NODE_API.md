# Bridgeport Node API

Interact with the Bridges on a single Bridgeport Node

Note that you must **never** hard-code the URL of a Bridgeport node directly in your application. Instead, use BHRP to search for and resolve URLs for nodes that host the bridges you need every time. For how this works, check out the [Parapet reference client](https://github.com/p2ppsr/parapet).

Once you have used BHRP to resolve the URL for a Bridgeport node that advertises it hosts the bridge you need, you can reference these docs for how to talk to the Bridgeport node over HTTPS.

Each Bridgeport Node hosts an identical API. The only difference is which Bridges it makes available, based on what it has committed to hosting. Commitments are advertised over certain timespans by the node via BHRP.

## API Documentation

The Bridgeport Node itself exposes a simple API, with the following two endpoints:

### GET `/`

Web interface where you can see a list of bridges hosted by this Bridgeport node

### GET `/bridges`

Returns an array of objects, each containing information about one of the bridges hosted by this Bridgeport node

The object contains `name`, `description`, `id` and `version`.

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
