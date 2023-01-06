# Solution to Challenger #6 Alchemy University

Objetives:

In this tutorial we'll learn the following building blocks for staking:

1.  Building with Scaffold-Eth
    - Hacking together frontends
    - Crafting Solidity "backends"
2.  Transferring ETH from wallets to smart contracts & vice versa
3.  Utilizing Solidity modifiers

Let’s start by understanding how Scaffold-Eth works!

## Beginning 🚀

_These instructions will allow you to get a copy of the project running on your local machine for
development and testing purposes._

See **Deployment** for how to deploy the project.

### Prerequisites 📋

_Download Scaffold-Eth_

In this tutorial, we're going to use the Scaffold-Eth developer environment to craft our smart contracts 
and put together our frontend UI.

    📘

    Before jumping in, I want to convey a few important details to keep in mind!

    Scaffold-Eth is awesome at abstracting away environment set-ups and frontend dependencies which makes 
    it a powerful tool.

    While there are lots of functionalities that are handled by Scaffold-Eth automatically, it is important 
    to dive into the code to understand how some of these features are generated when you have a more solid 
    grasp of the developer environment as a whole.

Let's begin by forking the base code repository from [Challenge #1](https://speedrunethereum.com/challenge/decentralized-staking) of [SpeedRunEthereum](https://speedrunethereum.com/):


```
git clone https://github.com/scaffold-eth/scaffold-eth-challenges.git 6-StakingApp

cd 6-StakingApp

git checkout challenge-1-decentralized-staking

yarn install
```

If you've followed along successfully, you'll be able to a new folder titled 6-StakingApp in your base 
file directory.

After running the commands above, we're left with a large folder full of files.

Even before moving on to the code, we should familiarize ourselves with where key files are stored in 
Scaffold-Eth so we know where to focus on.

