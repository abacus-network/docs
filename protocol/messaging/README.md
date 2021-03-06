---
description: Send and receive messages
---

# Messaging API

The primary purpose of the Abacus protocol is to facilitate the passing of messages from an address on one chain to an address on another chain.

Applications send and receive cross-chain messages using the [`Outbox`](outbox.md) and [`Inbox`](inbox.md) smart contracts.&#x20;

## Mailboxes

The [`Outbox`](outbox.md) and [`Inbox`](inbox.md) contracts allow application developers to send and receive cross-chain messages.

There is one `Outbox` on every Abacus-supported chain. Applications send messages to other chains by calling `Outbox.dispatch()`. These messages are inserted into the `Outbox's` [merkle tree](https://en.wikipedia.org/wiki/Merkle\_tree).

There are `n-1` `Inboxes` on every Abacus-supported chain, one for every other chain. Applications receive messages via `handle()`, a function that gets called by `Inbox` contracts whenever interchain messages are being delivered.

The [Abacus validator se](../agents/validators.md)[t](../agents/validators.md) for each chain is responsible for signing the latest `Outbox` merkle root on that chain. Messages can be delivered to remote `Inboxes` by providing merkle proofs against this signed root.

## Lifecycle

Sending and receiving a cross-chain message consists of three steps.

1. An application calls [`Outbox.dispatch()`](outbox.md#dispatch)on the origin chain, inserting the message into the `Outbox's` merkle tree.
2. The validator set for the origin chain signs the new merkle root. If specified, the message recipient's sovereign validators also sign the new merkle root.&#x20;
3. A relayer delivers the message by calling `InboxValidatorManager.process()`, providing a mekle proof of the message against the signed root from step 2. `InboxValidatorManager` will then call `Inbox.process` which verifies the merkle proof and eventually will call `handle` on the message recipient.

![](<../../.gitbook/assets/Untitled Diagram.drawio (2).svg>)
