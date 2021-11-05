# Bridgeport Docs

Create Bitcoin-powered backends

## Overview

Users create Bitcoin transactions that follow your rules in order to interact with your **Bridges**.

The relevant transactions are submitted by your users to **Bridgeport Nodes** that host your bridge in real-time for processing.

1. You write a filter that determines what kinds of Bitcoin transactions your bridge wants to know about. This is called a **Bridge Filter**.

> If you do not provide a **Bridge Filter**, the default filter allows all Bitcoin transactions to enter your **Bridge Transformer**, from the Genesis block to the present.

2. You write code that *transforms* these transactions into database commands. This code is called a **Bridge Transformer**.

> If you do not provide a **Bridge Transformer**, the default transformer will store all transactions in your bridge database.

3. You also write code allowing people to *read* data from that database over the internet. This code is called a **Bridge Reader**.

> If you do not provide a **Bridge Reader**, the default reader will allow anyone to write open-ended queries against your bridge database.

Thus, the empty bridge is the most versitile and open-ended, but also the most expensive to run. ***Expect to be billed:***

- For each transaction that passes your **Bridge Filter**
- For your **Bridge Transformer**'s allocation of its databases and other resources
- Based on how many requests and read operations are made by your **Bridge Reader**

You can interpret the Bitcoin transactions in any way you would like. You can deal with encrypted data so that everything does not have to be public. Because the order of confirmed transactions in Bitcoin is immutable, anyone who has a copy of your **Bridge Repo** can spin up an identical copy of your bridge, audit your records or deploy a completely synchronized mirror of the API, allowing it to achieve infinite scale.

Your APIs are no longer identified by the servers on which they are hosted, but instead by their **Bridge ID**. Servers can easily replicate and host a copy of the API with a given **Bridge ID**, allowing automatic protocol-level load balancing. The distributed nature of the system makes it robust against central points of failure. Since all Bridgeport nodes are kept in sync by the ordering of transactions in the blockchain, it should never matter which server the clients are talking to.

## 1. Set Up Repo and Billing

You'll start by creating a GitHub repository for your new Bridge. You can fork the [Starter Repo](https://github.com/p2ppsr/hello-world-protocol), then you'll want to install the [Bridgeport Deployment GitHub App](https://github.com/apps/bridgeport-deployment/installations/new) and follow the instructions in the [Bridgeport Dashboard](https://bridgeport-dashboard.babbage.systems).

To link your Bridge repo for auto-deployment, start by going to "Create Bridge" and paste in the link to your repo. **This will work even if your repo is private, as long as the Bridgeport Deployment app is installed.** No need for a public repo.

Select "Generate New Bridge Private Key" and save the key to your computer, as it is important. Then, follow the instructions for updating `bridgeport.json` to authenticate ownership over the Bridge repo. Push the changes to your master branch, and view your new deployment from the Deployments page. 

Deposit some money into Bridgeport Dashboard to pay for the hosting of your bridges, then you will be ready to start working! Whenever you push to your master branch, your bridge will be updated live onto our Bridgeport node.

You can view your logs, change settings and add environment variables to bridges in the Bridgeport Dashboard.

## 2. Understand  `bridgeport.json`

The `bridgeport.json` file in the root of your **Bridge Repo** is where you specify information about your Bridge. You should [read the docs here](BRIDGEPORT_JSON_SPEC.md).

## 3. Write a Bridge Transformer

A **Bridge Transformer** will transform Bitcoin transactions into database operations. To get started, learn how to [write your first Bridge Transformer](TRANSFORMERS.md).

## 4. Write a Bridge Reader

Finally, it's time to make your bridge accessible to the world with an API. Head over to the [Bridge Readers Tutorial](READERS.md) to learn how!

## 5. Use Your New Bridge

Once you have created a Bridge, and once it has been hosted by a Bridgeport node (such as by using the Bridgeport Deployment GitHub app), the Bridgeport node will broadcast a commitment transaction over [BHRP (Bridgeport Host Resolution Protocol)](https://bridgeport.babbage.systems/1TW5ogeDZyvq5q7tEjpNcBmJWsmQk7AeQ), publicly committing to host your bridge at a URL for as long as you pay them.

You can use [`parapet-js`](https://github.com/p2ppsr/parapet) to run queries, listen to sockets and employ custom routes defined in your Bridge. Just provide your Bridge ID, and parapet will use BHRP to resolve a competent host automatically. No more servers!

## Examples

To help you understand how Bridgeport works, we have created a few example bridges. You can refer to these repos for real-world uses of Bridgeport:

- **[Convo User Messages Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)**: A bridge that tracks the private messages exchanged between users of the on-chain Convo Messenger app
- **[Convo User Profiles Protocol Bridge](https://github.com/p2ppsr/convo-cump-bridge)**: A bridge that tracks the user profiles of users of the on-chain Convo Messenger app
- **[Hello World Protocol Bridge](https://github.com/p2ppsr/hello-world-protocol)**: This is the simple example bridge you can fork to start your own

Finally, to see how Parapet is used to run queries against these Bitcoin-powered backends, refer to the [main Convo codebase](https://github.com/p2ppsr/convo) (which, by the way, is also an app built with the [Babbage App Engine](https://projectbabbage.com)).

Feel free to reach out to [Ty Everett](mailto:ty@projectbabbage.com) if you would like help understanding the system or building bridges :)

## Bridgeport Node API

For interactions with the Bridgeport Node API itself, check the [documentation](BRIDGEPORT_NODE_API.md).

## Inspiration

This system was inspired by and adapted from the old, apparently "left for dead" Planaria ideas of a pseudonymous online entity known to us only as *_unwriter*. Credit is given for publishing the proof of concept under the MIT license.

No fear of Big Blocks!

## Copyright

All materials in this repository are copyright © 2021 Peer-to-peer Privacy Systems Research, LLC. All rights reserved. Do not re-distribute!
