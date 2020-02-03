# Examples

## **Case #1**: Charge \$0.05 for each minute of video

We can use [Promised Payments](/promised-payments.md) to charge users per minute of content and claim the promise at the end of the playback.

In this we show how to create a promised payment to charge users dynamically per each minute of video we provide. It shows how to charge:

- A fixed amount of \$0.02 for the streaming service (service fee).
- A variable amount \$0.05 for each minute of video for the creator of the video.

1. We define the initial payment as well as the receipt to be attached to the payment.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerMinute = 0.05;
const totalFragmentsWatched = 1;
const description = 'Watch video #312195128';
const receipt = {
    id: '086b8346523f912ca1200b',
    videoId: '1828172081',
    title: 'Interview with Ihsotas Otomakan',
    totalSecondsPurchased: 60 * totalFragmentsWatched
};
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
{ description, payments, receipt }
);
```

2. We update the promised payment as we deliver more content to the user.

`totalFragmentsWatched` goes from `1` to `8`.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerMinute = 0.05;
const totalFragmentsWatched = 8;
const description = 'Watch video #312195128';
const receipt = {
    id: '086b8346523f912ca1200b',
    videoId: '1828172081',
    title: 'Interview with Ihsotas Otomakan',
    totalSecondsPurchased: 60 * totalFragmentsWatched
};
const payments = [
  {
      to: 'stream_service_handle',
      currency: 'USD',
      amount: service_fee
  },
  {
    to: 'video_creator_handle',
    currency: 'USD',
    amount: totalFragmentsWatched * usdPerMinute
  }
];
const promisedPaymentId = await cloudAccount.payments.promisePayment(
{ description, payments, receipt }
);
```

3. Once users sessions have ended and we consider they won't keep watching the video, we can claim the promise.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const paymentId = await cloudAccount.payments.claimPromisedPayment(
  promisedPaymentId
);
```

## **Case #2**: Charge to publish content in a social media app

```javascript
// TODO
```

## **Case #3**: --

```javascript
// TODO
```
