# Busdriver: Bridgeport State Machines

Redux for Bitcoin

## Overview

Since we rely on [Bitbus](https://bitbus.network), Busdriver "drives" your bus (your Bridgeport State Machine) by 
transforming the global Bitcoin bus into your application-specific bus.

## Pre-requisites

You need to understand [Bitquery and the TXO transaction format](https://bitquery.planaria.network) before you can write effective state machines. All Bitcoin transactions passed into your state machine are in the TXO format, and you need to write a simple Bitquery to use as your bus route.

## Example Usage

For this example, we'll be creating a State Machine that stores all Memo posts on the BSV blockchain.

You should create a `bus.js` file with the bus that you want to drive:

```js
const busdriver = require('@cwi/busdriver')

module.exports = {
  busdriver: '0.0.1',
  id: '1Dev7vE2QiNDxCCiQXb4DZ1eCCVESNB7zU',
  startingBlockHeight: 550000,
  onCreate: async state => {
    console.log('Creating index for posts...')
    await state.db
      .collection('posts')
      .createIndex(
        { h: 1 },
        { session: state.session }
      )
  },
  boardPassenger: async (state, action) => {
    if (action.blk && action.blk.i) {
      console.log('[+]', action.blk.i, action.i)
    } else {
      console.log('[+]', action.timestamp)
    }
    await busdriver.createEvent({
      channel: 'posts',
      payload: action.out[0].s3
    })
    await state.create({
      collection: 'posts',
      data: {
        text: action.out[0].s3,
        h: action.tx.h
      }
    })
  },
  ejectPassenger: async (state, action) => {
    console.log('[-]', action)
    await state.delete({
      collection: 'posts',
      find: {
        h: action
      }
    })
  },
  ejectPassengersWithFullTX: false,
  busRoute: {
    find: {
      'out.s2': '\x6d\x02'
    }
  }
}
```

## Bus Configuration Options

Define your State Machine's logic by providing the following fields:

### `busdriver`

This is the version of Busdriver to use with this bus. It is currently `0.0.1`.

### `id`

Generate a random Bitcoin SV address and private key to use with your bus. Keep the private key safe, because you'll find it valuable in the future. The `id` is your BSV address.

### `startingBlockHeight`

This specifies the block height at which to start sending Bitcoin SV transactions into your bus. If you know that your bus won't ever use or need transactions older than a certain height, you should use this to prevent yourself from wasting time and CPU power while throwing away data you don't need to sync or process.

Currently, transactions older than around block 566000 are not supported. This will change in the future. If this is a blocker for you, email bridgeport@babbage.systems and we will work on a solution.

### `onCreate`

This is a function that is called when your state machine first starts. It can be useful for things like creating MongoDB collections and setting up indexes on them.

You should return a Promise from this (make it `async`), and `await` any database operations you initiate.

This function is passed a State API as its first parameter.

### `boardPassenger`

This is the core of your bus. It is like a reducer in Redux, because it takes `state` and `action` and applies the action to the state.

The first parameter is a State API. The second parameter is the Bitcoin SV transaction in TXO format which is to be applied to your state machine.

All transactions get applied to your state machine in the order that they appear in the blockchain.

### `ejectPassenger`

This is for rolling back state transitions. Think of it as the inverse function of `boardPassenger`, because your goal is to revert the state back to how it was before the given transaction occurred.

The first parameter is a State API, and the second parameter depends on the value of `ejectPassengersWithFullTX`. If it is true, you will get the full TXO transaction. If it is false, you will only get the TXID. Setting it to true is slower, so you should just rely on the TXID if you can.

### `ejectPassengersWithFullTX`

This is a boolean that tells Busdriver whether the `action` passed to `ejectPassenger` needs to be the full transaction in TXO format, or whether just the TXID is OK. If you need the full TX when ejecting, your database will be larger on disk and things will run more slowly. The default is to just provide the TXID.

### `busRoute`

This is the query that you will use to restrict down the amount of transactions you process. This way you don't need to process every single Bitcoin transaction that you don't care about. Provide the "q" block of a Bitquery that will be sent to Bitbus.

## The State API

The State API is how you can mutate the state of your Bitcoin-powered state machine.

### `state.create`

Use this to create Mongodb documents in your state machine. Pass an object with `collection` and `data`, where data can either be a single object or an array of documents to insert. It returns a Promise, which you should always `await`, even if you don't care about the results.

### `state.read`

Use this to read information from state. Pass an object with any o these parameters: `collection`, `aggregate`, `find`, `sort`, `limit`, `skip`, `project`.

This returns a Promise. Always `await` Promises, even if you don't care about the results.

### `state.update`

Use this to update documents in the state machine.

Pass an object with any of `collection`, `find`, `project`, `sort`, `limit`, `skip`, `map`.

Map is a function that takes one of the old records and returns one of the new records.

> `mapAsync` is coming soon, so you can map things asynchronously and use `await` in your map.

This returns a Promise. Always `await` Promises, even if you don't care about the results.

### `state.delete`

Use this to remove documents from your state machine. Pass an object with `collection` and `find`. All matching objects will be deleted.

This return a Promise. Always `await` Promises, even if you don't care about the results.

### `state.db` and `state.session`

We pass `db` and `session` in the state object. If you leverage this, you MUST pass `session` into any of the database operations you run. This insures that they are associated with the MongoDB transactions that Busdriver started for your use.

These operations return Promises. Always `await` Promises, even if you don't care about the results.

## Creating Live Events

Live events can be created that will be served over the Bridgeport live SSE socket. Just use `createEvent` and pass in the data that you want to broadcast to the socket. Check out the example code above for a simple use-case.

## Implementation Notes

We pass `db` and `session` in the state object. If you leverage this, you MUST pass `session` into any of the database operations you run. This insures that they are associated with the MongoDB transactions that Busdriver started for your use.

All collections in your database that begin with `busdriver_` are reserved for Busdriver's use. You are free to store data in whichever other collections you wish.

Having a live event document that has fields that begin with `_busdriver` is not allowed, because Busdriver uses these fields internally to process the live events.

Finally, note the proprietary dependency on `@cwi/busdriver`. Until the Bridgeport/Busdriver ecosystem sufficiently matures, all Bridgeport state machines are powered by and hosted with proprietary software. For now, please contact bridgeport-hosting@babbage.systems if you have created a Bridgeport bridge and state machine, and would like to host it on our servers. See the **Hosting** section of [the README](README.md) for more details.