![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un título")

In this tutorial, we'll be primarily working on Staker.sol and App.jsx. 


### Instalation 🔧

Next, you'll need three separate terminals up for the three following commands:

Start your React frontend:

```
yarn start
```

You'll to see:

```
Compiled successfully!

You can now view @scaffold-eth/react-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.125.128:3000

Note that the development build is not optimized.
To create a production build, use npm run build.
```

Start your Hardhat backend:

```
yarn chain
```

You'll to see:

```
llabori@Xubuntu64Bits-virtual-machine:~/BlockChains/AlchemyUniversity/6-StakingApp$ yarn chain
yarn run v1.22.19
$ yarn workspace @scaffold-eth/hardhat chain
$ hardhat node --network hardhat --no-deploy
Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/

web3_clientVersion
eth_chainId
eth_accounts
eth_chainId
eth_accounts
eth_chainId
eth_estimateGas
eth_chainId
eth_getTransactionCount
eth_blockNumber
eth_chainId
eth_feeHistory
eth_sendTransaction
  Contract deployment: <UnrecognizedContract>
  Contract address:    0x5fbdb2315678afecb367f032d93f642f64180aa3
  Transaction:         0x969048d3d9d28fe79e4acc6c4a78e29e962d37301144c6ab104af437fd7915bf
  From:                0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
  Value:               0 ETH
  Gas used:            87965 of 87965
  Block #1:            0xc0ae22a97dfc48578cd35a7367af67492a0aaafa3670817cd37b6fd0b73df901

eth_chainId
eth_getTransactionByHash
eth_chainId
eth_getTransactionReceipt
eth_accounts
eth_chainId (2)
eth_estimateGas
eth_chainId
eth_getTransactionCount
eth_feeHistory
eth_sendTransaction
  Contract deployment: <UnrecognizedContract>
  Contract address:    0xe7f1725e7734ce288f8367e1bb143e90bb3f0512
  Transaction:         0xa9dbae5562a285cc3846ac44d75a53f5317aa0d62fee48364e7126dbaf8bfcbf
  From:                0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
  Value:               0 ETH
  Gas used:            109206 of 109206
  Block #2:            0x454dbbfbe9c2e5639dc32e2082a0efdb9db8f246e9e030e4556b36f57f8a1755

eth_chainId
eth_getTransactionByHash
eth_chainId
eth_getTransactionReceipt
```

Compile, deploy, and publish all contracts in your packages/contracts file:

```
yarn deploy
```

You'll to see:

```
llabori@Xubuntu64Bits-virtual-machine:~/BlockChains/AlchemyUniversity/6-StakingApp$ yarn deploy
yarn run v1.22.19
$ yarn workspace @scaffold-eth/hardhat deploy
$ hardhat deploy --export-all ../react-app/src/contracts/hardhat_contracts.json
Downloading compiler 0.8.4
Compiling 3 files with 0.8.4
Compilation finished successfully
deploying "ExampleExternalContract" (tx: 0x969048d3d9d28fe79e4acc6c4a78e29e962d37301144c6ab104af437fd7915bf)...: deployed at 0x5FbDB2315678afecb367f032d93F642f64180aa3 with 87965 gas
deploying "Staker" (tx: 0xa9dbae5562a285cc3846ac44d75a53f5317aa0d62fee48364e7126dbaf8bfcbf)...: deployed at 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 with 109206 gas
$ hardhat run scripts/publish.js
✅  Published contracts to the subgraph package.
Done in 130.58s.
```


    Whenever you update your contracts, run yarn deploy --reset to "refresh" your contracts in Scaffold-Eth.

Nice! You should now be able to see this repository's UI frontend at http://localhost:3000/


_Get Familiar with Scaffold-Eth_

While I know you're dying to get started with code, only a few more details to take care of!

In our default view, we have two tabs— Staker UI & Debug Contracts. 


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un título")


Go ahead and switch back and forth on your frontend to take a look at the different features.

Staker UI contains all the frontend components we'll be primarily interacting with.

If you click on the provided buttons, you'll notice that most of them aren't quite hooked up yet and 
you'll immediately run into errors.

    📘

    Take a look at Staker UI. You'll notice that it's painfully spartan! That's what we'll be primarily flushing out.

Since any on-chain interaction on Ethereum requires testnet ETH, you'll need local testnet ETH to begin 
hacking away.

First, grab your localhost wallet address.

Click on the "Copy" button in the upper right-hand corner


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un título")


Next, head to the lower left-hand corner. You'll be able to access the local faucet here.

    Either copy/paste in your address in the open field or click on the "Wallet" icon
    Paste in your address in the expanded view
    Send yourself some test ETH


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un título")


After topping your local wallet, you'll be able to interact with your contracts!

The second tab, Debug Contracts, is another frontend display that contains one of Scaffold-Eth's 
superpowers!

Once you yarn deploy your contracts and configure it to read the contract data properly, it'll 
automatically generate a barebones UI allowing you to interact with your contract's functions.

For example, in the sample below, we can read and write information via our smart contract by simply 
dropping in parameters and clicking "Send". With Scaffold-Eth, we don't need to only use CLI commands and 
have a more intuitive way of prototyping.

    📘

    If you want to store and view a variable in the Debug Contractstab, make sure to set the variable as public!

Awesome!

Now that we're familiar with Scaffold-Eth, we can dive deeper into the code.


_Dive into Solidity_

Looking at our Staker.sol file, we see that we have quite an empty Solidity file with a bunch of comments 
that dictate what needs to be filled out.

Since the Road to Web3 (R2W3) tutorial deviates from Scaffold-Eth's Challenge #1, we can go ahead and 
ignore the comments and start off with the following code.

```
pragma solidity >=0.6.0 <0.7.0;

import "hardhat/console.sol";
import "./ExampleExternalContract.sol";

contract Staker {
  ExampleExternalContract public exampleExternalContract;
  
  constructor(address exampleExternalContractAddress) public {
      exampleExternalContract = ExampleExternalContract(exampleExternalContractAddress);
  }
  
}
```

Project Parameters

Before writing out our smart contract code, let's go over how we expect our staking dApp to work!

    For simplicity, we only expect a single user to interact with our staking dApp
    We need to be able to deposit and withdraw from the Staker Contract
        Staking is a single-use action, meaning once we stake we cannot re-stake again
        Withdraws from the contract removes the entire principal balance and any accrued interest
    The Staker contract has an interest payout rate of 0.1 ETH for every second that the deposited ETH is 
    eligible for interest accrument
    Upon contract deployment, the Staker contract should begin with 2 timestamp counters. The first 
    deadline should be set to 2 minutes and the second set to 4 minutes
        The 2-minute deadline dictates the period in which the staking user is able to deposit funds. 
        (Between t=0 minutes and t=2 minutes, the staking user can deposit)
        All blocks that take place between the deposit of funds to the 2-minute deadline are valid for 
        interest accrual
        After the 2-minute withdrawal deadline has passed, the staking user is able to withdraw the 
        entire principal balance and any accrued interest until the 4-minute deadline hits
        After the additional 2-minute window for withdraws has passed, the user is blocked from 
        withdrawing their funds since they timed out.
    If a staking user has funds left, we have one last function which we can call to "lock" the funds in 
    an external contract that is already pre-installed in our Scaffold-Eth environment, 
    ExampleExternalContract.sol


    📘

    While the staking parameters listed above may seem a bit convoluted, many real-life staking dApps 
    feature similar primitives where users have a limited period for deposits and withdraws.

    And, many dApps will disincentivize "unproductive" capital that is simply sitting around after 
    staking periods have ended.

    Sometimes, the DeFi protocol may even absorb the outstanding deposits after a waiting period ends 
    similar to the last parameter we stated in our tutorial.

Solidity Mappingss

In our smart contract, we'll need two mappings to help us store some data.

In particular, we need something to keep track of:

    how much ETH is deposited into the contract
    the time that the deposit happened

We can achieve this with:

```
mapping(address => uint256) public balances; 
mapping(address => uint256) public depositTimestamps;
```

Public Variables

In pursuant to the guidelines outlined above, we'll also need a handful of different variables.

```
uint256 public constant rewardRatePerSecond = 0.1 ether; 
uint256 public withdrawalDeadline = block.timestamp + 120 seconds; 
uint256 public claimDeadline = block.timestamp + 240 seconds; 
uint256 public currentBlock = 0;
```

The reward rate sets the interest rate for the disbursement of ETH on the principal amount staked.

The withdrawal and claim deadlines help us set deadlines for the staking mechanics to begin/end.

And, lastly, we have a variable that we use to save the current block.

    📘

    We use block.timestamp + XXX seconds to create deadlines exactly XXX seconds after our contract is 
    initatiated. It's definitely a bit "naive" as a timing mechanism; can you think of a better way to 
    implement this so it's more generalizable for instance?

Events

Even though we will not push events to our frontend, we should still make sure we emit them in key parts 
of our contract to ensure that we maintain best programming practices.

```
event Stake(address indexed sender, uint256 amount); 
event Received(address, uint); 
event Execute(address indexed sender, uint256 amount);
```

Now that we have key parameters/variables locked down, we can move on to craft the specific functions 
we'll be using in our tutorial.

READ ONLY Time Functions

As stated in the project parameters, many of the different staking dApp's functionalities are subject to 
"time-locks" which enable/prohibit certain actions are particular points in time.

Here we have two different functions that govern the start and end of the withdrawal window.

```
  function withdrawalTimeLeft() public view returns (uint256 withdrawalTimeLeft) {
    if( block.timestamp >= withdrawalDeadline) {
      return (0);
    } else {
      return (withdrawalDeadline - block.timestamp);
    }
  }

  function claimPeriodLeft() public view returns (uint256 claimPeriodLeft) {
    if( block.timestamp >= claimDeadline) {
      return (0);
    } else {
      return (claimDeadline - block.timestamp);
    }
  }
```

Both functions are actually very familiar in design.

They both feature a standard if -> else statement.

The conditional simply checks whether the current time is greater than or less than the deadlines 
dictated in the public variables section.

If the current time is greater than the pre-arranged deadlines, we know that the deadline has passed and 
we return 0 to signify that a "state change" has occurred.

Otherwise, we simply return the remaining time before the deadline is reached.


Modifiers

For a more in-depth example of a modifier take look at [Solidity By Example](https://solidity-by-example.org/function-modifier).

As a gist, Solidity modifiers are pieces of code that can run before and/or after a function call.

While they have many different purposes, one of the most common and basic use cases is for restricting 
access to certain functions if particular conditions are not fully met.

In this tutorial, we'll be precisely using modifiers to help gate key functions that dictate our stake, 
withdraw, and repatriation functionalities.

Here are the three modifiers we use:

```
  modifier withdrawalDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = withdrawalTimeLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Withdrawal period is not reached yet");
    } else {
      require(timeRemaining > 0, "Withdrawal period has been reached");
    }
    _;
  }

  modifier claimDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = claimPeriodLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Claim deadline is not reached yet");
    } else {
      require(timeRemaining > 0, "Claim deadline has been reached");
    }
    _;
  }

  modifier notCompleted() {
    bool completed = exampleExternalContract.completed();
    require(!completed, "Stake already completed!");
    _;
  }
```

The modifiers withdrawalDeadlineReached(bool requireReached) & claimDeadlineReached(bool requireReached) 
both accept a boolean parameter and check to ensure that their respective deadlines are either true or 
false.

The modifier notCompleted() operates in a similar fashion but is actually a little bit more complex in 
nature even though it contains fewer lines of code.

It actually calls on a function completed() from an external contract outside of Staker and checks to see 
if it's returning true or false to confirm if that flag has been switched.

Now, let's implement the modifiers we just created on the next couple of functions to gate access.

Depositing/Staking Function

In our stake function, we use the modifiers created earlier by setting the params within 
withdrawalDeadlineReached() to be false and claimDeadlineReached() to be false since we don't want either 
deadline to have passed yet.

```
  // Stake function for a user to stake ETH in our contract
  
  function stake() public payable withdrawalDeadlineReached(false) claimDeadlineReached(false) {
    balances[msg.sender] = balances[msg.sender] + msg.value;
    depositTimestamps[msg.sender] = block.timestamp;
    emit Stake(msg.sender, msg.value);
  }
```

The rest of the function is fairly standard in a typical "deposit" scenario where our balance mapping is 
updated to include the money sent in.

We also set our deposit timestamp with the current time of the deposit so that we access that stored 
value for interest calculations later.

Withdrawal Function

In our withdrawal function, we again use the modifiers created earlier but this time we 
wantwithdrawalDeadlineReached() to be true and claimDeadlineReached() to be false.

This set of modifiers/parameters means that we are in the sweet spot for the withdrawal window since its 
time for the withdrawal to take place without any penalties and we get interest as well.

```
  /*
  Withdraw function for a user to remove their staked ETH inclusive
  of both the principle balance and any accrued interest
  */
  
  function withdraw() public withdrawalDeadlineReached(true) claimDeadlineReached(false) notCompleted{
    require(balances[msg.sender] > 0, "You have no balance to withdraw!");
    uint256 individualBalance = balances[msg.sender];
    uint256 indBalanceRewards = individualBalance + ((block.timestamp-depositTimestamps[msg.sender])*rewardRatePerSecond);
    balances[msg.sender] = 0;

    // Transfer all ETH via call! (not transfer) cc: https://solidity-by-example.org/sending-ether
    (bool sent, bytes memory data) = msg.sender.call{value: indBalanceRewards}("");
    require(sent, "RIP; withdrawal failed :( ");
  }
```

The rest of the function does a few important steps.

    It checks to ensure that the person trying to withdraw ETH actually has a non-zero stake.
    It calculates the amount of ETH owed in interest by taking the number of seconds that passed from 
    deposit to withdrawal and multiplying that by our interest constant.
    It sets the user's balance staked ETH to 0 so that no double counting can occur.
    It transfers the ETH from the smart contract back to the user's wallet.

Execute Repatriation Function

Here, we want claimDeadlineReached() to be true since the repatriation of unproductive funds can only 
happen after the 4-minute mark.

Likewise, we want notCompleted to be true since this dApp is only designed for a single use.

```
  /*
  Allows any user to repatriate "unproductive" funds that are left in the staking contract
  past the defined withdrawal period
  */
  
  function execute() public claimDeadlineReached(true) notCompleted {
    uint256 contractBalance = address(this).balance;
    exampleExternalContract.complete{value: address(this).balance}();
  }
```

The rest of the functions:

    Grabs the current balance of ETH in the Staker contract
    Sends the ETH to the repo's exampleExternalContract

If you've followed along with the Solidity so far, your Staker.sol should look like the following:

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

import "hardhat/console.sol";
import "./ExampleExternalContract.sol";

contract Staker {

  ExampleExternalContract public exampleExternalContract;

  mapping(address => uint256) public balances;
  mapping(address => uint256) public depositTimestamps;

  uint256 public constant rewardRatePerSecond = 0.1 ether;
  uint256 public withdrawalDeadline = block.timestamp + 120 seconds;
  uint256 public claimDeadline = block.timestamp + 240 seconds;
  uint256 public currentBlock = 0;

  // Events
  event Stake(address indexed sender, uint256 amount);
  event Received(address, uint);
  event Execute(address indexed sender, uint256 amount);

  // Modifiers
  /*
  Checks if the withdrawal period has been reached or not
  */
  modifier withdrawalDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = withdrawalTimeLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Withdrawal period is not reached yet");
    } else {
      require(timeRemaining > 0, "Withdrawal period has been reached");
    }
    _;
  }

  /*
  Checks if the claim period has ended or not
  */
  modifier claimDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = claimPeriodLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Claim deadline is not reached yet");
    } else {
      require(timeRemaining > 0, "Claim deadline has been reached");
    }
    _;
  }

  /*
  Requires that the contract only be completed once!
  */
  modifier notCompleted() {
    bool completed = exampleExternalContract.completed();
    require(!completed, "Stake already completed!");
    _;
  }

  constructor(address exampleExternalContractAddress){
      exampleExternalContract = ExampleExternalContract(exampleExternalContractAddress);
  }

  // Stake function for a user to stake ETH in our contract
  function stake() public payable withdrawalDeadlineReached(false) claimDeadlineReached(false){
    balances[msg.sender] = balances[msg.sender] + msg.value;
    depositTimestamps[msg.sender] = block.timestamp;
    emit Stake(msg.sender, msg.value);
  }

  /*
  Withdraw function for a user to remove their staked ETH inclusive
  of both principal and any accrued interest
  */
  function withdraw() public withdrawalDeadlineReached(true) claimDeadlineReached(false) notCompleted{
    require(balances[msg.sender] > 0, "You have no balance to withdraw!");
    uint256 individualBalance = balances[msg.sender];
    uint256 indBalanceRewards = individualBalance + ((block.timestamp-depositTimestamps[msg.sender])*rewardRatePerSecond);
    balances[msg.sender] = 0;

    // Transfer all ETH via call! (not transfer) cc: https://solidity-by-example.org/sending-ether
    (bool sent, bytes memory data) = msg.sender.call{value: indBalanceRewards}("");
    require(sent, "RIP; withdrawal failed :( ");
  }

  /*
  Allows any user to repatriate "unproductive" funds that are left in the staking contract
  past the defined withdrawal period
  */
  function execute() public claimDeadlineReached(true) notCompleted {
    uint256 contractBalance = address(this).balance;
    exampleExternalContract.complete{value: address(this).balance}();
  }

  /*
  READ-ONLY function to calculate the time remaining before the minimum staking period has passed
  */
  function withdrawalTimeLeft() public view returns (uint256 withdrawalTimeLeft) {
    if( block.timestamp >= withdrawalDeadline) {
      return (0);
    } else {
      return (withdrawalDeadline - block.timestamp);
    }
  }

  /*
  READ-ONLY function to calculate the time remaining before the minimum staking period has passed
  */
  function claimPeriodLeft() public view returns (uint256 claimPeriodLeft) {
    if( block.timestamp >= claimDeadline) {
      return (0);
    } else {
      return (claimDeadline - block.timestamp);
    }
  }

  /*
  Time to "kill-time" on our local testnet
  */
  function killTime() public {
    currentBlock = block.timestamp;
  }

  /*
  \Function for our smart contract to receive ETH
  cc: https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function
  */
  receive() external payable {
      emit Received(msg.sender, msg.value);
  }

}
```

_Foray into the Frontend_

Awesome! We just went through a bunch of Solidity. When it comes to frontend displays, Scaffold-Eth tries 
to keep things nice and simple. It contains a lot of different react components that give users low code 
solutions for awesome UIs! I encourage you to play around with the different components available to you 
but, in the meantime, we are going to learn more on the spartan side.

Looking at our App.jsx file, specifically at the code block around link 573, we see a block of code used 
to capture emitted events from our Solidity contracts and display them as a list.

Effectively, it allows us to log the different actions fired off from our smart contracts, parse the 
information stored, and then visually allow dApp users to look at their on-chain history.

While we will practice good programming standards by emitting events in our Solidity contract, this time, 
we'll be ignoring events in our frontend for simplicity so let's remove that code block entirely.

If you look at your Staker UI tab, you'll notice that the events box has been erased.
Frontend Edits

For the most part, a lot of the frontend code will remain the same as the default! In the previous step, 
we already removed the events react component.

Our final goal will be to have a nice, simple UI that looks like the following:


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un título")

Note that the default frontend is missing a few of the visual elements from the default repo.

Reward Rate Per Second

To construct this part, we insert the following piece of code right under the Staker contract block:

```
  <div style={{ padding: 8, marginTop: 16 }}>
    <div>Reward Rate Per Second:</div>
    <Balance balance={rewardRatePerSecond} fontSize={64} /> ETH
  </div>
