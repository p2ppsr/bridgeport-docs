# Bridgeport Docs

Create Bitcoin-powered backends

## Getting Started

Users create Bitcoin transactions that follow you rules in order to interact with your bridges. The transactions are submitted to Bridgeport by the users in real-time for processing.

1. You write a filter that determines what kinds of Bitcoin transactions your bridge wants to know about. This is called a **Brige Filter**.

2. You write code that *transforms* these transactions into database commands. This code is called a **Bridge Transformer**.

3. You also write code allowing people to *read* data from that database over the internet. This code is called a **Bridge Reader**.

If you do not provide a **Bridge Filter**, all Bitcoin transactions will be sent through your **Bridge Transformer** from the Genesis block to the present.

If you do not provide a **Bridge Transformer**, the default transformer will insert all transactions into the MongoDB database, and emit a new live event for each new transaction.

If you do not provide a **Bridge Reader**, the default reader will allow anyone to write open-ended MongoDB queries against the bridge database, and listen for live events.

Thus, the empty bridge is the most versitile and open-ended, but also the most expensive to run. You will be billed:

- For each transaction that passes your **Bridge Filter**
- For your **Bridge Transformer**'s allocation of its databases and other computational resources
- Based on how many requests and read operations are made by your **Bridge Reader**

You can interpret the Bitcoin transactions in any way you would like. You can deal with encrypted data so that everything does not have to be public. Because the order of confirmed transactions in Bitcoin is immutable, anyone who has a copy of your **Bridge Transformer** can spin up an identical copy of your database, audit your records or deploy a completely synchronized mirror of the API, allowing it to achieve infinite scale.

Your APIs are no longer identified by the servers on which they are hosted, but instead by their **Bridge ID**. Servers can easily replicate and host a copy of the API with a given **Bridge ID**, allowing automatic protocol-level load balancing and distribution robustness against central points of failure. Since they are all kept in sync by the blockchain[1], it should not matter which physical server clients are connecting with.

## 1. Set Up Repo and Billing

You'll start by creating a GitHub repository for your new Bridge. You can fork the Starter Repo(TODO: link), then you'll want to install the [Bridgeport Deployment GitHub App](TODO: link) and head over to the Bridgeport Dashboard(TODO: link).

To link your Bridge repo for auto-deployment, start by going to "New Bridge from GitHub" and paste in the link to your repo. **This will work even if your repo is private, as long as the Bridgeport Deployment app is installed.** No need for a public repo.

Select "Generate New Bridge Private Key" and save the key to your computer, as it is important. Then, follow the instructions for updating `bridgeport.json` to authenticate ownership over the Bridge repo. Push the changes to your production branch (`master` by default), and click "Verify". 

Deposit some money into Bridgeport Dashboard to pay for the hosting of your bridges, then you will be ready to start working! Whenever you push to your configured production branch, your bridge will be updated live on Bridgeport.

## 2. Writing Bridge Transformers

Bridgeport Bridge Transformers transform Bitcoin transactions into database operations. To get started, learn how to [write your first Bridge Transformer](TRANSFORMERS.md).

## 3. Writing Bridge Readers

Once you've written a Bridge Transformer for your use-case, it's time to make it useful with an API. Head over to the [Bridge Readers Tutorial](READERS.md) and create yourself a Bridge object!

## Documentation for `bridgeport.json`

The `bridgeport.json` file in the root of your repository is where you specify information about your Bridge. You can [read the docs here](BRIDGEPORT_JSON_SPEC.md).

## Using Bridgeport

Once you have created a Bridge, and once it has been hosted by a Bridgeport node (such as by using the Bridgeport Deployment GitHub app), the Bridgeport node will broadcast a commitment transaction over [BHRP (Bridgeport Host Resolution Protocol)](https://bridgeport.babbage.systems/1TW5ogeDZyvq5q7tEjpNcBmJWsmQk7AeQ), publicly committing to host your bridge at a URL for as long as you pay them.

You can use [`parapet-js`](https://github.com/p2ppsr/parapet) to run queries, listen to sockets and employ custom routes defined in your Bridge. Just provide your Bridge ID, and parapet will use BHRP to resolve a competent host automatically. No more servers!

## Examples

To help you understand how Bridgeport works, we have created a few example bridges. You can refer to these repos for real-world uses of Bridgeport:

- **[Convo User Messages Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)**: A bridge that tracks the private messages exchanged between users of the on-chain Convo Messenger app
- **[Convo User Profiles Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)**: A bridge that tracks the user profiles of users of the on-chain Convo Messenger app

Finally, to see how Parapet is used to run queries against these Bitcoin-powered backends, refer to the [main Convo codebase](https://github.com/p2ppsr/convo) (which, by the way, is also an app built with the [Babbage App Engine](https://projectbabbage.com)).

Feel free to reach out to **Ty Everett** if you would like help understanding the system or building bridges :)

## Bridgeport Node API

For interactions with the Bridgeport Node API itself, check the [documentation](BRIDGEPORT_NODE_API.md).

## Inspiration

This system was inspired by and adapted from the old, apparently "left for dead" Planaria ideas of a pseudonymous online entity known to us *only* as _unwriter. Credit is given for publishing the proof of concept under the MIT license.

## Copyright

The materials in this repository are copyright Peer-to-peer Privacy Systems Research, LLC. All rights reserved. Do not re-distribute!
