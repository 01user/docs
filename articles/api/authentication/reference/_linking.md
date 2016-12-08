# Account Linking

## Link

<h5 class="code-snippet-title">Examples</h5>

```http
GET https://${account.namespace}/authorize
Content-Type: 'application/json'
{
  "response_type": {code or token},
  "client_id": "${account.client_id}",
  "connection": "",
  "redirect_uri": "http://localhost/callback",
  "access_token": "" //logged-in user access_token
}
```

```shell
curl --request GET \
  --url 'https://${account.namespace}/authorize' \
  --header 'content-type: application/json' \
  --data '{"client_id":"${account.client_id}", "connection":"", "redirect_uri":"http://localhost/callback", "access_token":""}'
```

```javascript
```

::: panel-warning Depreciation Notice
This endpoint will soon be depreciated. The [POST	/api/v2/users/{id}/identities](/api/management/v2#!/Users/post_identities) should be used instead.
:::

Call this endpoint when a user wants to link a second authentication method (e.g user/password + facebook).

This endpoint will trigger the login flow to link an existing account with a new one. This will return a 302 redirect to the `connection` that the current user wants to add. The user is identified by the `access_token` that was returned on login success.


**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `response_type`  | `code` for server side flows, `token` for client side flows |
| `client_id`      | The `client_id` of your client |
| `connection`     | The name of the connection configured to your client. If null, it will redirect to [Auth0 Login Page](https://auth0.com/#/login_page) and show the Login Widget using the first database connection. |
| `redirect_uri`   | `http://localhost/callback` |
| `access_token`   | the logged-in user's access token |


**More Information**

- [Linking Accounts](/link-accounts)
- [User Initiated Account Linking](/link-accounts/user-initiated-linking)
- [Account Linking from Server Side Code](/link-accounts/suggested-linking)


## Unlink

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/login/unlink
Content-Type: 'application/json'
{
  "access_token": "LOGGED_IN_USER_ACCESS_TOKEN", // Primary identity access_token
  "user_id": "LINKED_USER_ID" // (provider|id)
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/login/unlink' \
  --header 'content-type: application/json' \
  --data '{"access_token": "LOGGED_IN_USER_ACCESS_TOKEN", "user_id": "LINKED_USER_ID"}'
```

```javascript
```

::: panel-warning Depreciation Notice
This endpoint will soon be depreciated. The [DELETE	/api/v2/users/{id}/identities/{provider}/{user_id}](/api/management/v2#!/Users/delete_provider_by_user_id) should be used instead.
:::

Given a logged-in user's `access_token` and `user_id`, this endpoint will unlink a user's account from the identity provider.


**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `access_token`   | the logged-in user's `access token` |
| `user_id`        | the logged-in user's `user_id` |


**More Information**

- [Unlinking Accounts](/link-accounts#unlinking-accounts)