```

Here, we take advantage of Scaffold-Eth's balance react component to help with formatting!

Deadline UI Elements

To construct the deadline visual component, we use the following 2 snippets of code:

```
  ** keep track of a variable from the contract in the local React state:
  const claimPeriodLeft = useContractReader(readContracts, "Staker", "claimPeriodLeft");
  console.log("⏳ Claim Period Left:", claimPeriodLeft);

  const withdrawalTimeLeft = useContractReader(readContracts, "Staker", "withdrawalTimeLeft");
  console.log("⏳ Withdrawal Time Left:", withdrawalTimeLeft);
```

```
 <div style={{ padding: 8, marginTop: 16, fontWeight: "bold" }}>
  <div>Claim Period Left:</div>
  {claimPeriodLeft && humanizeDuration(claimPeriodLeft.toNumber() * 1000)}
 </div>

 <div style={{ padding: 8, marginTop: 16, fontWeight: "bold"}}>
  <div>Withdrawal Period Left:</div>
  {withdrawalTimeLeft && humanizeDuration(withdrawalTimeLeft.toNumber() * 1000)}
 </div>
```

While the second code snippet is familiar and resembles what we have already seen, the first block of 
code is a bit unique. Not that we call claimPeriodLeft and withdrawalTimeLeft to access the stored 
variable values in the frontend.

However, we must actually read the values themselves first from the smart contract. The first code 
snippet handles this logic!

Misc. Edits to Other Existing UI components

Now that you've seen 2 different examples of frontend UI components, can you figure out how to make the 
remaining changes to the frontend so that it'll look like the sample provided above!

You should only need to adjust a few parameters here so don't think too much!

Freestyling on the UI

While Scaffold-Eth has a lot of default components that empower users to simply leverage "low-code" 
solutions, it also gives users access larger frontend libraries.

By default, it has a hook into Ant Design react components (https://ant.design/components/overview/) 
which allows anyone to pull components from there!

In our sample frontend, we actually see "lines" dividing each large block of visual components.

Recreate these lines by exploring the different options available in Ant Design

    📘

    If you want a hint, take a look at Ant Dividers!

    Don't forget that we need to import the component we plan on using at the top of our App.jsx file!


    import { Alert, Button, Col, Menu, Row, List, Divider } from "antd";

If you've followed along with the frontend code, your App.jsx should look like the following:

```
import WalletConnectProvider from "@walletconnect/web3-provider";
//import Torus from "@toruslabs/torus-embed"
import WalletLink from "walletlink";
import { Alert, Button, Col, Menu, Row, List, Divider } from "antd";
import "antd/dist/antd.css";
import React, { useCallback, useEffect, useState } from "react";
import { BrowserRouter, Link, Route, Switch } from "react-router-dom";
import Web3Modal from "web3modal";
import "./App.css";
import { Account, Address, Balance, Contract, Faucet, GasGauge, Header, Ramp, ThemeSwitch } from "./components";
import { INFURA_ID, NETWORK, NETWORKS } from "./constants";
import { Transactor } from "./helpers";
import {
  useBalance,
  useContractLoader,
  useContractReader,
  useGasPrice,
  useOnBlock,
  useUserProviderAndSigner,
} from "eth-hooks";
import { useEventListener } from "eth-hooks/events/useEventListener";
import { useExchangeEthPrice } from "eth-hooks/dapps/dex";
// import Hints from "./Hints";
import { ExampleUI, Hints, Subgraph } from "./views";

