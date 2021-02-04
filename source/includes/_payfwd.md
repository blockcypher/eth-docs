# Payment Forwarding

One of the well-known benefits of cryptocurrency is the ability to allow users to partake in online commerce without necessarily requiring extensive setup barriers, like registering new accounts. In that spirit, our Payment Forwarding API is the easiest way to accept---and consolidate---payments securely without forcing your users to create accounts and jump through unnecessary loops. It's also a generic way to automatically transfer value from one address to another. While there are many possible use cases, the main one is a way to generate payment-specific addresses for which funds will automatically transfer to a main merchant address. Great for automatic merchandise (whether physical or virtual) processing.

We do not take a fee on payment forwarding, other than the required 1,680,000 wei miner fee (default 80 gwei gas price with 21000 gas limit); which can be changed with the `gas_price_gwei` parameter.

<aside class="notice">
Payment forwarding is only available for paid plans with an active token, you can't create a payment forwarding request without one. You can <a href="http://accounts.blockcypher.com">register for a token and pay for a plan here.</a>
</aside>

<aside class="notice">
Note that <b>mining fees</b> are deducted from any user-set <b>processing fees</b> first. This can often lead to lower processing fees than expected.
</aside>

<aside class="warning">
By default, all payments will be debited with a minimum mining fee. The amount of the fee is configurable. But for very small payments, if the amount sent is even lower than the mining, the forward will fail. By default the mining fee is computed by multiplying the current <code>high_fee</code> with the <code>gas_limit</code> of 21000 for a standard transaction.

</aside>

## Create Payment Endpoint

```shell
curl -d '{"destination":"0xda4b58eff2f3fd6c9845f82268cac1115b9b446b","callback_url": "https://my.domain.com/callbacks/new-pay"}' https://api.blockcypher.com/v1/eth/main/payments?token=YOURTOKEN

{
  "id": "82f0af68-da11-477f-8b42-79edbf3e6481",
  "token": "YOURTOKEN",
  "destination": "da4b58eff2f3fd6c9845f82268cac1115b9b446b",
  "input_address": "718d2ab6160cf79cab1815dcc87d6b76fc20a0ba",
  "callback_url": "https://my.domain.com/callbacks/new-pay"
}
```

First, to create an payment forwarding address, you need to POST a partially filled [PaymentForward](#paymentforward) object to the payment creation endpoint. You need to include at least a **destination** address and your **token**; optionally, you can add a **callback_url**, a **gas_price_gwei** and a few other options. You can see more details about these options in the [PaymentForward](#paymentforward) object details.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/payments | POST | [PaymentForward](#paymentforward) | [PaymentForward](#paymentforward)

In return, you'll get a more complete [PaymentForward](#PaymentForward) object, including an **input_address** and **id**.

<aside class="notice">
If you decide to have a <b>callback_url</b>, you'll receive a payload at that url whenever a payment is made to the <b>input_address</b>. The payload will come in the form of a <a href="#paymentforwardcallback">PaymentForwardCallback</a> object.
</aside>

## List Payments Endpoint

```shell
curl https://api.blockcypher.com/v1/eth/main/payments?token=YOURTOKEN

[
  {
    "id": "82f0af68-da11-477f-8b42-79edbf3e6481",
    "token": "YOURTOKEN",
    "destination": "da4b58eff2f3fd6c9845f82268cac1115b9b446b",
    "input_address": "718d2ab6160cf79cab1815dcc87d6b76fc20a0ba",
    "callback_url": "https://my.domain.com/callbacks/new-pay"
  },
]
```

To list your currently active payment forwarding addresses, you can use this endpoint.

Resource | Method | Return Object
-------- | ------ | -------------
/payments | GET | Array[[PaymentForward](#paymentforward)]

Flag | Type | Effect
---- | ---- | ------
**start** | *integer* | Returns list of payment forwards starting at the **start** index; useful for paging beyond the limit of 200 payment forwards.

This returns the full array of your currently active payment forwarding addresses, based on your token. By default, this endpoint only returns the first 200 payment forwards. If you have more, you can page through them using the optional **start** parameter.

## Delete Payment Endpoint

```shell
# Piping to grep to just show status code
curl -X DELETE -Is https://api.blockcypher.com/v1/eth/main/payments/82f0af68-da11-477f-8b42-79edbf3e6481?token=YOURTOKEN | grep "HTTP/1.1"

HTTP/1.1 204 No Content
```

When you're done with a payment forwarding address, you can delete it via its id.

Resource | Method | Return Object
-------- | ------ | -------------
/payments/$PAYID | DELETE |  *nil*

PAYID is a string representing the payment forwarding request you want to delete, for example:

`82f0af68-da11-477f-8b42-79edbf3e6481`

This will return no object, but will return an HTTP Status Code 204 if the request was successfully deleted.
