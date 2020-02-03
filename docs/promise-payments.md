# Promise Payments

Promise payments are similar to [instant payments](/payments.md). The difference is that promise payments allow you to commit the amount of money being spent and update the payment amount or data as time goes on. Eventually you can broadcast the promise to finalize the payment.

## Create a promise

To start, you will need to create a promise payment with some initial parameters.

Below is an example of how to create a promise payment to charge users as a stream service. It shows how to charge:
- A fixed amount of $0.02 for the stream service (service fee).
- A variable amount $0.05 for each minute of video for the creator of the video.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerMinute = 0.05;
const totalMinutesWatched = 1;
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
    amount: totalMinutesWatched * usdPerMinute
  }
];
const promisePaymentId = await cloudAccount.payments.promisePayment(
    { description, payments }
);
```

You may use the `promisePaymentId` to [claim a promise](#claim-a-promise) anytime.

## Update a promise

You can change any parameter of the promise payment anytime. You can change the amounts of the payments, add more payments, add or change the data attached...

Keeping with the example of the stream service, this snippet shows how to update the amount that goes to the video creator, depending on the seconds of video watched.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerMinute = 0.05;
const totalMinutesWatched = 5;
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
    amount: totalMinutesWatched * usdPerMinute
  }
];
await cloudAccount.payments.updatePromisePayment(
    promisePaymentId, {description, payments}
);
const paymentId = await cloudAccount.payments.claimPromisePayment(
  promisePaymentId
);
```

## Claim a promise

Eventually, you will have to close the agreement and claim the promise, so that the recipients of the payment receive their funds.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const paymentId = await cloudAccount.payments.claimPromisePayment(
  promisePaymentId
);
```

> **Note:** once you claim a promise payment you will no longer be able to update. In any case, you may create a new promise payment.