import { useContractConfig } from "./hooks";
import Portis from "@portis/web3";
import Fortmatic from "fortmatic";
import Authereum from "authereum";
import humanizeDuration from "humanize-duration";

const { ethers } = require("ethers");
/*
    Welcome to 🏗 scaffold-eth !

    Code:
    https://github.com/austintgriffith/scaffold-eth

    Support:
    https://t.me/joinchat/KByvmRe5wkR-8F_zz6AjpA
    or DM @austingriffith on Twitter or Telegram

    You should get your own Infura.io ID and put it in `constants.js`
    (this is your connection to the main Ethereum network for ENS etc.)


    🌏 EXTERNAL CONTRACTS:
    You can also bring in contract artifacts in `constants.js`
    (and then use the `useExternalContractLoader()` hook!)
*/

/// 📡 What chain are your contracts deployed to?
const targetNetwork = NETWORKS.localhost; // <------- select your target frontend network (localhost, rinkeby, xdai, mainnet)

// 😬 Sorry for all the console logging
const DEBUG = true;
const NETWORKCHECK = true;

// 🛰 providers
if (DEBUG) console.log("📡 Connecting to Mainnet Ethereum");
// const mainnetProvider = getDefaultProvider("mainnet", { infura: INFURA_ID, etherscan: ETHERSCAN_KEY, quorum: 1 });
// const mainnetProvider = new InfuraProvider("mainnet",INFURA_ID);
//
// attempt to connect to our own scaffold eth rpc and if that fails fall back to infura...
// Using StaticJsonRpcProvider as the chainId won't change see https://github.com/ethers-io/ethers.js/issues/901
const scaffoldEthProvider = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider("https://rpc.scaffoldeth.io:48544")
  : null;
