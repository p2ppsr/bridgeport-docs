# Bridges

Exposing Bridgeport State Machines to the Real World

## Overview

A bridge connects as a read-only observer to a MongoDB database that is hosting a State Machine. The bridge then defines configurable HTTPS and SSE endpoints, exposing the data from the State Machine for the world to use.

Bridges are defined as a JavaScript object known as the Bridge object. This object has several properties that allow you to modify and configure the bridge. In the example below, a bridge suitable for use with a State Machine running the Memo protocol is defined:

```js
module.exports = {
    bridgeport: '0.0.1',
    name: 'Memo Posts',
    description: 'Explore the universe of Memo content from the BSV blockchain',
    version: '0.1.0',
    // No custom routes are needed
    routes: [],
    // No transorms are applied
    requestTransformer: x => x,
    responseTransformer: x => x,
    defaultQuery: JSON.stringify({
      v: 3,
      q: {
        collection: 'posts',
        find: {},
        project: { text: 1 },
        limit: 10
      }
    }, null, 2),
    defaultSocket: JSON.stringify({
      v: 3,
      q: {
        find: {}
      }
    }, null, 2),
    readme: `
# Memo Posts

> A collection of all Memo posts on the Bitcoin SV blockchain

## Overview

You can use this Bridge to query for all Memo posts on the BSV blockchain. Use Parapet to get results over HTTPS, as well as to subscribe to live SSE-based updates!`
  }
}
```

You can now create an npm package with your bridge, or put it into a repository so that it can be used by a Bridgeport host to make your State Machine available to the world.

## Bridge Object Documentation

The Bridge object is an object with the following fields:

### `bridgeport`

The version of the bridge object to use for this bridge, currently `0.0.1`

### `name`

Human-readable name to show the users who interact with this bridge

### `description`

Human-readable one-line description for this bridge

### `version`

The version of the bridge that is being served

### `routes`

An array of objects that define custom Express routes for the bridge, where each of the objects has:

- **`method`**. The HTTP method in lowercase, such as `get` or `post`

- **`path`**. The path, relative to the bridge namespace under `/<id>`.

- **`function`**. The route handler function that will take `(req, res)` as parameters.

Note that if you provide custom routes for `q`, `s`, `query` and/or `socket`, they will override the default routes for your bridge. This can be extremely useful when implementing an "opaque bridge", where not all of the data from the MongoDB database is intended to be publicly queryable (perhaps because the state transition logic is not public and the data in the transactions was encrypted).

### `requestTransforms`

A transformer function applied to incoming query requests.

### `responseTransforms`

A transformer function applied to outgoing response objects.

### `defaultQuery`

This is a string that will populate the query interface in the web interface when the query editor is opened.

### `defaultSocket`

This is a string that will populate the live socket interface when the socket editor is opened.

### `readme`

This is a markdown string that will be rendered as HTML to create the homepage for the bridge.
