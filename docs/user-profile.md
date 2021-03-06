# User Profile

HandCash Connect provides access to the user profile, **as long as the user grants access**.

These properties are provided to help app developers with two aspects:

- Speed-up the sign-up process of the user with in new Connect Apps. No more sign-up forms!
- Provide more customized and contextual information to the user inside the Connect App, such as displaying amounts in the user's local currency or their available balance.

## Balance

> This feature requires the [permission](/user-authorization.md#permissions) `Permissions.Balance`. Otherwise you will receive an error.

```javascript
const balance = await cloudAccount.getBalance();
console.log(balance);
// {
//       "fiatCurrency": "USD",
//       "fiatAmount": 27.42,
//       "satoshiAmount": 1094091900
// }
```

## Public profile

The user's public profile is accessible by default. This is useful for a basic profile for your users, inside your app.

All Connect Apps provide access to the following user properties:

- **Handle** (stuk_91)
- **Display Name** (Steven Urban K.)
- **Avatar URL**: (https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db)

```javascript
const userProfile = await cloudAccount.getProfile();
console.log(userProfile.publicProfile());
// {
//      "handle": "stuk_91",
//      "displayName": "Steven Urban K.",
//      "avatarUrl": "https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db"
// }
```

## Private profile

> This feature requires the [permission](/user-authorization.md#permissions) `Permissions.PrivateProfile`. Otherwise you will receive an error.

```javascript
const userProfile = await cloudAccount.getProfile();
console.log(userProfile.getPrivateProfile());
// {
//       "email": "steven_urk@domain.com",
//       "phoneNumber": "+12025550142"
// }
```

## Preferences

All Connect Apps will have access the user preferences in order to offer a customized experience inside the app.

```javascript
const userProfile = await cloudAccount.getProfile();
console.log(userProfile.getPreferences());
// {
//       "localCurrency": "USD"
// }
```

For example, the prefered local currency is useful to display amounts in Connect Apps expressed in the users' local currency.

[Illustration of UI showing an action expressed in fiat amount]