const poktMainnetProvider = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider(
      "https://eth-mainnet.gateway.pokt.network/v1/lb/611156b4a585a20035148406",
    )
  : null;
const mainnetInfura = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider("https://mainnet.infura.io/v3/" + INFURA_ID)
  : null;
// ( ⚠️ Getting "failed to meet quorum" errors? Check your INFURA_ID

// 🏠 Your local provider is usually pointed at your local blockchain
const localProviderUrl = targetNetwork.rpcUrl;
// as you deploy to other networks you can set REACT_APP_PROVIDER=https://dai.poa.network in packages/react-app/.env
const localProviderUrlFromEnv = process.env.REACT_APP_PROVIDER ? process.env.REACT_APP_PROVIDER : localProviderUrl;
if (DEBUG) console.log("🏠 Connecting to provider:", localProviderUrlFromEnv);
const localProvider = new ethers.providers.StaticJsonRpcProvider(localProviderUrlFromEnv);

// 🔭 block explorer URL
const blockExplorer = targetNetwork.blockExplorer;

// Coinbase walletLink init
const walletLink = new WalletLink({
  appName: "coinbase",
});

// WalletLink provider
const walletLinkProvider = walletLink.makeWeb3Provider(`https://mainnet.infura.io/v3/${INFURA_ID}`, 1);

// Portis ID: 6255fb2b-58c8-433b-a2c9-62098c05ddc9
/*
  Web3 modal helps us "connect" external wallets:
*/
const web3Modal = new Web3Modal({
  network: "mainnet", // Optional. If using WalletConnect on xDai, change network to "xdai" and add RPC info below for xDai chain.
  cacheProvider: true, // optional
  theme: "light", // optional. Change to "dark" for a dark theme.
  providerOptions: {
    walletconnect: {
      package: WalletConnectProvider, // required
      options: {
        bridge: "https://polygon.bridge.walletconnect.org",
        infuraId: INFURA_ID,
        rpc: {
          1: `https://mainnet.infura.io/v3/${INFURA_ID}`, // mainnet // For more WalletConnect providers: https://docs.walletconnect.org/quick-start/dapps/web3-provider#required
          42: `https://kovan.infura.io/v3/${INFURA_ID}`,
          100: "https://dai.poa.network", // xDai
        },
      },
    },
    portis: {
      display: {
        logo: "https://user-images.githubusercontent.com/9419140/128913641-d025bc0c-e059-42de-a57b-422f196867ce.png",
        name: "Portis",
        description: "Connect to Portis App",
      },
      package: Portis,
      options: {
        id: "6255fb2b-58c8-433b-a2c9-62098c05ddc9",
      },
    },
    fortmatic: {
      package: Fortmatic, // required
      options: {
        key: "pk_live_5A7C91B2FC585A17", // required
      },
    },
    // torus: {
    //   package: Torus,
    //   options: {
    //     networkParams: {
    //       host: "https://localhost:8545", // optional
    //       chainId: 1337, // optional
    //       networkId: 1337 // optional
    //     },
    //     config: {
    //       buildEnv: "development" // optional
    //     },
    //   },
    // },
    "custom-walletlink": {
      display: {
        logo: "https://play-lh.googleusercontent.com/PjoJoG27miSglVBXoXrxBSLveV6e3EeBPpNY55aiUUBM9Q1RCETKCOqdOkX2ZydqVf0",
        name: "Coinbase",
        description: "Connect to Coinbase Wallet (not Coinbase App)",
      },
      package: walletLinkProvider,
      connector: async (provider, _options) => {
        await provider.enable();
        return provider;
      },
    },
    authereum: {
      package: Authereum, // required
    },
  },
});

