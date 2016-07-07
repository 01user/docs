# API

## new Auth0Lock(clientID, domain, options)

Initializes a new instance of `Auth0Lock` configured with your application `clientID` and your account's `domain` at [Auth0](https://manage.auth0.com/). You can find this information at your [application settings](https://manage.auth0.com/#/applications).

- **clientId {String}**: Your application _clientId_ in Auth0.
- **domain {String}**: Your Auth0 _domain_. Usually _your-account.auth0.com_.
- **options {Object}**: Allows to customize the dialog's appearance and behavior. See [the customization page](/libraries/articles/lock/v10/customization) for the details.

### Example

```js
var clientId = '${account.clientId}';
var domain = '${account.namespace}';
var lock = new Auth0Lock(clientId, domain);

lock.on("authenticated", function(authResult) {
  lock.getProfile(authResult.idToken, function(error, profile) {
    if (error) {
      // Handle error
      return;
    }

    localStorage.setItem("idToken", authResult.idToken);
    localStorage.setItem("profile", JSON.stringify(profile));

    // Update DOM
  });
});
```

### getProfile(idToken, callback)

Once the user has logged in and you are in possesion of and id token, you can obtain the profile with `getProfile`.

- **idToken {String}**: User id token.
- **callback {Function}**: Will be invoked after the user profile been retrieved.

#### Example

```js
lock.getProfile(idToken, function(error, profile) {
  if (!error) {
    alert("hello " + profile.name);
  }
});
```

### on(event, callback)

Lock will emit events during its lifecycle.

- `show`: emitted when Lock is shown. Has no arguments.
- `hide`: emitted when Lock is hidden. Has no arguments.
- `unrecoverable_error`: emitted when there is an unrecoverable error, for instance when no connection is available. Has the error as the only argument.
- `authenticated`: emitted after a successful authentication. Has the authentication result as the only argument.
- `authorization_error`: emitted when authorization fails. Has error as the only argument.