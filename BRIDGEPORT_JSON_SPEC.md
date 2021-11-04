# `bridgeport.json`

This document is about the `bridgeport.json` file that you must include in the root directory of your Bridge repository in order to declare your bridge and define its functionality. This is the "main coniguration file" for Bridgeport.

The fields in the JSON file have the following meanings:

## `bridgeport`

This specifies the version of the Bridgeport JSON spec you are using. The current version is `0.2.0`.

## `id`

This is a BSV address that identifies the Bridge and distinguishes it from other Bridges. The private key to this address is what controls the distribution and hosting of the bridge on Bridgeport.

Proving posession of the private key an help provide evidence that you own and are the creator of a particular bridge. You should safeguard your Bridge Private Key.

In the future, it will be possible to sign the code that runs your Bridge with your Bridge Private Key, and use this signature for various purposes.

## `name`

A human-readable name for the bridge. This should not exceed 50 characters and should relate to the purpose and content of the Bridge. It will be shown on the homepage of any Bridgeport nodes that host your bridge, such as the GitHub Bridgeport Deployment app.

## `description`

A one-line human-readable description for your bridge. The max is 280 characters, and no newline characters are allowed.

## `version`

This is a version like the `version` field in JavaScript `package.json` files.

## `reader`

This is an object containing configuration related to the Bridge Reader.

The reader should create an HTTP server that handles requests to the database.

You will receive `MONGODB_READ_CREDS` and `MONGODB_DATABASE` to perform read-only operations from your database, and serve data to requesters.

When `reader.runtime` is `nodejs14`, the reader directory must contain `package.json` with a `main` field pointing to a JavaScript file that creates an HTTP server. See the example bridge repositories for more guidence.

## `reader.runtime`

This will be used to define the runtime environment of your frontend, once multiple runtimes are supported. Currently, it must be `nodejs14`

## `reader.directory`

Specify a path relative to the `bridgeport.json` file where the Bridge Reader is kept. You are not allowed to use a parent directory and it must be a directory within the same folder (or a subfolder). If not provided, the default is `./reader`.

## `transformer`

This is an object containing configuration related to the Bridge Transformer.

The transformer contains an `index.js` fils with `exports.transformer`, which is a serverless function.

The function HTTP payload contains `action` and `payload`.

You will receive `MONGODB_READWRITE_CREDS` and `MONGODB_DATABASE` to transform the Bitcoin transactions you receive into your database.

When `action` is `process`, take `payload` and add it to your database.

When `action` is `rollback`, take `payload` and remove it from your database.

## `transformer.directory`

Specify a path relative to the `bridgeport.json` file where the Bridge Transformer is kept. You are not allowed to use a parent directory and it must be a directory within the same folder (or a subfolder). If not provided, the default is `./transformer`.

## `transformer.runtime`

This will be used to define the runtime environment of your backend, once multiple runtimes are supported. Currently, it must be `nodejs14`

## `filter`

The filter is how you will specify the kinds of Bitcoin transactions you are interested in.

Note that you may rarely receive transactions that don't follow your filter, if they are accidentally submitted to a node by a user, so validate them in your transformer before processing.

## `filter.startingBlockHeight`

The block number when your protocol was created.

## `filter.find`

This is the Bitbus/Bitsocket "find" block.
