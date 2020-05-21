---
layout: post
title: Building EC2 Instance
summary: Quick reference summary of how I've built my EC2 Instances.
author: jon_maloney
---

Getting Started with Ethereum Part 1
-----------------------------

### Original Article Published [here](https://medium.com/coinmonks/getting-started-with-ethereum-blockchain-development-part-1-d6543b441bea)

## Node environment
Node installer is available [here](https://nodejs.org/en/download/). You can also use package manager like brew to install node.

```bash
brew install node@8
```
## [Solidity](https://solidity.readthedocs.io/en/v0.4.21/installing-solidity.html)
Smart contract are compiled by Solc compiler. There are different ways to install Solc compiler. You can also use npm to install it.

```bash
npm install -g solc
```

## Go Ethereum 
Go Ethereum which is referred as geth is command line interface to run ethereum node implemented in Go. You can install geth from [here](https://geth.ethereum.org/downloads/).

## [Truffle framework](https://www.trufflesuite.com/)
Truffle is development environment and test framework for ethereum. You can install truffle using npm.

```bash
npm install -g truffle
```

##[Ganache](https://www.trufflesuite.com/ganache)
What Ganache does is simple, it creates a virtual Ethereum blockchain, and it generates some fake accounts that we will use during development.

```bash
npm install -g ganache-cli
```

### Write Smart Contract: 

#### 1. Add truffle config file: 
Add below config to truffle.js. In below config, we are declaring development network, which will connect ethereum node running on localhost and port 8545.

```js
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 8545,
      network_id: "*" // Match any network id
    }
  }
};
```

#### 2. Creating a contract: 
Lets create a simple contract ‘counter.sol’ file in contract folder.

I. Events:

What are events in Solidity: Events are dispatched signals that smart contracts can fire. DApps, or anything connected to Ethereum JSON-RPC API(like node js application), can listen to these events and act accordingly.

Counter contract defines two events-
    - CounterIncrementedEvent
    - CounterDecrementedEvent

II. Variable: Like other programming languages, variable is something that holds data.

III. Methods: Counter contract defines three methods to increment, decrement and get counter value.

```js
pragma solidity ^0.4.23;

contract Counter {
    
    event CounterIncrementedEvent(int count);
    event CounterDecrementedEvent(int count);
    int private count = 0;
    function incrementCounter() public {
        count += 1;
        emit CounterIncrementedEvent(count);
    }
    function decrementCounter() public {
        count -= 1;
        emit CounterDecrementedEvent(count);
    }
    function getCount() public constant returns (int) {
        return count;
    }
}
```

#### 3. Deployment Script
Add deployment script named as ‘2_initial_migration.js’ for Counter.sol

```js
var Counter = artifacts.require("./Counter.sol");
module.exports = function(deployer) {
  deployer.deploy(Counter);
};
```

#### 4. Write Truffle Test cases
Create test file named as counter_test.js for Counter contract in test folder.

```js 
const Counter = artifacts.require("Counter");
contract('Counter test', async (accounts) => {
  let instance;
  before(async () => {
    instance = await Counter.deployed();//deploy contract
  });
  it("Initial value of counter should be zero", async () => {
    let count = await instance.getCount.call({from: accounts[0]});
    assert.equal(count, 0);
  });
});
```

Run ganache-cli: Run below command to run ganache
Ganache-cli
Run truffle tests: In separate Window, run truffle test cases using below command.
truffle test
Yayy!!! You have deployed and tested your first smart contract.

#### 6. Add more tests: 

##### i. Add test for Increment Counter: 

```js
it("Should increment counter", async () => {
  await instance.incrementCounter({from: accounts[0]});
  let count = await instance.getCount.call({from: accounts[0]});
  assert.equal(count, 1);
});
```

##### ii. Add test to verify if event is emitted on Incremented Counter

```js
it("Should emit event on increment counter", async () => {
  let reciept = await instance.incrementCounter({from: accounts[0]});
  assert.equal(reciept.logs.length, 1);
  assert.equal(reciept.logs[0].args.count, 2);
});
```

Now you can try adding tests for Decrement Counter :)

Full source code is available on Github: [sarveshgs/firstBlockchainApp](https://github.com/sarveshgs/firstBlockchainApp)