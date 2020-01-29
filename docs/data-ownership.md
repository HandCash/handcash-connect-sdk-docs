# Data Ownership

Each HandCash user has their own identity. You can use this identities to make users owners of the information they produce: social media content, blogs, videos.

You can use this identity to sign any piece of information and make the user the owner of it.

> **Explanation.** The signatures are stored in the blockchain which acts as a timestamp server. So, if user A signs "this is my comment" and also 5 minutes later user B signs the same message, the blockchain will tell us who is the actual owner of the content.

## Sign Data

```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const ownedData = '❤️ you all';
const dataHash = crypto.getStringHashAsHex(ownedData);
const dataSignature = await cloudAccount.signing.sign(dataHash);
console.log(dataSignature);
// {
//      "signatureAsDer": "30440220683f3c385edc42223c4efa00493c34532379ac88c1aafa57997d903de49c7e9302202175d70f029515f0013c1d10de82b116984c7511f3f52928470ddfb495059100",
//      "dataHashAsHex": "eab568fc68c2d87b03aa8d5ab388278b5df83fd8f82cee2e0ba6446bcebf8363"
// }
```

## Verify Ownership

On the other hand, there is a way to validate ownership of data (signed data).

This snippet shows how to validate a signature for the current user cloud account:
```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const ownedData = "❤️ you all"
const dataHash = crypto.getStringHashAsHex(ownedData);
const signatureAsDer = '30440220683f3c385edc42223c4efa00493c34532379ac88c1aafa57997d903de49c7e9302202175d70f029515f0013c1d10de82b116984c7511f3f52928470ddfb495059100'
const isValid = await cloudAccount.signing.isValidSignature(dataHash, signatureAsDer);
console.log(isValid);
// true
```
