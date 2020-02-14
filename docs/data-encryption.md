# Data Encryption

Each HandCash user has their own identity. You can use these identities to encrypt any piece of information.

> This module is useful in case you need the user identity to encrypt data that is not stored in the blockchain. If you want to store the encrypted data in the blockchain, please refer to [attach data to payments](/payments.md#attach-data).

## User-to-User

In the user-to-user encryption schema User A sends a message to User B. It allows the following participants to decrypt the messages:
- User A.
- User B.
- App.

> This encryption method uses a combined ECDH schema to allow 3 different participants to access to the information.

This snippet shows how to encrypt a message between two users:
```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const message = Buffer.from('Hey dude!');
const encryptedMessage = await cloudAccount.encryption.encryptToUser(message, 'otherUserHandle');
console.log(encryptedMessage.toString('hex'));
// "424945311f9befac925c0d86f053011102a575250ce8d8f3a7b713b4af1058e6bfc8e48e6c33dd55c7bfa7af50706a519c75ecc5"
```

On the other hand, you can decrypt a message just by doing:
```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const encryptedMessage = Buffer.from('424945311f9befac925c0d86f053011102a575250ce8d8f3a7b713b4af1058e6bfc8e48e6c33dd55c7bfa7af50706a519c75ecc5', 'hex');
const message = await cloudAccount.encryption.decryptFromUser(encryptedMessage, 'otherUserHandle');
console.log(message.toString('hex'));
// "Hey dude!"
```

## User-to-App

User-to-App is an schema that allows both the user and the app to decrypt messages.

This snippet shows how to encrypt a message between the user and the app:
```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const message = Buffer.from('Keep this secure!');
const encryptedMessage = await cloudAccount.encryption.encrypt(message);
console.log(encryptedMessage.toString('hex'));
// "42494531873342d502863f3371b3183e69887f7b0564ac8d7d2db92cedfe2e672da4a7be592f773b4d5faec442f82452559c1250"
```

On the other hand, you can decrypt a message just by doing:
```javascript
const { HandCashCloudAccount, crypto } = require('handcash-connect');

const cloudAccount = new HandCashCloudAccount({...});

const encryptedMessage = Buffer.from('42494531873342d502863f3371b3183e69887f7b0564ac8d7d2db92cedfe2e672da4a7be592f773b4d5faec442f82452559c1250', 'hex');
const message = await cloudAccount.encryption.decrypt(encryptedMessage);
console.log(message.toString('hex'));
// "Keep this secure!"
```