function App(props) {
  const mainnetProvider =
    poktMainnetProvider && poktMainnetProvider._isProvider
      ? poktMainnetProvider
      : scaffoldEthProvider && scaffoldEthProvider._network
      ? scaffoldEthProvider
      : mainnetInfura;

  const [injectedProvider, setInjectedProvider] = useState();
  const [address, setAddress] = useState();

  const logoutOfWeb3Modal = async () => {
    await web3Modal.clearCachedProvider();
    if (injectedProvider && injectedProvider.provider && typeof injectedProvider.provider.disconnect == "function") {
      await injectedProvider.provider.disconnect();
    }
    setTimeout(() => {
      window.location.reload();
    }, 1);
  };

  /* 💵 This hook will get the price of ETH from 🦄 Uniswap: */
  const price = useExchangeEthPrice(targetNetwork, mainnetProvider);

  /* 🔥 This hook will get the price of Gas from ⛽️ EtherGasStation */
  const gasPrice = useGasPrice(targetNetwork, "fast");
  // Use your injected provider from 🦊 Metamask or if you don't have it then instantly generate a 🔥 burner wallet.
  const userProviderAndSigner = useUserProviderAndSigner(injectedProvider, localProvider);
  const userSigner = userProviderAndSigner.signer;

  useEffect(() => {
    async function getAddress() {
      if (userSigner) {
        const newAddress = await userSigner.getAddress();
        setAddress(newAddress);
      }
    }
    getAddress();
  }, [userSigner]);

  // You can warn the user if you would like them to be on a specific network
  const localChainId = localProvider && localProvider._network && localProvider._network.chainId;
  const selectedChainId =
    userSigner && userSigner.provider && userSigner.provider._network && userSigner.provider._network.chainId;

  // For more hooks, check out 🔗eth-hooks at: https://www.npmjs.com/package/eth-hooks

  // The transactor wraps transactions and provides notificiations
  const tx = Transactor(userSigner, gasPrice);

  // Faucet Tx can be used to send funds from the faucet
  const faucetTx = Transactor(localProvider, gasPrice);

  // 🏗 scaffold-eth is full of handy hooks like this one to get your balance:
  const yourLocalBalance = useBalance(localProvider, address);

  // Just plug in different 🛰 providers to get your balance on different chains:
  const yourMainnetBalance = useBalance(mainnetProvider, address);

  const contractConfig = useContractConfig();

  // Load in your local 📝 contract and read a value from it:
  const readContracts = useContractLoader(localProvider, contractConfig);

  // If you want to make 🔐 write transactions to your contracts, use the userSigner:
  const writeContracts = useContractLoader(userSigner, contractConfig, localChainId);

  // EXTERNAL CONTRACT EXAMPLE:
  //
  // If you want to bring in the mainnet DAI contract it would look like:
  const mainnetContracts = useContractLoader(mainnetProvider, contractConfig);

  // If you want to call a function on a new block
  useOnBlock(mainnetProvider, () => {
    console.log(`⛓ A new mainnet block is here: ${mainnetProvider._lastBlockNumber}`);
  });

  // Then read your DAI balance like:
  const myMainnetDAIBalance = useContractReader(mainnetContracts, "DAI", "balanceOf", [
    "0x34aA3F359A9D614239015126635CE7732c18fDF3",
  ]);

  //keep track of contract balance to know how much has been staked total:
  const stakerContractBalance = useBalance(
    localProvider,
    readContracts && readContracts.Staker ? readContracts.Staker.address : null,
  );
  if (DEBUG) console.log("💵 stakerContractBalance", stakerContractBalance);

  const rewardRatePerSecond = useContractReader(readContracts, "Staker", "rewardRatePerSecond");
  console.log("💵 Reward Rate:", rewardRatePerSecond);

  // ** keep track of a variable from the contract in the local React state:
  const balanceStaked = useContractReader(readContracts, "Staker", "balances", [address]);
  console.log("💸 balanceStaked:", balanceStaked);

  // ** 📟 Listen for broadcast events
  const stakeEvents = useEventListener(readContracts, "Staker", "Stake", localProvider, 1);
  console.log("📟 stake events:", stakeEvents);

  const receiveEvents = useEventListener(readContracts, "Staker", "Received", localProvider, 1);
  console.log("📟 receive events:", receiveEvents);

  // ** keep track of a variable from the contract in the local React state:
  const claimPeriodLeft = useContractReader(readContracts, "Staker", "claimPeriodLeft");
  console.log("⏳ Claim Period Left:", claimPeriodLeft);

  const withdrawalTimeLeft = useContractReader(readContracts, "Staker", "withdrawalTimeLeft");
  console.log("⏳ Withdrawal Time Left:", withdrawalTimeLeft);


  // ** Listen for when the contract has been 'completed'
  const complete = useContractReader(readContracts, "ExampleExternalContract", "completed");
  console.log("✅ complete:", complete);

  const exampleExternalContractBalance = useBalance(
    localProvider,
    readContracts && readContracts.ExampleExternalContract ? readContracts.ExampleExternalContract.address : null,
  );
  if (DEBUG) console.log("💵 exampleExternalContractBalance", exampleExternalContractBalance);

  let completeDisplay = "";
  if (complete) {
    completeDisplay = (
      <div style={{padding: 64, backgroundColor: "#eeffef", fontWeight: "bold", color: "rgba(0, 0, 0, 0.85)" }} >
        -- 💀 Staking App Fund Repatriation Executed 🪦 --
        <Balance balance={exampleExternalContractBalance} fontSize={32} /> ETH locked!
      </div>
    );
  }

  /*
  const addressFromENS = useResolveName(mainnetProvider, "austingriffith.eth");
  console.log("🏷 Resolved austingriffith.eth as:", addressFromENS)
  */

  //
  // 🧫 DEBUG 👨🏻‍🔬
  //
  useEffect(() => {
    if (
      DEBUG &&
      mainnetProvider &&
      address &&
      selectedChainId &&
      yourLocalBalance &&
      yourMainnetBalance &&
      readContracts &&
      writeContracts &&
      mainnetContracts
    ) {
      console.log("_____________________________________ 🏗 scaffold-eth _____________________________________");
      console.log("🌎 mainnetProvider", mainnetProvider);
      console.log("🏠 localChainId", localChainId);
      console.log("👩‍💼 selected address:", address);
      console.log("🕵🏻‍♂️ selectedChainId:", selectedChainId);
      console.log("💵 yourLocalBalance", yourLocalBalance ? ethers.utils.formatEther(yourLocalBalance) : "...");
      console.log("💵 yourMainnetBalance", yourMainnetBalance ? ethers.utils.formatEther(yourMainnetBalance) : "...");
      console.log("📝 readContracts", readContracts);
      console.log("🌍 DAI contract on mainnet:", mainnetContracts);
      console.log("💵 yourMainnetDAIBalance", myMainnetDAIBalance);
      console.log("🔐 writeContracts", writeContracts);
    }
  }, [
    mainnetProvider,
    address,
    selectedChainId,
    yourLocalBalance,
    yourMainnetBalance,
    readContracts,
    writeContracts,
    mainnetContracts,
  ]);

  let networkDisplay = "";
  if (NETWORKCHECK && localChainId && selectedChainId && localChainId !== selectedChainId) {
    const networkSelected = NETWORK(selectedChainId);
    const networkLocal = NETWORK(localChainId);
    if (selectedChainId === 1337 && localChainId === 31337) {
      networkDisplay = (
        <div style={{ zIndex: 2, position: "absolute", right: 0, top: 60, padding: 16 }}>
          <Alert
            message="⚠️ Wrong Network ID"
            description={
              <div>
                You have <b>chain id 1337</b> for localhost and you need to change it to <b>31337</b> to work with
                HardHat.
                <div>(MetaMask -&gt; Settings -&gt; Networks -&gt; Chain ID -&gt; 31337)</div>
              </div>
            }
            type="error"
            closable={false}
          />
        </div>
      );
    } else {
      networkDisplay = (
        <div style={{ zIndex: 2, position: "absolute", right: 0, top: 60, padding: 16 }}>
          <Alert
            message="⚠️ Wrong Network"
            description={
              <div>
                You have <b>{networkSelected && networkSelected.name}</b> selected and you need to be on{" "}
                <Button
                  onClick={async () => {
                    const ethereum = window.ethereum;
                    const data = [
                      {
                        chainId: "0x" + targetNetwork.chainId.toString(16),
                        chainName: targetNetwork.name,
                        nativeCurrency: targetNetwork.nativeCurrency,
                        rpcUrls: [targetNetwork.rpcUrl],
                        blockExplorerUrls: [targetNetwork.blockExplorer],
                      },
                    ];
                    console.log("data", data);

                    let switchTx;
                    // https://docs.metamask.io/guide/rpc-api.html#other-rpc-methods
                    try {
                      switchTx = await ethereum.request({
                        method: "wallet_switchEthereumChain",
                        params: [{ chainId: data[0].chainId }],
                      });
                    } catch (switchError) {
                      // not checking specific error code, because maybe we're not using MetaMask
                      try {
                        switchTx = await ethereum.request({
                          method: "wallet_addEthereumChain",
                          params: data,
                        });
                      } catch (addError) {
                        // handle "add" error
                      }
                    }

                    if (switchTx) {
                      console.log(switchTx);
                    }
                  }}
                >
                  <b>{networkLocal && networkLocal.name}</b>
                </Button>
              </div>
            }
            type="error"
            closable={false}
          />
        </div>
      );
    }
  } else {
    networkDisplay = (
      <div style={{ zIndex: -1, position: "absolute", right: 154, top: 28, padding: 16, color: targetNetwork.color }}>
        {targetNetwork.name}
      </div>
    );
  }

  const loadWeb3Modal = useCallback(async () => {
    const provider = await web3Modal.connect();
    setInjectedProvider(new ethers.providers.Web3Provider(provider));

    provider.on("chainChanged", chainId => {
      console.log(`chain changed to ${chainId}! updating providers`);
      setInjectedProvider(new ethers.providers.Web3Provider(provider));
    });

    provider.on("accountsChanged", () => {
      console.log(`account changed!`);
      setInjectedProvider(new ethers.providers.Web3Provider(provider));
    });

    // Subscribe to session disconnection
    provider.on("disconnect", (code, reason) => {
      console.log(code, reason);
      logoutOfWeb3Modal();
    });
  }, [setInjectedProvider]);

  useEffect(() => {
    if (web3Modal.cachedProvider) {
      loadWeb3Modal();
    }
  }, [loadWeb3Modal]);

  const [route, setRoute] = useState();
  useEffect(() => {
    setRoute(window.location.pathname);
  }, [setRoute]);

  let faucetHint = "";
  const faucetAvailable = localProvider && localProvider.connection && targetNetwork.name.indexOf("local") !== -1;

  const [faucetClicked, setFaucetClicked] = useState(false);
  if (
    !faucetClicked &&
    localProvider &&
    localProvider._network &&
    localProvider._network.chainId === 31337 &&
    yourLocalBalance &&
    ethers.utils.formatEther(yourLocalBalance) <= 0
  ) {
    faucetHint = (
      <div style={{ padding: 16 }}>
        <Button
          type="primary"
          onClick={() => {
            faucetTx({
              to: address,
              value: ethers.utils.parseEther("0.01"),
            });
            setFaucetClicked(true);
          }}
        >
          💰 Grab funds from the faucet ⛽️
        </Button>
      </div>
    );
  }

  return (
    <div className="App">
      {/* ✏️ Edit the header and change the title to your project name */}
      <Header />
      {networkDisplay}
      <BrowserRouter>
        <Menu style={{ textAlign: "center" }} selectedKeys={[route]} mode="horizontal">
          <Menu.Item key="/">
            <Link
              onClick={() => {
                setRoute("/");
              }}
              to="/"
            >
              Staker UI
            </Link>
          </Menu.Item>
          <Menu.Item key="/contracts">
            <Link
              onClick={() => {
                setRoute("/contracts");
              }}
              to="/contracts"
            >
              Debug Contracts
            </Link>
          </Menu.Item>
        </Menu>

        <Switch>
          <Route exact path="/">
            {completeDisplay}

            <div style={{ padding: 8, marginTop: 16 }}>
              <div>Staker Contract:</div>
              <Address value={readContracts && readContracts.Staker && readContracts.Staker.address} />
            </div>

            <Divider />

            <div style={{ padding: 8, marginTop: 16 }}>
              <div>Reward Rate Per Second:</div>
              <Balance balance={rewardRatePerSecond} fontSize={64} /> ETH
            </div>

            <Divider />

            <div style={{ padding: 8, marginTop: 16, fontWeight: "bold" }}>
              <div>Claim Period Left:</div>
              {claimPeriodLeft && humanizeDuration(claimPeriodLeft.toNumber() * 1000)}
            </div>

            <div style={{ padding: 8, marginTop: 16, fontWeight: "bold"}}>
              <div>Withdrawal Period Left:</div>
              {withdrawalTimeLeft && humanizeDuration(withdrawalTimeLeft.toNumber() * 1000)}
            </div>

            <Divider />

            <div style={{ padding: 8, fontWeight: "bold"}}>
              <div>Total Available ETH in Contract:</div>
              <Balance balance={stakerContractBalance} fontSize={64} />
            </div>

            <Divider />

            <div style={{ padding: 8,fontWeight: "bold" }}>
              <div>ETH Locked 🔒 in Staker Contract:</div>
              <Balance balance={balanceStaked} fontSize={64} />
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={"default"}
                onClick={() => {
                  tx(writeContracts.Staker.execute());
                }}
              >
                📡 Execute!
              </Button>
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={"default"}
                onClick={() => {
                  tx(writeContracts.Staker.withdraw());
                }}
              >
                🏧 Withdraw
              </Button>
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={balanceStaked ? "success" : "primary"}
                onClick={() => {
                  tx(writeContracts.Staker.stake({ value: ethers.utils.parseEther("0.5") }));
                }}
              >
                🥩 Stake 0.5 ether!
              </Button>
            </div>

            {/*
                🎛 this scaffolding is full of commonly used components
                this <Contract/> component will automatically parse your ABI
                and give you a form to interact with it locally
            */}

            {/* uncomment for a second contract:
            <Contract
              name="SecondContract"
              signer={userProvider.getSigner()}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
            */}
          </Route>
          <Route path="/contracts">
            <Contract
              name="Staker"
              signer={userSigner}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
            <Contract
              name="ExampleExternalContract"
              signer={userSigner}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
          </Route>
        </Switch>
      </BrowserRouter>

      <ThemeSwitch />

      {/* 👨‍💼 Your account is in the top right with a wallet at connect options */}
      <div style={{ position: "fixed", textAlign: "right", right: 0, top: 0, padding: 10 }}>
        <Account
          address={address}
          localProvider={localProvider}
          userSigner={userSigner}
          mainnetProvider={mainnetProvider}
          price={price}
          web3Modal={web3Modal}
          loadWeb3Modal={loadWeb3Modal}
          logoutOfWeb3Modal={logoutOfWeb3Modal}
          blockExplorer={blockExplorer}
        />
        {faucetHint}
      </div>

      <div style={{ marginTop: 32, opacity: 0.5 }}>
        {/* Add your address here */}
        Created by <Address value={"Your...address"} ensProvider={mainnetProvider} fontSize={16} />
      </div>

      <div style={{ marginTop: 32, opacity: 0.5 }}>
        <a target="_blank" style={{ padding: 32, color: "#000" }} href="https://github.com/scaffold-eth/scaffold-eth">
          🍴 Fork me!
        </a>
      </div>

      {/* 🗺 Extra UI like gas price, eth price, faucet, and support: */}
      <div style={{ position: "fixed", textAlign: "left", left: 0, bottom: 20, padding: 10 }}>
        <Row align="middle" gutter={[4, 4]}>
          <Col span={8}>
            <Ramp price={price} address={address} networks={NETWORKS} />
          </Col>

          <Col span={8} style={{ textAlign: "center", opacity: 0.8 }}>
            <GasGauge gasPrice={gasPrice} />
          </Col>
          <Col span={8} style={{ textAlign: "center", opacity: 1 }}>
            <Button
              onClick={() => {
                window.open("https://t.me/joinchat/KByvmRe5wkR-8F_zz6AjpA");
              }}
              size="large"
              shape="round"
            >
              <span style={{ marginRight: 8 }} role="img" aria-label="support">
                💬
              </span>
              Support
            </Button>
          </Col>
        </Row>

        <Row align="middle" gutter={[4, 4]}>
          <Col span={24}>
            {
              /*  if the local provider has a signer, let's show the faucet:  */
              faucetAvailable ? (
                <Faucet localProvider={localProvider} price={price} ensProvider={mainnetProvider} />
              ) : (
                ""
              )
            }
          </Col>
        </Row>
      </div>
    </div>
  );
}

