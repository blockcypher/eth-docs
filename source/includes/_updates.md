# Updates

This section list all the updates in reverse chronological order. All dates are in the DD/MM/YY format.

**24/01/22 - Add ERC-20 Deployement on BETH Example**

We have added an example showing you how to deploy an ERC-20 on our Ethereum testnet: BETH.

**29/10/21 - Add Priority Fee Estimation**

We are now estimating the priority fee. Soon, transactions created with the API will be only EIP-1559 transactions.

**18/10/21 - Fix Unconfirmed Transactions on Address Full Endpoint**

Previously, when using the address full endpoint with `confirmations>0`, you'd still get unconfirmed transactions. This is fixed.

**14/10/21 - Fix Websocket Origin Check**

We fixed an error where using blockcypher websocket would returns a 403.

**12/10/21 - Allow Multiple Transaction Replacement**

Previously, a hard limit of 2 two transactions with the same nonce was in place. We have removed this limitation.

**14/09/21 - Documentation for Address Full Endpoint**

We've added some documentation for the [Address Full Endpoint](#address-full-endpoint) and fixed some hyperlinks.

**06/08/21 - New Filters for Uncofirmed Transactions Endpoint**

We've added new filters possibilities to the [Unconfirmed Transactions Endpoint](#unconfirmed-transactions-endpoint).

**04/02/21 - Creation of the "Updates" Section**

We've created this new "Updates" section to let you know about the new feature and bugfixes we deployed :).

**04/02/21 - Ethereum Transaction Cancellation/Replacement and Bugfixes**

You can now cancel and replace Ethereum with our Transaction API. See the [Creating Transactions part](#creating-transactions) for more information. 
Alongside, when creating a new Ethereum transaction, the available balance is now the pool balance and not the confirmed balance.

**27/11/20 - Support for OpenZeppelin ERC20 Token**

Introduce limited support to contract with fallback. For now, we only allow calls on the USDC contract. Send us a message if you need another token to be included.

**04/11/20 - Dynamic Fee for Payment Forwards**

Payment Forwards now use our dynamic fees. This should ensure that your forwards are delivered as soon as possible.

**15/10/20 - Ethereum Payment Forwards**

Finally! Ethereum payment forward are now deployed. We are working hard to add this feature to ERC20 tokens as well.
