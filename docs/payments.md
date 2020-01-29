# Instant Payments

HandCash allows you to pay to anyone in the world instantly and without intermediaries.

## Simple Payment

This is an example of the simplest payment you can make using Connect:

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const description = 'Hold my beer!🍺';
const payments: [{ to: 'eyeone', currency: 'USD', amount: 0.25 }];

const paymentConfirmation = await cloudAccount.payments.pay({description, payments});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```

## Pay to multiple people

You can add up to 200 people to the same payment. Also you can reference them using:

- **Handles** (eyeone): recommended to send between HandCash users.
- **Paymails** (name@moneybutton.io, name@relayx.io, etc...): recommended to send to other services.
- **P2SH Addresses**: recommended for very custom transactions.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const description = 'You guys rock!😎';
const payments: [
    { to: 'eyeone', currency: 'USD', amount: 0.25 },
    { to: 'eyeone@moneybutton.com', currency: 'EUR', amount: 0.25 },
    { to: '131xrWSKXHbhucFPTfZqnxF8ZhjpMxJH7K', currency: 'SAT', amount: 50000 }
];

const paymentConfirmation = await cloudAccount.payments.pay({description, payments});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```

## Supported Currencies

We support the following local currencies:

| Currency Code | Currency Name        |
| ------------- | -------------------- |
| ARS           | Argentinian Peso     |
| AUD           | Australian Dollar    |
| BRL           | Brazilian Real       |
| CAD           | Canadian Dollar      |
| CHF           | Swiss Franc          |
| CNY           | Chinese Yuan         |
| COP           | Colombian Peso       |
| CZK           | Czech Koruna         |
| DKK           | Danish Krone         |
| EUR           | Eurozone Euro        |
| GBP           | British Pound        |
| HKD           | Hong Kong Dollar     |
| JPY           | Japanese Yen         |
| KRW           | Korean Won           |
| MXN           | Mexican Peso         |
| NOK           | Norwegian Krone      |
| NZD           | New Zealand Dollar   |
| PHP           | Philippine Peso      |
| RUB           | Russian Ruble        |
| SEK           | Swedish Krona        |
| SGD           | Singapore Dollar     |
| THB           | Thai Baht            |
| USD           | United States Dollar |
| ZAR           | South African Rand   |

<br>

Additionally we support the following Bitcoin SV currencies:

| Currency Code     | Currency Name | Description                    |
| ----------------- | ------------- | ------------------------------ |
| SAT               | satoshis      | Indivisible unit               |
| BSV               | bitcoins      | 1 bitcoin = 100000000 satoshis |
| DUR (Recommended) | duros         | 1 duro = 5000 satoshis         |

## Attach Data

In addition to transfer money between people you can attach data to payments. This data is inmutable: stored forever and with no chance to modify a single bit.

Attaching data into a payment improves the transparency and auditability in the exchange of money for information. These are some use cases.

### Content creators

Content creators have a way to proof they are the original creators of the content. One way to build such a proof is **to sign the hash of the content with the user identity keys**. Notice that you actually don't need to store the content to proof the ownership, just the hash is enough.

As this signature is stored in a inmutable system and in cronologycal order, any possible fraud as someone trying to store a new signature of the content with their identity keys will be stored after the signature of the original creator. 

> **Notice** that any proof of fraud will be also stored immutably.

```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const userComment = 'HandCash Connect SDK is awesome!';

const description = 'Post a comment';
const payments: [{ to: 'service.handle', currency: 'USD', amount: 0.05 }];
const data = { type: 'sign', rawDataAsHex: crypto.getStringHashAsHex(userComment) };

const paymentConfirmation = await cloudAccount.payments.pay({description, payments, data});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```

### Content providers

### Simple Storage
