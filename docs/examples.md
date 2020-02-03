# Examples

## **Case #1**: Charge \$0.05 for each minute of video

We can use [Promise Payments](/promise-payments.md) to charge users per minute of content and claim the promise at the end of the playback.

In this we show how to create a promise payment which charges users dynamically per each minute of video we provide. Below shows an example of two types of charges:

- A fixed amount of \$0.02 for the streaming service (service fee).
- A variable amount \$0.05 for each minute of video for the creator of the video.

**1) We define the initial payment as well as the receipt to be attached to the payment.**

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerFragment = 0.05;
const totalFragmentsWatched = 1;
const totalSecondsPerFragments = 60;
const description = 'Watch video #312195128';
const receipt = {
    id: '086b8346523f912ca1200b',
    videoId: '1828172081',
    title: 'Interview with Ihsotas Otomakan',
    totalSecondsPurchased: totalSecondsPerFragments * totalFragmentsWatched
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
    amount: totalFragmentsWatched * usdPerFragment
  }
];
const promisePaymentId = await cloudAccount.payments.promisePayment(
{ description, payments, receipt }
);
```

**2) We update the promise payment as we deliver more content to the user.**

?> We just need to change `totalFragmentsWatched` from `1` to `8`.

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const usdServiceFee = 0.02;
const usdPerFragment = 0.05;
const totalFragmentsWatched = 8;
const totalSecondsPerFragments = 60;
const description = 'Watch video #312195128';
const receipt = {
    id: '086b8346523f912ca1200b',
    videoId: '1828172081',
    title: 'Interview with Ihsotas Otomakan',
    totalSecondsPurchased: totalSecondsPerFragments * totalFragmentsWatched
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
    amount: totalFragmentsWatched * usdPerFragment
  }
];
await cloudAccount.payments.updatePromisePayment(
    promisePaymentId, {description, payments, receipt}
);
```

**3) Once the user's session has ended and we are sure they are done watching the video, we can claim the promise.**

```javascript
const { HandCashCloudAccount } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const paymentId = await cloudAccount.payments.claimPromisePayment(
  promisePaymentId
);
```

## **Case #2**: Social media app with micropayments

You can use HandCash Connect to build a social media app that rewards content creators with instant payments for each interaction.

**1) A user pays to create a post in the app.**

```javascript
const { HandCashCloudAccount, Data } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const userContent = 'HandCash Connect SDK is awesome!';

const description = 'Create a post';
const payments: [{ to: 'service.handle', currency: 'USD', amount: 0.05 }];
const data = Data.fromObject({
    id: '27a7c7c77102de901921',
    type: 'post',
    content: userContent
}).sign();

const paymentConfirmation = await cloudAccount.payments.pay({description, payments, data});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```

**2) A user likes a post and pays both the platform and the creator of the post.**

```javascript
const { HandCashCloudAccount, Data } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const description = 'Like a post';
const payments: [
  { to: 'service.handle', currency: 'USD', amount: 0.01 }
  { to: 'post.creator.handle', currency: 'USD', amount: 0.01 }
];
const data = Data.fromObject({
    id: '76c100aeca0127127172',
    type: 'like'
    referencedPostId: '27a7c7c77102de901921'
}).sign();

const paymentConfirmation = await cloudAccount.payments.pay({description, payments, data});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```

**3) A user comments a post and pays the platform.**

```javascript
const { HandCashCloudAccount, Data } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const userContent = 'This is a game changer!';

const description = 'Like a post';
const payments: [{ to: 'service.handle', currency: 'USD', amount: 0.03 }];
const data = Data.fromObject({
    id: '18c81001a09e816381aa',
    type: 'comment'
    referencedPostId: '27a7c7c77102de901921'
    content: userContent
}).sign();

const paymentConfirmation = await cloudAccount.payments.pay({description, payments, data});
console.log(paymentConfirmation);
// {
//      transactionId: "8db02465200a0aec733f046b86b0ee0847e66d7cd451e198b25c493346ca4601",
//      date: "2019-01-20T18:23:02.412Z"
// }
```
