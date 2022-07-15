# Local Blockchain Testing

This tutorial should familiarize you with starting a local blockchain using Hardhat, deploying a sample smart contract to the local blockchain and interacting with that blockchain with Metamask and Remix.

---

### Why local blockchain?

- It is very useful to run a local blockchain because testing becomes very fast and efficient.
- Its only your machine which is running the blockchain and thus consensus is fast and you dont have to wait for other nodes to sync or validate.
- You can also use many specialized modules specially built for local testing like [Hardhat console.log](https://hardhat.org/tutorial/debugging-with-hardhat-network.html) which helps you to add printing inside your contract.

<Quiz questionId="008be2da-50d1-4267-928c-ff8b10eabd09" />

## Build

To build the smart contract we would be using [Hardhat](https://hardhat.org/). Hardhat is an Ethereum development environment and framework designed for full stack development in Solidity. In simple words you can write your smart contract, deploy them, run tests, and debug your code.

- To setup a Hardhat project, Open up a terminal and execute these commands

  ```bash
  mkdir Local-Blockchain-Testing
  cd Local-Blockchain-Testing
  npm init --yes
  npm install --save-dev hardhat
  ```

- In the same directory where you installed Hardhat run:

  ```bash
  npx hardhat
  ```

  - Select `Create a JavaScript project`
  - Press enter for the already specified `Hardhat Project root`
  - Press enter for the question on if you want to add a `.gitignore`
  - Press enter for `Do you want to install this sample project's dependencies with npm (@nomicfoundation/hardhat-toolbox)?`

Now you have a hardhat project ready to go!

If you are not on mac, please do this extra step and install these libraries as well :)

```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox
```

and press `enter` for all the questions.

In the contracts folder, create a new solidity file called Greeter.sol. Now we would write some code in the file.

```Solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "hardhat/console.sol";

contract Greeter {
    string private greeting;

    constructor(string memory _greeting) {
        console.log("Deploying a Greeter with greeting:", _greeting);
        greeting = _greeting;
    }

    function greet() public view returns (string memory) {
        return greeting;
    }

    function setGreeting(string memory _greeting) public {
        console.log("Changing greeting from '%s' to '%s'", greeting, _greeting);
        greeting = _greeting;
    }
}

```

- This contract declares a string - `greeting`. There are also two methods and a constructor. The constructor initiates the greeting variable with the provided string value.
- The `greet` method returns the greeting string. Since this is a `view` function, it costs no gas, and requires no signing to execute.
- The `setGreeting` method sets the greeting string with a provided user value. Since this updates the smart contract state, it costs gas, and requires signing.
  **One interesting thing to note about the `setGreeting` method is that it uses the Hardhat's console.log contract, so we can actually debug and see to what values was `greeting` changed to!**

<Quiz questionId="d7a6c9a2-2aca-47de-b5a9-805aafd04831" />

Now to actually start running your local blockchain in your terminal pointing to your directory execute this command:

```bash
npx hardhat node
```

**(Keep this terminal running)**

This command starts a local blockchain node for you. You should be able to see some accounts which have already been funded by hardhat with 10000 ETH

<Quiz questionId="c0ab069f-1e0c-4d13-bc12-d20792529539" />

![](https://i.imgur.com/NkwsCXn.png)

Now, you can continue by deploying the contract to the local blockchain using Hardhat. To do so, update the code inside `scripts/deploy.js` with the code below.

```js
// We require the Hardhat Runtime Environment explicitly here. This is optional
// but useful for running the script in a standalone fashion through `node <script>`.
//
// You can also run a script with `npx hardhat run <script>`. If you do that, Hardhat
// will compile your contracts, add the Hardhat Runtime Environment's members to the
// global scope, and execute the script.
const hre = require("hardhat");

async function main() {
  // We get the contract to deploy
  const Greeter = await hre.ethers.getContractFactory("Greeter");
  const greeter = await Greeter.deploy("Hello, Hardhat!");

  await greeter.deployed();

  console.log("Greeter deployed to:", greeter.address);
}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

Deploy to local blockchain by running `npx hardhat run scripts/deploy.js`.

Alternatively, you can also use something like Remix and have it deploy contracts to your local blockchain. The second method will also involve setting up Metamask to work with your local blockchain, and will give you an idea of how to locally test your React/Next.js apps using contracts running on the local blockchain as well, so let's do that.

## Metamask Connection

- To use metamask to connect to this network, click on your profile and then click on settings
  ![](https://i.imgur.com/rZi6Ofi.png)

- Then click on Networks, followed by `Localhost 8545`
  ![](https://i.imgur.com/X74AcuZ.png)

![](https://i.imgur.com/9SjtWCu.png)

- Change the Chain ID to `31337`(this is the chainId for the local blockchain you are running) and then click `Save`
  ![](https://i.imgur.com/Dt6py3h.png)

- Awesome now your MetaMask has a connection to your local blockchain, we will now add the accounts that Hardhat gave to us
- In the node terminal, you should see several accounts displayed. Let's grab one of those:

  ```Shell
  Account #0: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 (10000 ETH)
  Private Key: 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
  ```

Go to metamask --> click on your profile --> import account. Select private key in the dropdown and paste the private key from the account you wish. You should now see an account with 10000 ETH

<Quiz questionId="c28d2f88-b2c9-4e8b-b6ab-ac8400ede5ae" />

## Remix

We will now deploy our contract to local blockchain and interact with it using Remix

Go to [remix.ethereum.org](https://remix.ethereum.org/#optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.7+commit.e28d00a7.js) and create a new file inside the contracts folder named `Greeter.sol`

- Copy this code into it:

```Solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "hardhat/console.sol";

contract Greeter {
    string private greeting;

    constructor(string memory _greeting) {
        console.log("Deploying a Greeter with greeting:", _greeting);
        greeting = _greeting;
    }

    function greet() public view returns (string memory) {
        return greeting;
    }

    function setGreeting(string memory _greeting) public {
        console.log("Changing greeting from '%s' to '%s'", greeting, _greeting);
        greeting = _greeting;
    }
}

```

This is the same code, we explained above

- Compile `Greeter.sol`
  ![](https://i.imgur.com/bhAwIRf.png)

- Now to deploy, go to deployment tab and in your environment select `Injected Web3`, make sure that the account connected is the one that you imported above and the network is `Localhost 8545` on your MetaMask
- ![](https://i.imgur.com/zgGKlQm.png)
  ![](https://i.imgur.com/qrJTtLi.png)

- Set a greeting and click on deploy

- Your contract is now deployed 🎉

- Set a greeting and click on `setGreeting`

  ![](https://i.imgur.com/Rkc6tOH.png)

- Check your terminal which was running your hardhat node, it should have the console.log

  ![local-node-console-](https://user-images.githubusercontent.com/56781761/156940719-d41dbe65-9dde-40b5-83c4-6641c0fa9737.png)

<Quiz questionId="662f7e38-0490-4417-8ce0-0689fa7e3bb6" />

---

## Contributors

**This module was built in collaboration with [Hypotenuse Labs](https://hypotenuse.ca/)**

<Quiz questionId="a1cbd14b-3243-4be4-a5be-a6d9c6d95b09" />
<Quiz questionId="7f93386d-1b8e-470e-8517-1d13c683ecd7" />

<SubmitQuiz />
