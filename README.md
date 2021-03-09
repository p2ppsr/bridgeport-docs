# Bridgeport Docs

Learn how to create Bitcoin-powered backends

## Overview

People create Bitcoin transactions to interact with your backend. You write code that turns those Bitcoin transactions into MongoDB transactions, performing operations on a database. Then you write code allowing people to read from that database over the internet.

You can model any finite state machine in your database, since it's just mongoDB. You can interpret the Bitcoin transactions in any way. You can deal with encrypted data so that not everything is public. And because the order of transactions in Bitcoin is immutable, anyone who has a copy of your State Machine's state transition logic can spin up a copy, audit your records or deploy a completely synchronized mirror of your API, allowing it to achieve infinite scale.

Your APIs are no longer identiied by the servers on which they are hosted, but instead by the State Machine ID associated with their state transition logic. Any server can easily replicate and host a copy of the API with a given State Machine ID, allowing automatic, protocol-level load balancing and distribution robustness against central points of failure.

## Writing State Machines

Bridgeport State Machines (sometimes referre to as "busses") transform Bitcoin transactions into MongoDB transactions that update your State Machine database. To get started, learn how to [write your first Bridgeport State Machine](STATE_MACHINES.md).

## Writing Bridges

Once you've written a State Machine for your use-case, it's time to make it useful with an API. Head over to the [Bridges Tutorial](BRIDGES.md) and create yourself a Bridge object!

## Hosting

In the future, we intend to create a hosting service that will allow you to make all your State Machines and Bridges available. We may also create a licensing program to allow enterprise customers to self-host their own Bridgeport nodes and software.

For now, once you have written a State Machine and Bridge that you would like to host, please reach out to bridgeport-hosting@babbage.systems. We will host it on our Bridgeport node at [bridgeport.babbage.systems](https://bridgeport.babbage.systems).

## Using Bridgeport

Once you have written a State Machine and a Bridge, and once it has been hosted by a Bridgeport node, the Bridgeport node will broadcast a commitment transaction over BHRP (Bridgeport Host Resolution Protocol), publicly committing to host your bridge at a URL for a certain length of time.

You can use [`parapet-js`](https://github.com/p2ppsr/parapet) to run queries, listen to sockets and employ custom routes defined in your Bridge. Just provide your State Machine ID, and parapet will use BHRP to resolve a competent host automatically.

## Examples

To help you understand how Bridgeport works, we have created a few example Bridgeport State Machines as part of the Convo Messenger project. You can refer to these repos for real-world uses of Bridgeport:

- [Convo User Messages Protocol State Machine](https://github.com/p2ppsr/convo-cump-bus)
- [Convo User Messages Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)
- [Convo User Profiles Protocol State Machine](https://github.com/p2ppsr/convo-cupp-bus)
- [Convo User Profiles Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)

Finally, to see how Parapet is used to run queries against these Bitcoin-powered backends, refer to the [main Convo codebase](https://github.com/p2ppsr/convo) (which, by the way, is also a [Rubeus](https://rubeus.app) app).

## Bridgeport API

For custom interactions with the Bridgeport API itself, check the [documentation](BRIDGEPORT_API.md).

## Inspiration

This system, and the code that runs it, was inspired by and adapted from the old Planaria ideas of a pseudonymous online entity known to us only as "_unwriter". Credit is given for publishing the proof of concept under the MIT license.

## Copyright

The materials in this repository are copyright Peer-to-peer Privacy Systems Research, LLC. All rights reserved. Do not re-distribute!
