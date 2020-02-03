# User Authorization

## Overview

Connect Apps require consent from users in order to access to their profile and use their wallet capabilities. Once the permission is granted, the app is allowed to use the wallet capabilities. This permission can be revoked by the users at any point. 

Below are the steps each user will need to follow in order to authenticate with your Connect App:

1. The user will click "Connect to HandCash" redirecting them to the HandCash login portal, or native app.
2. The user will then log into HandCash if needed.
3. HandCash will display the Connect App that is requesting authorization as well as the requested permissions.
4. The user will Accept or Decline access to the Connect App.
5. HandCash will then redirect the user back to the Connect App according to the action taken:

- Accept Permissions -> Authorization Success URL.
- Decline Permissions -> Authorization Failed URL.

![HandCash Authorization Flow](/../resources/images/handcash-connect-auth-flow.png)

## Permissions

There are different levels of permissions. Connect apps will need to request the permissions they want and users will be asked to grant those permissions. Below are the available permissions:

| Name           | Description                                                             |
| -------------- | ----------------------------------------------------------------------- |
| Balance        | Grants access to get the user's wallet balance.                          |
| Payment        | Grants access to make payments on the user's behalf.                     |
| Signing        | Grants access to sign data using the user's identity.                     |
| PublicProfile  | Grants access to read the user's `handle`, `displayName` and `avatarUrl`. |
| PrivateProfile | Grants access to read the user's profile details such as `email`.         |

Note: `PublicProfile` is a default permission, meaning all Connect Apps will gain access to the following user properties:

- **Handle** (stuk_91)
- **Display Name** (Steven Urban K.)
- **Avatar URL**: (https://handcash.io/avatar/7d399a0c-22cf-40cf-b162-f5511a4645db)

## Your App Authorization

Connect SDK provides a method to build a URL that redirects the user to the HandCash login portal. Once the authorization process has been completed, they will be redirected back to your app.

This code shows how to generate the URL that **you must use to redirect the user** in order to authorize your app:

```javascript
const { AppAuthorization } = require('handcash-connect');
const { Permissions } = AppAuthorization.Permissions;

const redirectionLoginUrl = AppAuthorization.getRedirectionLoginUrl({
  appId: 'your appId',
  permissions: [Permissions.Balance, Permissions.Payment];
});

// Use redirectionLoginUrl to redirect users in your front-end (browser, native app, ...).

```

> Users will be redirected from the HandCash login portal back to either the **Authorization Success URL** or **Authorization Failed URL**, specified by your app in the `App Profile`.
