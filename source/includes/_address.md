# Address API

BlockCypher's Ethereum Address API allows you to look up information about public Ethereum addresses/accounts and generate your own low-value Ethereum addresses/public-private key pairs.

If you're new to blockchains, you can think of public addresses as similar to bank account numbers in a traditional ledger. The biggest differences:

- Anyone can generate a public address themselves (through [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) in Ethereum by default). No single authority is needed to generate new addresses; it's just public-private key cryptography.
- There are two kinds of addresses in Ethereum: external addresses and contract addresses. Contract addresses are controlled by embedded scripts within the Ethereum blockchain, while external addresses are controlled in a similar manner to Bitcoin.

## Address Balance Endpoint

```shell
curl -s https://api.blockcypher.com/v1/eth/main/addrs/738d145faabb1e00cf5a017588a9c0f998318012/balance
{
  "address": "738d145faabb1e00cf5a017588a9c0f998318012",
  "total_received": 9762206505909057760,
  "total_sent": 6916970312523512365,
  "balance": 2845236193385545395,
  "unconfirmed_balance": 0,
  "final_balance": 2845236193385545395,
  "n_tx": 702,
  "unconfirmed_n_tx": 0,
  "final_n_tx": 702
}
```

The Address Balance Endpoint is the simplest---and fastest---method to get a subset of information on a public address.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/balance | GET | [Address](#address)

ADDRESS is a *string* representing the public address you're interested in querying, for example:

`738d145faabb1e00cf5a017588a9c0f998318012`

The returned object contains information about the address, including its balance in wei and the number of transactions associated with it. The endpoint omits any detailed transaction information, but if that isn't required by your application, then it's the fastest and preferred way to get public address information.

## Address Endpoint

```shell
curl -s https://api.blockcypher.com/v1/eth/main/addrs/738d145faabb1e00cf5a017588a9c0f998318012
{
  "address": "738d145faabb1e00cf5a017588a9c0f998318012",
  "total_received": 9762206505909057760,
  "total_sent": 6916970312523512365,
  "balance": 2845236193385545395,
  "unconfirmed_balance": 0,
  "final_balance": 2845236193385545395,
  "n_tx": 702,
  "unconfirmed_n_tx": 0,
  "final_n_tx": 702,
  "txrefs": [
    {
      "tx_hash": "ba44f568dc77d33893261c041994eeaef17d108a1845ccb9e376afea2acdd0e9",
      "block_height": 1648312,
      "tx_input_n": 0,
      "tx_output_n": -1,
      "value": 50420000000000000,
      "ref_balance": 2845236193385545395,
      "confirmations": 8342,
      "confirmed": "2016-06-05T12:47:08Z",
      "double_spend": false
    },
    {
      "tx_hash": "99ec589ab8cb9498093c4d0c534bdcfd8bd04bfead1d869e3a79165a4f98af0d",
      "block_height": 1628712,
      "tx_input_n": 0,
      "tx_output_n": -1,
      "value": 10623553359437326,
      "ref_balance": 2895656193385545395,
      "confirmations": 27942,
      "confirmed": "2016-06-02T06:53:55Z",
      "double_spend": false
    },
    {
      "tx_hash": "20551685c6c311738da9f42019eb66fccd2cec4cc48e00d98967365d6ad77164",
      "block_height": 1628222,
      "tx_input_n": 0,
      "tx_output_n": -1,
      "value": 10623553359437326,
      "ref_balance": 2906279746744982721,
      "confirmations": 28432,
      "confirmed": "2016-06-02T05:02:50Z",
      "double_spend": false
    },
    ...
  ],
  "hasMore": true,
  "tx_url": "https://api.blockcypher.com/v1/eth/main/txs/"
}
```

The Address Endpoint returns more information about an address' transactions than the [Address Balance Endpoint](#address-balance-endpoint), but sacrifices some response speed in the process.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS | GET | [Address](#address)

Flag | Type | Effect
---- | ---- | ------
**before** | *integer* | Filters response to only include transactions below **before** height in the blockchain.
**after** | *integer* | Filters response to only include transactions above **after** height in the blockchain.
**limit** | *integer* | **limit** sets the minimum number of returned TXRefs; there can be less if there are less than **limit** TXRefs associated with this address, but there can be more in the rare case of more TXRefs in the block at the bottom of your call. This ensures paging by block height never misses TXRefs. Defaults to 200, maximum is 2000.
**confirmations** | *integer* | If set, only returns the **balance** and TXRefs that have *at least* this number of **confirmations**.

ADDRESS is a *string* representing the public address (or wallet/HD wallet name) you're interested in querying, for example:

`738d145faabb1e00cf5a017588a9c0f998318012`

The returned object contains information about the address, including its balance in wei, the number of transactions associated with it, and transaction summaries in descending order by block height.

## Address Full Endpoint

```shell
curl -s https://api.blockcypher.com/v1/eth/main/addrs/0x7CEcBd7a618146Cb251735b524e98f62d548177b/full
{
  "address": "0x7CEcBd7a618146Cb251735b524e98f62d548177b",
  "total_received": 613136037139846027,
  "total_sent": 609820639361600898,
  "balance": 3315397778245129,
  "unconfirmed_balance": 0,
  "final_balance": 3315397778245129,
  "n_tx": 58,
  "unconfirmed_n_tx": 0,
  "final_n_tx": 58,
  "nonce": 36,
  "pool_nonce": 36,
  "hasMore": true,
  "txs": [
    {
      "block_hash": "eec1dd4d7271a53ada4cf3145577d0610e8648aefa42a9420a7ab1d3ae9a787b",
      "block_height": 13113955,
      "block_index": 550,
      "hash": "5b199d5a3b3a3f9b2ffe37878bb3b428affbec117e7888202ba22503b3967a79",
      "addresses": [
        "7cecbd7a618146cb251735b524e98f62d548177b",
        "f9209e6809e0a08ae18b1366c437cb3cad2b6182"
      ],
      "total": 1550897349206251,
      "fees": 735000000000000,
      "size": 109,
      "gas_limit": 21000,
      "gas_used": 21000,
      "gas_price": 35000000000,
      "gas_tip_cap": 35000000000,
      "gas_fee_cap": 35000000000,
      "confirmed": "2021-08-28T12:41:39Z",
      "received": "2021-08-28T12:01:08.52Z",
      "ver": 0,
      "double_spend": false,
      "vin_sz": 1,
      "vout_sz": 1,
      "confirmations": 111695,
      "confidence": 1,
      "inputs": [
        {
          "sequence": 35,
          "addresses": [
            "7cecbd7a618146cb251735b524e98f62d548177b"
          ]
        }
      ],
      "outputs": [
        {
          "value": 1550897349206251,
          "addresses": [
            "f9209e6809e0a08ae18b1366c437cb3cad2b6182"
          ]
        }
      ]
    },
  ...,
]
}
```


The Address Full Endpoint returns all information available about a particular address, including an array of complete [transactions](#tx) instead of just transaction inputs and outputs. Unfortunately, because of the amount of data returned, it is the slowest of the address endpoints, but it returns the most detailed data record.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/full | GET | [Address](#address)

Flag | Type | Effect
---- | ---- | ------
**before** | *integer* | Filters response to only include transactions below **before** height in the blockchain.
**after** | *integer* | Filters response to only include transactions above **after** height in the blockchain.
**limit** | *integer* | **limit** sets the minimum number of returned TXs; there can be less if there are less than **limit** TXs associated with this address, but there can also be more in the rare case of more TXs in the block at the bottom of your call. This ensures paging by block height never misses TXs. Defaults to 10, maximum is 50.
**txlimit** | *integer* | This filters the TXInputs/TXOutputs within the returned TXs to include a maximum of **txlimit** items.
**confirmations** | *integer* | If set, only returns the **balance** and TXs that have *at least* this number of **confirmations**.
**confidence** | *integer* | Filters response to only include TXs above **confidence** in percent; e.g., if this is set to 99, will only return TXs with 99% confidence or above (including all confirmed TXs). For more detail on confidence, check the [Confidence Factor](#confidence-factor) documentation.
**includeHex** | *bool* | If *true*, includes hex-encoded raw transaction for each TX; *false* by default.
**includeConfidence** | *bool* | If *true*, includes the **confidence** attribute (useful for unconfirmed transactions) within returned TXs. For more info about this figure, check the [Confidence Factor](#confidence-factor) documentation.
**omitWalletAddresses** | *bool* | If **omitWalletAddresses** is *true* and you're querying a [Wallet](#wallet) or [HDWallet](#hdwallet), the response will omit address information (useful to speed up the API call for larger wallets).

ADDRESS is a *string* representing the public address (or wallet/HD wallet name) you're interested in querying, for example:

`0x7CEcBd7a618146Cb251735b524e98f62d548177b`

The returned object contains information about the address, including its balance in satoshis, the number of transactions associated with it, and the corresponding full transaction records in descending order by block height---and if multiple transactions associated with this address exist within the same block, by descending block index (position in block).

<aside class="notice">
If your returned <a href="#address">Address</a> object includes the <b>hasMore</b> attribute, there are more transactions associated with the address than transfered through this endpoint. If this happens, note the block height of the last transaction in the array, and then you can use the <b>before</b> flag to page through results.
</aside>

## Generate Address Endpoint

```shell
curl -sX POST https://api.blockcypher.com/v1/eth/main/addrs?token=YOURTOKEN
{
  "private": "44c984a089184a9a659e10210af360049f366a34f7f3cb253ac5cbfcb33f3d7c",
  "public": "04b7de9b47fc1fb278648266316dffe47cc7f5280b69b05b5770ad1624ee3da7647c98ade5e4c35b99e606535eaca54db0f913c8aa1fa7226f15b9996325d0989d",
  "address": "e3f7e628fff7589218d88ae1d6bcdac52fef2168"
}
```

The Generate Address endpoint allows you to generate private-public key-pairs along with an associated public address. No information is required with this POST request.

<aside class="success">
The private key returned is immediately discarded by our servers, but we advise that these keys should not be used for any high-value---or long-term storage---addresses.
</aside>

<aside class="warning">
Always use HTTPS for Address Generation requests. Otherwise, your generated private keys will be sent over insecure channels and could be MITM'd.
</aside>

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/addrs | POST | *nil* | [AddressKeychain](#addresskeychain)

The returned object contains a private key, a public key, and a public address, all hex-encoded.
