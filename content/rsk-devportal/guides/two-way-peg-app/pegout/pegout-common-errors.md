---
menu_order: 1700
menu_title: Troubleshooting and Common Errors
title: "Common errors when using the app | 2 way peg app Documentation"
description: "Here, we will learn how to perform a peg-out using Metamask Wallet."
tags: 2 way peg, powpeg, peg-in, peg-out, 2way-peg, bridge, rbtc, btc, testnet, mainnet, trezor, liquality, leger, guide, setup, integrate, use
layout: rsk
render_features: 'collapsible'
---

![2 way peg app (peg-out)](/assets/img/guides/two-way-peg-app/pegout.gif)

You may encounter the following errors when trying out the application:

[](#top "collapsible")
- Liquality 
    > **Liquality is not showing BTC Address:** 
    > ![Not Showing BTC Address](/assets/img/guides/two-way-peg-app/liquality/common-errors/liquality-no-address.png)
    > - Problem: Liquality is not showing the BTC address.
    > - Possible Fix: Some users have encountered difficulties while attempting to create new Bitcoin accounts in Liquality. It’s   important to note that this issue is unrelated to the 2WP app. If you experience this problem, we recommend selecting another wallet.
    > **User Rejected Error:** 
    > ![User Rejected error](/assets/img/guides/two-way-peg-app/liquality/common-errors/1-common-errors.png)
    > - Problem: This error occurs when the user starts connecting its wallet and for some reason cancels it.
    > - Possible Fix: See fix below
    > **Error when connecting**
    > ![Error connecting](/assets/img/guides/two-way-peg-app/liquality/common-errors/2-common-errors.png)
    > - Problem: This error occurs if you have MetaMask and Liquality enabled in your browser. 
    > ![MetaMask and Liquality enabled](/assets/img/guides/two-way-peg-app/liquality/common-errors/3-common-errors.png)
    > - Cases: This also happens if another activity is been carried out, outside the connecting process when the connection is happening (for example, trying to disable the Liquality wallet extension in the middle of the connection).
    > - To fix both errors: 
    > - * Go to manage chrome extensions, and disable any other wallet extension, 
    > ![Disable extensions](/assets/img/guides/two-way-peg-app/liquality/common-errors/4-common-errors.png)
    > - * Disable Liquality, and  enable it
    > ![Enable Liquality](/assets/img/guides/two-way-peg-app/liquality/common-errors/5-common-errors.png)
    > - * Refresh the 2 way peg app.
----

## Resources
* 2 way peg app frontend [repo](https://github.com/rsksmart/2wp-app)
* 2 way peg app backend [repo](https://github.com/rsksmart/2wp-api)
* How to get [RBTC using Rootstock’s built in Powpeg](/guides/get-crypto-on-rsk/powpeg-btc-rbtc/)
* [Rootstock Testnet Faucet](https://faucet.rootstock.io/)
* [Get RBTC using Exchanges](/guides/get-crypto-on-rsk/rbtc-exchanges/)
* [Design architecture](/guides/two-way-peg-app/advanced-operations/design-architecture/)