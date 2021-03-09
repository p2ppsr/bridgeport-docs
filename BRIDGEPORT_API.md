# Bridgeport API

Interact with Bridges and read data from State Machines

## Overview

Each Bridgeport node hosts an identical API. The only difference is which Bridges it makes available, based on the State Machines that it has committed to hosting.

## API Documentation

Bridgeport exposes a simple API, with the following endpoints:

Note that `<id>` is the State Machine ID corresponding to the Bridge being queried.

### `/`

Web interface where you can see a list of bridges hosted by this Bridgeport node

### `/bridges`

Returns a list of State Machine IDs for the Bridges hosted by this Bridgeport node, as a JSON array

### `/<id>`

Renders the `readme` field for the Bridge with `<id>` as an HTML page

### `/<id>/...`

Your custom routes, as defined in the `routes` array, will be rendered relative to this path. For example, a route with `path: "posts"` would render at `/<id>/posts`. Do not prefix your routes with `/`.

### `/<id>/query`

Web interface where you can write queries that interact with the Bridge with `<id>`

### `/<id>/socket`

Web interface where you can listen to the live SSE socket for the Bridge with `<id>`

### `/<id>/q`

HTTP API endpoint for running queries on the Bridge with `<id>`

### `/<id>/s`

SSE socket endpoint where you can receive events from the Bridge with `<id>`

## Reference Client

To see the `/q` and `/s` endpoints in action, check out the code for [`parapet-js`](https://github.com/p2ppsr/parapet).
