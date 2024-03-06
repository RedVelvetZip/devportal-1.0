---
menu_order: 600
menu_title: Interact with the Front-end
layout: rsk
title: 'Interact with the Front-end'
description: 'Learn how to integrate your Rootstock smart contract with front-end applications'
tags: quick-start, getting-started, guide, how-to, bitcoin, rsk, rootstock, blockchain
render_features: 'collapsible'
---

Creating a user-friendly web interface for smart contracts on the Rootstock network can enhance user interaction. Here, we'll focus on using `ethers.js`, a popular Ethereum library, for connecting your smart contracts to a web front-end.

## Project Setup

1. Create a new folder called `frontend` and navigate to the directory:
    
```shell
  mkdir frontend
  cd frontend
```
> However, if you clone the master branch, there's already a frontend folder, just `cd` into the frontend directory

2. In the frontend directory, initialize a Node.js Project:

```shell
  npm init -y
```

3. Install Ethers.js:

```shell
  npm install --save ethers
```

## Create HTML File

[](#top "collapsible")
- Update HTML File
    - In the frontend directory, open the `index.html` file:
      - Copy the code snippet below and paste it in your html file:
        ```html
        <!DOCTYPE html>
        <html lang="en">

        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Web3 App with Ethers.js</title>
        </head>

        <body>
        </body>

        </html>
        ```
- Import Ethers
   - To import the Ethers library to interact with the wallet to the network, copy the code snippet below and paste it in the `<head>` section of your html file:
      ```html
        <script src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js"></script>
      ```
- Create HTML elements inside the body
  1. Create a button to trigger the function for connecting the wallet.
  2. Create a button to trigger the function to get balance.
  3. Create a div element to show the answer for the address connected.
  4. Create a div element to show the answer for wallet balance.
      ```html
      <body>
        <div>
          <h1>Connect to Rootstock Network</h1>
          <button id="connectButton">Connect Wallet</button>
          <button id="getBalanceButton" disabled>Get MTK Balance</button>
          <div id="walletAddress"></div>
          <div id="walletBalance"></div>
        </div>
      </body>
      ```
- Import javascript file
  - Finally, to import the javascript library that we will create in a further step, copy the code snippet below and paste it in the `<body>` section of your html file::
    ```html
      <script src="app.js"></script>
    ```

Your `index.html` file should now look like the [`index.html` file](https://raw.githubusercontent.com/rsksmart/rootstock-quick-start-guide/feat/complete/frontend/index.html) on GitHub.

## Create JavaScript Functions

[](#top "collapsible")
- Create basic javascript function
  1. In the frontend directory, open the `app.js` file.
  2. Create the function to wait until the DOM is loaded, instance the HTML elements (buttons and divs), and declare some variables:
      ```js
      document.addEventListener('DOMContentLoaded', function () {
        const connectButton = document.getElementById('connectButton');
        const getBalanceButton = document.getElementById('getBalanceButton');
        const walletAddressDiv = document.getElementById('walletAddress');
        const walletBalanceDiv = document.getElementById('walletBalance');

        let provider, account, myTokenContract;
        let contractABI = [];
        // Replace with your contract's address
        const contractAddress = '0xa6fb392538BaC56e03a900BFDE22e76C05fb5122';
      });
      ```
- Add a function that fetches the ABI and stores it in a variable
    ```js
      async function fetchABI() {
        // Place the token file generated after compiling the contracts
        const response = await fetch('MyToken.json');
        const data = await response.json();
        contractABI = data.abi;
      }
    ```
- Add a function that checks the wallet is connected to the Rootstock network
    ```js
      async function checkNetwork() {
        const networkId = await window.ethereum.request({ method: 'net_version' });
        if ((networkId === "30") || (networkId === "31")) {
          return true;
        } else {
          return false;
        }
      }
    ```
- Call the fetchABI function that loads the ABI and connects the wallet to the network
    ```js
      fetchABI().then(() => {
        // Connect button
        connectButton.addEventListener('click', async function () {
          const correctNetwork = await checkNetwork();
          if (!correctNetwork) {
            alert('Select the Rootstock Network on the Metamask wallet');
            return
          }
          if (typeof window.ethereum !== 'undefined') {
            try {
              const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
              account = accounts[0];
              walletAddressDiv.innerHTML = `Connected account: ${account}`;
              provider = new ethers.providers.Web3Provider(window.ethereum);
              signer = provider.getSigner();
              myTokenContract = new ethers.Contract(contractAddress, contractABI, signer);
              getBalanceButton.disabled = false;
            } catch (error) {
              console.error("Error connecting to MetaMask", error);
              walletAddressDiv.innerHTML = `Error: ${error.message}`;
            }
          } else {
            walletAddressDiv.innerHTML = 'Please install MetaMask!';
          }
        })});
    ```
- Add a function responding to the click event on the get balance button.
    ```js
        // Get Balance Button
        getBalanceButton.addEventListener('click', async function () {
          if (myTokenContract) {
            const balance = await myTokenContract.balanceOf(account);
            walletBalanceDiv.innerHTML = `MyToken Balance: ${balance} MTK`;
          }
        });
    ```
- View the Complete Code
  - [GitHub Link](https://raw.githubusercontent.com/rsksmart/rootstock-quick-start-guide/feat/complete/frontend/app.js)
    ```js
      document.addEventListener('DOMContentLoaded', function () {
      // Instantiating HTML elements
      const connectButton = document.getElementById('connectButton');
      const getBalanceButton = document.getElementById('getBalanceButton');
      const walletAddressDiv = document.getElementById('walletAddress');
      const walletBalanceDiv = document.getElementById('walletBalance');
      // Instantiating variables
      let provider, account, myTokenContract;
      let contractABI = [];
      let networks = {};
      const contractAddress = `Replace with your contract's address`;

      /**
     * Load data from external JSON files
    */
      async function fetchExternalFiles() {
        // Place MyToken.json generated in artifacts after compiling the contracts
        let response = await fetch('MyToken.json');
        const data = await response.json();
        contractABI = data.abi;
        // Place networks.json to set the network automatically with the checkNetwork() function
        // You can set it manually instead following this guide https://dev.rootstock.io/kb/rootstock-metamask/
        response = await fetch('networks.json');
        networks = await response.json();
      }

      /**
    * Check and set network automatically in case it is not already done
    */
      async function checkNetwork() {
        try {
          // Make sure Metamask is installed
          if (!window.ethereum){
            alert('Please install Metamask!');
            return;
          }
          // Switch network
          await window.ethereum.request({
            method: 'wallet_switchEthereumChain',
            params: [{ chainId: networks.rskTestnet.chainId }],
          });
        } catch (error) {
          // This error code indicates that the chain has not been added to Metamask
          if (error.code === 4902) {
            // Trying to add new chain to Metamask
            await window.ethereum.request({
              method: 'wallet_addEthereumChain',
              params: [networks.rskTestnet],
            });
          } else {
            // Rethrow all other errors
            throw error;
          }
        }
      }

      // Get the required data and set the events
      fetchExternalFiles().then(() => {
        // Connect button event
        connectButton.addEventListener('click', async function () {
          // Check the network is set properly
          await checkNetwork();
          if (typeof window.ethereum !== 'undefined') {
            try {
              // Get the account from Metamask
              const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
              account = accounts[0];
              // Update the front with the account address
              walletAddressDiv.innerHTML = `Connected account: ${account}`;
              // Get the network provider
              provider = new ethers.providers.Web3Provider(window.ethereum);
              // Get the signer for network interaction
              signer = provider.getSigner();
              // Instantiate the contract
              myTokenContract = new ethers.Contract(contractAddress, contractABI, signer);
              // Activates the getBalanceButton
              getBalanceButton.disabled = false;
            } catch (error) {
              console.error("Error connecting to MetaMask", error);
              walletAddressDiv.innerHTML = `Error: ${error.message}`;
            }
          } else {
            walletAddressDiv.innerHTML = 'Please install MetaMask!';
          }
        });

        // Get balance button event
        getBalanceButton.addEventListener('click', async function () {
          // Check if the contract is instatiated properly
          if (myTokenContract) {
            // Obtains the user balance
            const balance = await myTokenContract.balanceOf(account);
            // Show the user balance
            walletBalanceDiv.innerHTML = `MyToken Balance: ${balance} MTK`;
          }
        });
      });
    });
    ```

### Run the frontend

To run the frontend, execute a local web server to test the HTML file using the following command:
```shell
npx http-server
```

Navigate to the URL: [http://127.0.0.1:8080](http://127.0.0.1:8080) to test the code in the browser and you should get a result similar to the image below:
![Smart Contract Frontend](/assets/img/guides/quickstart/hardhat/frontend.png)

> - Tips: Ensure local hardhat network is running. Run `npx hardhat node` in the root directory to start the local network. See section  on [Troubleshooting and Common Errors](/guides/quickstart/hardhat/debugging-and-troubleshooting/) to fix common errors.
> - Note: You can view and run the complete project from the [`feat/complete` branch](https://github.com/rsksmart/rootstock-quick-start-guide/tree/feat/complete). To do so, git checkout into the `feat/complete` branch, run `cd frontend`, run `npm install`, then run `npx http-server` to view and interact with the smart contract from the frontend.

----

## Resources

These tools are specifically tailored for Web3 development, and they can simplify the integration of blockchain functionaity into web interfaces. Here are a few recommended tools and libraries that are popular in the Web3 space, along with brief descriptions:

[](#top "collapsible")
- RainbowKit
  - [RainbowKit](https://www.rainbowkit.com/) is a React library offering a comprehensive wallet connection solution. It provides a beautiful, easy-to-use wallet connection interface that supports multiple wallets and networks.
  - **Why Use It:** 
    It is great for projects where you want a seamless and user-friendly wallet connection experience. It's easy to integrate and manage, especially in React-based applications.
- Web3Modal
  - [Web3Modal](https://web3modal.com/) is a JavaScript library that provides a simple, unified wallet connection modal for Web3 applications. It supports various wallet providers and can be used with different Web3 libraries.
  - **Why Use It:** 
    If you need to start using React or want a framework-agnostic wallet connection solution, Web3Modal is an excellent choice. It’s customizable and works well with both web3.js and ethers.js.
- Wagmi
  - [Wagmi](https://wagmi.sh/) is a React Hooks for Ethereum set that simplifies interactions with ethers.js. It provides hooks for wallet connection, contract interaction, balances, and more.
  - **Why Use It:** 
    For React developers who prefer a hooks-based approach, Wagmi offers an elegant way to integrate Ethereum functionality. It makes managing state and blockchain interactions more intuitive.
- Moralis
  - [Moralis](https://moralis.io/) is a fully managed backend platform for Web3 and blockchain applications. It offers a suite of tools for authentication, real-time databases, cloud functions, and syncing blockchain data.
  - **Why Use It:** 
    It can be a time-saver to build a more comprehensive application with backend support. It handles much of the backend complexity and lets you focus on front-end development.
