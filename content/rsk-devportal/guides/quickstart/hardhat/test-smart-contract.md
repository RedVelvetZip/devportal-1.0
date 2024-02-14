---
menu_order: 400
menu_title: Test Smart Contracts
layout: rsk
title: 'Test Smart Contracts'
description: 'Learn how to test your Rootstock smart contract'
tags: quick-start, getting-started, guide, how-to, bitcoin, rsk, rootstock, blockchain
---

In this section, you'll set up a smart contract test and test your contract to ensure it works as expected.

Follow these steps below to test your smart contract.

### Step 1: Install Dependencies

We'll install the Mocha and Chai testing dependencies.

Mocha is a JavaScript test framework running on Node.js. Chai is an assertion library for the browser and Node that can be paired with any JavaScript testing framework.

- Before writing tests for your token contract, install the required testing dependencies:
  ```shell
  npm install --save-dev mocha@10.2.0 chai@4.2.0 @nomiclabs/hardhat-ethers@2.2.3
  ```

### Step 2: Create Tests

1. Navigate to the `test` directory in the root directory of your project, this is recommended for storing all test files:
    ```shell
    cd test
    ```

2. In the test directory, open the `MyToken.test.js` file where we'll write tests for your token contract using Mocha and Chai:

    - `MyToken.test.js` initial state:
        ```js
        const { expect } = require("chai");
        const { ethers } = require("hardhat");

        // Configure tests

        ```

    - Copy the code snipet below and paste it in your test file or see the [`MyToken.test.js` file](https://raw.githubusercontent.com/jesus-iov/rootstock-quick-start-guide/feat/complete/test/MyToken.test.js) on GitHub.
        ```js
        const { expect } = require("chai");
        const { ethers } = require("hardhat");

        describe("MyToken", function () {
          it("Should deploy MyToken and assign the total supply to the owner", async function () {
            const [owner] = await ethers.getSigners();

            const MyToken = await ethers.getContractFactory("MyToken");
            const myToken = await MyToken.deploy(1000);
            await myToken.deployed();

            expect((await myToken.totalSupply()).toString()).to.equal('1000');
            expect((await myToken.balanceOf(owner.address)).toString()).to.equal('1000');
          });
        });
        ```

### Step 3: Run the Tests

To execute your tests, run the following command in your project's root directory. This will run your written tests, confirming that your contract works as expected.
```shell
npx hardhat test
```

You should get a response like below:
![Test Success](/assets/img/guides/quickstart/hardhat/test-success.png)

By following these steps, you'll have the necessary testing frameworks installed and be well-prepared to write effective tests for your smart contract.

---
Now that you've tested your smart contract to ensure it works as expected, next, we'll [Deploy your Smart Contract](/guides/quickstart/hardhat/deploy-smart-contract/).
