---
description: The skeleton of an Abacus-connected contract and app
---

# HelloWorld

The [abacus-app-template repo](https://github.com/abacus-network/abacus-app-template) shows the basic skeleton of an Abacus app.

#### Contract

Its [contract](https://github.com/abacus-network/abacus-app-template/blob/main/contracts/HelloWorld.sol) sends a user-specified string to another chain which handles the message by increasing counters and emitting events.&#x20;

To conveniently implement the router pattern, the contract extends `@abacus-network/app/contracts/Router.sol`

To send the message, it calls the `_dispatchWithGas` method.

#### Deployer

The [deployer](https://github.com/abacus-network/abacus-app-template/blob/main/src/deploy/deploy.ts) is configured to deploy to local hardhat-based test networks.

The main purpose of defining a deployer is to provide the custom types and and implementation of the `deployContracts` method. See [Deploying contracts](../deploying-contracts/) for more details.

```typescript
export class HelloWorldDeployer<
  Chain extends ChainName,
> extends AbacusRouterDeployer<
  Chain,
  HelloWorldContracts,
  HelloWorldFactories,
  HelloWorldConfig
> 
```

#### Application

The [application](https://github.com/abacus-network/abacus-app-template/blob/main/src/sdk/app.ts#L12) fetches some basic statistics and returns them.

The `sendHelloWorld` method shows an example of triggering the remote message dispatch:

```typescript
const tx = await sender.sendHelloWorld(
  toDomain,
  message,
  chainConnection.overrides,
);
```
