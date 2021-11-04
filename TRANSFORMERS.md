# Bridge Transformers

Redux for Bitcoin

## Overview

Since we (*currently*) rely on [Bitbus](https://bitbus.network), The Bridgeport Busdriver "drives" your Bridge Transformer by 
transforming the global Bitcoin bus into your application-specific bus with your Bridge Filter.

## Pre-requisites

You need to understand [Bitquery and the TXO transaction format](https://bitquery.planaria.network) before you can write effective Bridge Filters and Bridge Transformers. All Bitcoin transactions passed into your transformer are in the TXO format, and you need to write a simple Bitquery to use as your Bridge Filter.

## Example Usage

You should check out the [Hello World Bridge Transformer](https://github.com/p2ppsr/hello-world-protocol/tree/master/transformer) for an example.

## The State API

The State API is how you can mutate the state of your Bitcoin-powered state machine.

### `state.create`

Use this to create Mongodb documents in your state machine. Pass an object with `collection` and `data`, where data can either be a single object or an array of documents to insert. It returns a Promise, which you should always `await`, even if you don't care about the results.

### `state.read`

Use this to read information from state. Pass an object with any of these parameters: `collection`, `aggregate`, `find`, `sort`, `limit`, `skip`, `project`.

This returns a Promise. Always `await` Promises, even if you don't care about the results.

### `state.update`

Use this to update documents in the state machine.

Pass an object with any of `collection`, `find`, `project`, `sort`, `limit`, `skip`, `map`.

Map is a function that takes one of the old records and returns one of the new records.

> `mapAsync` is coming soon, so you can map things asynchronously and use `await` in your map.

This returns a Promise. Always `await` Promises, even if you don't care about the results.

### `state.delete`

Use this to remove documents from your state machine. Pass an object with `collection` and `find`. All matching objects will be deleted.

This returns a Promise. Always `await` Promises, even if you don't care about the results.

### `state.db` and `state.session`

We pass `db` and `session` in the state object. If you leverage this, you MUST pass `session` into any of the database operations you run. This insures that they are associated with the MongoDB transactions that Busdriver started for your use.

These operations return Promises. Always `await` Promises, even if you don't care about the results.

## Creating Live Events

Live events are coming very soon to Bridgeport 2.0. Please bug Ty for this if it is causing you pain.

## Implementation Notes

We pass `db` and `session` in the state object. If you leverage this, you MUST pass `session` into any of the database operations you run. This insures that they are associated with the MongoDB transactions that is associated with this current Bitcoin transaction.
