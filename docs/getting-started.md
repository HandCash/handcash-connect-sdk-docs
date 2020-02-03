# Quick Start

## Install

Include `handcash-connect-sdk` in your NodeJS project:

```bash
npm i handcash-connect-sdk
```

## Your first payment

To start, you will need to create an instance of `CloudAccount`. This object allows you to interact with the SDK.

A `CloudAccount` requires the following:

- **application credentials**: `appId` and `appSecret`. Don't have them yet? [Get your app credentials.](https://github.com/HandCash/handcash-connect-sdk-nodejs/)
- **userAuthorizationToken**: It represents the authorization users grant to your application in order to access to their _HandCash Account_. [Find more about the user authorization process.](user-authorization.md)

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({
  appId: 'your appId',
  appSecret: 'your app secret',
  userAuthorizationToken: 'user authorization token'
});

const paymentParameters = {
  description = 'Hold my beer!🍺',
  payments: [
    { to: 'eyeone', currency: 'USD', amount: 0.25 },
    { to: 'eyeone@moneybutton.com', currency: 'EUR', amount: 0.05 },
    { to: 'eyeone@simply.cash', currency: 'SAT', amount: 50000 }
    { to: 'eyeone@relayx.io', currency: 'BSV', amount: 0.12345678 }
  ]
};

const paymentConfirmation = await cloudAccount.payments.pay(paymentParameters);
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```
