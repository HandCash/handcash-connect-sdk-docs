# User Authorization

## Overview

Any Connect App requires previous consent from users in order to access to their profile and use their wallet capabilities. Once the permission is granted, the app is allowed to use the wallet capabilities. Noticed that this permission could be revoked by the users at some point.

These are the steps users have to follow in order to authenticate any Connect App:

1. Once the users click "Connect to HandCash" the Connect App redirects the user to HandCash.
2. The user will log into HandCash if needed (either in a website or native app).
3. HandCash will display the Connect App that is requesting authorization as well as the requested permissions.
4. The user will Accept or Decline the access to the Connect App.
5. HandCash will redirect the user back to the Connect App according to the action taken:
  - **Accept Permissions** -> Authorization Success URL.
  - **Decline Permissions** -> Authorization Failed URL.

![HandCash Authorization Flow](/../resources/images/handcash-connect-auth-flow.png)

## Permissions

There are different authorization permissions. All the Connect app must request the permissions they need and users must grant such permission. This is the list of different permissions:

| Name           | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| Balance        | Grants access to get the users wallet balance.                          |
| Payment        | Grants access to make payments on the users behalf.                     |
| Signing        | Grants access to sign data using the user identity.                     |
| PublicProfile  | Grants access to read the user `handle`, `displayName` and `avatarUrl`. |
| PrivateProfile | Grants access to read the user profile details such as `email`.         |

Notice that the permission `PublicProfile` is granted by default, so all Connect Apps access to the following user properties:

- **Handle** (stuk_91)
- **Display Name** (Steven Urban K.)
- **Avatar URL**: (https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db)

## Your App Authorization

Connect SDK provides a method to build an URL that redirects the user to HandCash to log in an authorize your application. Once the authorization process has been completed, they will be redirected back to your app.

This is code shows how to get the URL **you must use to redirect the user** to authorize your app:

```javascript
const { AppAuthorization } = require('handcash-connect');
const { Permissions } = AppAuthorization.Permissions;

const redirectionLoginUrl = AppAuthorization.getRedirectionLoginUrl({
  appId: 'your appId',
  permissions: [Permissions.Balance, Permissions.Payment];
});

// Use redirectionLoginUrl to redirect users in your front-end (browser, native app, ...).

```

?> Users will be accordingly redirected either to **Authorization Success URL** or **Authorization Failed URL** specified by your app in the `App Profile`.
