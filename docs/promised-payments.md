# Promised Payments

Promised payments are similar to [instant payments](/payments.md). The difference is that with promised payment you can commit the money to be spent and additionally update the payment, changing parameters as amounts or data attached. Eventually you claim the promise to make the payment effective.

## Create a promise

First of all, you need to create a promised payment with some initial parameters.

This is an example of how to create a promised payment to charge users as a stream service. It shows how to charge:
- A fixed amount of $0.02 for the stream service (service fee).
- A variable amount $0.05 for each 30 seconds of video for the creator of the video.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPer30Seconds = 0.05;
const totalFragmentsWatched = 1;
const description = 'Watch video #312195128';
const payments = [
  { 
      to: 'stream_service_handle', 
      currency: 'USD', 
      amount: service_fee
  },
  {
    to: 'video_creator_handle',
    currency: 'USD',
    amount: totalFragmentsWatched * usd_per_30_seconds
  }
];
const promisedPaymentId = await cloudAccount.payments.promisePayment(
    { description, payments }
);
```

You can use the `promisedPaymentId` to [claim a promise](#claim-a-promise) anytime.

## Update a promise

You can change any parameter of the promised payment anytime. You can change the amounts of the payments, add more payments, add or change the data attached...

Keeping with the example of the stream service, this snippet shows how to update the amount that goes for the video creator depending on the seconds of video watched.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPer30Seconds = 0.05;
const totalFragmentsWatched = 5; // 30 seconds * 5 = watched until second 150
const description = 'Watch video #312195128';
const payments = [
  { 
      to: 'stream_service_handle', 
      currency: 'USD', 
      amount: service_fee
  },
  {
    to: 'video_creator_handle',
    currency: 'USD',
    amount: totalFragmentsWatched * usd_per_30_seconds
  }
];
await cloudAccount.payments.updatePromisedPayment(
    promisedPaymentId, {description, payments}
);
const paymentId = await cloudAccount.payments.claimPromisedPayment(
  promisedPaymentId
);
```

## Claim a promise

Eventually, you have to close the agreement and claim the promise. So, the recipients of the payment receive the funds inmediately.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const paymentId = await cloudAccount.payments.claimPromisedPayment(
  promisedPaymentId
);
```

> **Notice** that once you claim the payment you can't update it anymore. In any case, you should create a new promised payment.