export default App;
```

Awesome!

We've worked through a lot of new components together both in terms of developer environments, Solidity, 
and frontend code.

Verify that your dApp functions as expected!

    Does the dApp feature single-use staking?
    Are the withdrawal/fund repatriation conditions respected? Go ahead of hit yarn deploy --reset a few 
    times to check each window of time.


## Challenger 🛠️

Okay, now time for the best part. I'm going to leave you with a few extension challenges to try on your 
own, to see if you fully understand what you've learned here!

    1. Update the interest mechanism in the Staker.sol contract so that you receive a "non-linear" amount 
    of ETH based on the blocks between deposit and withdrawal

    📘

    I suggest implementing a basic exponential function!

2. Allow users to deposit any arbitrary amount of ETH into the smart contract, not just 0.5 ETH.

3. Instead of using the vanilla ExampleExternalContract contract, implement a function in Staker.sol that 
allows you to retrieve the ETH locked up in ExampleExternalContract and re-deposit it back into the 
Staker contract.


    Make sure to only "white-list" a single address to call this new function to gate its usage!
    Make sure that you create logic/remove existing code to ensure that users are able to interact with 
    the Staker contract over and over again! We want to be able to ping-pong from Staker -> 
    ExampleExternalContract repeatedly!

Once you're done with the challenge, tweet about it by tagging @AlchemyPlatform on Twitter and the author 
@crypt0zeke using the hashtag #roadtoweb3!


## Built with 🛠️

_Tools you used to develop the project/challenge_

- [Visual Studio Code](https://code.visualstudio.com/) - The IDE
- [Alchemy](https://dashboard.alchemy.com) - Interface/API to the Goerli Tesnet Network
- [Xubuntu](https://xubuntu.org/) - Operating system based on Ubuntu distribution.
- [Opensea](https://testnets.opensea.io) - Web system used to verify/visualizate our NFT
- [Goerli Testnet](https://goerli.etherscan.io) - Web system used to verify transactions, verify 
  contracts, deploy contracts, verify and publish contract source code, etc.
- [Goerli Faucet](https://goerlifaucet.com/) - Faucet used to obtain ETH used in the tests to deploy the 
  SmartContrat tests to deploy the SmartContrat as well as to interact with them.
- [Solidity](https://soliditylang.org) Object-oriented programming language for implementing smart
  contracts on various blockchain platforms
- [Hardhat](https://hardhat.org) - Environment developers use to test, compile, deploy and debug dApps
  based on the Ethereum blockchain
- [GitHub](https://github.com/) - Internet hosting service for software development and version
  control using Git
  the SmartContrat as well as to interact with them.
- [MetaMask](https://metamask.io) - MetaMask is a software cryptocurrency wallet used to interact with
  the Ethereum blockchain.

## Contributing 🖇️

Please read the [CONTRIBUTING.md](https://gist.github.com/llabori-venehsoftw/xxxxxx) for details of our 
code of conduct, and the process for submitting pull requests to us.

## Wiki 📖

N/A

## Versioning 📌

We use [GitHub] for versioning all the files used (https://github.com/tu/proyecto/tags).

## Autores ✒️

_People who collaborated with the development of the challenge_

- **VeneHsoftw** - _Initial Work_ - [venehsoftw](https://github.com/venehsoftw)
- **Luis Labori** - _Initial Work_, _Documentationn_ - [llabori-venehsoftw](https://github.com/
  llabori-venehsoftw)

## License 📄

This project is licensed under the License (Your License) - see the file [LICENSE.md](LICENSE.md) for
details.

## Gratitude 🎁

- If you found the information reflected in this Repo to be of great importance, please extend your
  collaboration by clicking on the star button on the upper right margin. 📢
- If it is within your means, you may extend your donation using the following address:
  `0xAeC4F555DbdE299ee034Ca6F42B83Baf8eFA6f0D`

---

⌨️ con ❤️ por [Venehsoftw](https://github.com/llabori-venehsoftw) 😊

```

```
