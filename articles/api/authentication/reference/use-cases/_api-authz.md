# API Authorization

## Authorize Client

To begin an OAuth 2.0 Authorization flow, your Client application should first send the user to the authorization URL.

The purpose of this call is to obtain consent from the user to invoke the Resource Server (specified in `audience`) to do certain things (specified in `scope`) on behalf of the user. The Authorization Server will authenticate the user and obtain consent, unless consent has been previously given. If you alter the value in `scope`, the Authorization Server will require consent to be given again.

The OAuth 2.0 flows that require user authorization are:
- Authorization Code Grant
- Authorization Code Grant using Proof Key for Code Exchange (PKCE)
- Implicit Grant

On the other hand, the [Resource Owner Password Grant](/api-auth/grant/password) and [Client Credentials](/api-auth/grant/client-credentials) flows do not use this endpoint since there is no user authorization involved. Instead they invoke directly the `POST /oauth/token` endpoint to retrieve an access token.

Note also the following:
* Additional parameters can be sent that will be passed through to the provider, e.g. `access_type=offline` (for Google refresh tokens) , `display=popup` (for Windows Live popup mode).
* The `state` parameter will be returned and can be used for XSRF and contextual information (like a return url).

Based on the OAuth 2.0 flow you are implementing, the parameters slightly change. To determine which flow is best suited for your case refer to: [Which OAuth 2.0 flow should I use?](/api-auth/which-oauth-flow-to-use).

### Authorization Code Grant

<h5 class="code-snippet-title">Examples</h5>

```http
GET https://${account.namespace}/authorize
Content-Type: 'application/json'
{
  "audience": {API_AUDIENCE},
  "scope": {SCOPE},
  "response_type": "code",
  "client_id": "${account.client_id}",
  "redirect_uri": {CALLBACK_URL},
  "state": {OPAQUE_VALUE}
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/authorize' \
  --header 'content-type: application/json' \
  --data '{"audience":{API_AUDIENCE},"scope":{SCOPE},"response_type":"code","client_id": "${account.client_id}","redirect_uri":{CALLBACK_URL},"state":{OPAQUE_VALUE}}'
```

```javascript

```

This is the OAuth 2.0 grant that regular web apps utilize in order to access an API. For details refer to:
- [Calling APIs from Server-side Web Apps](/api-auth/grant/authorization-code)
- [Executing an Authorization Code Grant Flow](/api-auth/tutorials/authorization-code-grant)

**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `audience`       | The target API for which the Client Application is requesting access on behalf of the user. |
| `scope`          | The scopes which you want to request authorization for. These must be separated by a space. |
| `response_type`  | Indicates to the Authorization Server which OAuth 2.0 Flow you want to perform. Use `code` for Authorization Code Grant Flow. |
| `client_id`      | Your application's Client ID. |
| `state`          | An opaque value the client adds to the initial request that the Authorization Server (Auth0) includes when redirecting the back to the client. Used to prevent CSRF attacks. |
| `redirect_uri`   | The URL to which the Authorization Server (Auth0) will redirect the User Agent (Browser) after authorization has been granted by the User. |


### Authorization Code Grant using Proof Key for Code Exchange (PKCE)

<h5 class="code-snippet-title">Examples</h5>

```http
GET https://${account.namespace}/authorize
Content-Type: 'application/json'
{
  "audience": {API_AUDIENCE},
  "scope": {SCOPE},
  "response_type": "code",
  "client_id": "${account.client_id}",
  "code_challenge": {CODE_CHALLENGE},
  "code_challenge_method": "S256",
  "redirect_uri": {CALLBACK_URL}
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/authorize' \
  --header 'content-type: application/json' \
  --data '{"audience":{API_AUDIENCE},"scope":{SCOPE},"response_type":"code","client_id": "${account.client_id}","code_challenge":{CODE_CHALLENGE},"code_challenge_method":"S256","redirect_uri":{CALLBACK_URL}}'
```

```javascript

```

This is the OAuth 2.0 grant that mobile apps utilize in order to access an API. Before starting with this flow, you need to generate and store a `code_verifier`, and using that, generate a `code_challenge` that will be sent in the authorization request. For details refer to:
- [Calling APIs from Mobile Apps](/api-auth/grant/authorization-code-pkce)
- [Executing an Authorization Code Grant Flow with PKCE](/api-auth/tutorials/authorization-code-grant-pkce)

**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `audience`       | The target API for which the Client Application is requesting access on behalf of the user. |
| `scope`          | The scopes which you want to request authorization for. These must be separated by a space. |
| `response_type`  | Indicates to the Authorization Server which OAuth 2.0 Flow you want to perform. Use `code` for Authorization Code Grant (PKCE) Flow. |
| `client_id`      | Your application's Client ID. |
| `redirect_uri`   | The URL to which the Authorization Server (Auth0) will redirect the User Agent (Browser) after authorization has been granted by the User. |
| `code_challenge_method` | Method used to generate the challenge. The PKCE spec defines two methods, `S256` and `plain`, however, Auth0 supports only `S256` since the latter is discouraged. |
| `code_challenge` | Generated challenge from the `code_verifier`. |


### Implicit Grant

<h5 class="code-snippet-title">Examples</h5>

```http
GET https://${account.namespace}/authorize
Content-Type: 'application/json'
{
  "audience": {API_AUDIENCE},
  "scope": {SCOPE},
  "response_type": {token or id_token token},
  "client_id": "${account.client_id}",
  "redirect_uri": {CALLBACK_URL},
  "state": {OPAQUE_VALUE},
  "nonce": {CRYPTOGRAPHIC_NONCE}
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/authorize' \
  --header 'content-type: application/json' \
  --data '{"audience":{API_AUDIENCE},"scope":{SCOPE},"response_type":"token or id_token token","client_id": "${account.client_id}","redirect_uri":{CALLBACK_URL},"state":{OPAQUE_VALUE},"nonce":{CRYPTOGRAPHIC_NONCE}}'
```

```javascript

```

This is the OAuth 2.0 grant that Client-side web apps utilize in order to access an API. For details refer to:
- [Calling APIs from Client-side Web Apps](/api-auth/grant/implicit)
- [Executing the Implicit Grant Flow](/api-auth/tutorials/implicit-grant)

**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `audience`       | The target API for which the Client Application is requesting access on behalf of the user. |
| `scope`          | The scopes which you want to request authorization for. These must be separated by a space. |
| `response_type`  | This will specify the type of token you will receive at the end of the flow. Use `id_token token` to get an `id_token`, or `token` to get both an `id_token` and an `access_token`. |
| `client_id`      | Your application's Client ID. |
| `state`          | An opaque value the client adds to the initial request that the Authorization Server (Auth0) includes when redirecting the back to the client. Used to prevent CSRF attacks. |
| `redirect_uri`   | The URL to which the Authorization Server (Auth0) will redirect the User Agent (Browser) after authorization has been granted by the User. |
| `nonce` | A string value which will be included in the ID token response from Auth0, used to prevent token replay attacks. |

If `response_type=token`, after the user authenticates with the provider, this will redirect them to your application callback URL while passing the `access_token` and `id_token` in the address `location.hash`. This is used for Single Page Apps and on Native Mobile SDKs.

## Get Token

Use this endpoint to get an `access_token` in order to call an API. You can, optionally, retrieve an `id_token` and a `refresh_token` as well.

The only OAuth 2.0 flows that can retrieve a refresh token are:
- [Authorization Code Grant](/api-auth/grant/authorization-code)
- [Authorization Code Grant Flow with PKCE](/api-auth/grant/authorization-code-pkce)
- [Resource Owner Password Grant](/api-auth/grant/password)

### Authorization Code

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/oauth/token
Content-Type: 'application/json'
{
  "grant_type": "authorization_code",
  "client_id": "${account.client_id}",
  "client_secret": "${account.client_secret}",
  "code": "YOUR_AUTHORIZATION_CODE",
  "redirect_uri": "https://myclientapp.com/callback"
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/oauth/token' \
  --header 'content-type: application/json' \
  --data '{"grant_type":"authorization_code","client_id": "${account.clientId}","client_secret": "${account.clientSecret}","code": "YOUR_AUTHORIZATION_CODE","redirect_uri": "https://myclientapp.com/callback"}'
```

```javascript
```

> The response is a JSON with a body in this format:

```JSON
{
  "access_token": "eyJz93a...k4laUWw",
  "refresh_token": "GEbRxBN...edjnXbL",
  "id_token": "eyJ0XAi...4faeEoQ",
  "token_type": "Bearer"
}
```

This is the OAuth 2.0 grant that regular web apps utilize in order to access an API. For details refer to:
- [Calling APIs from Server-side Web Apps](/api-auth/grant/authorization-code)
- [Executing an Authorization Code Grant Flow](/api-auth/tutorials/authorization-code-grant)


**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `grant_type`     | Denotes the flow you are using: `authorization_code`, `client_credentials` or `password`. |
| `client_id`      | Your application's Client ID. |
| `client_secret`  | Your application's Client Secret. |
| `code`  | The Authorization Code received from the initial `/authorize` call. |
| `redirect_uri`  | The URL must match exactly the `redirect_uri` passed to `/authorize`. |

### Authorization Code (PKCE)

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/oauth/token
Content-Type: 'application/json'
{
  "grant_type": "authorization_code",
  "client_id": "${account.client_id}",
  "code_verifier": "YOUR_GENERATED_CODE_VERIFIER",
  "code": "YOUR_AUTHORIZATION_CODE",
  "redirect_uri": "com.myclientapp://myclientapp.com/callback"
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/oauth/token' \
  --header 'content-type: application/json' \
  --data '{"grant_type":"authorization_code","client_id": "${account.clientId}","code_verifier": "YOUR_GENERATED_CODE_VERIFIER","code": "YOUR_AUTHORIZATION_CODE","redirect_uri": "com.myclientapp://myclientapp.com/callback"}'
```

```javascript
```

> The response is a JSON with a body in this format:

```JSON
{
  "access_token": "eyJz93a...k4laUWw",
  "refresh_token": "GEbRxBN...edjnXbL",
  "id_token": "eyJ0XAi...4faeEoQ",
  "token_type": "Bearer"
}
```

This is the OAuth 2.0 grant that mobile apps utilize in order to access an API. For details refer to:
- [Calling APIs from Mobile Apps](/api-auth/grant/authorization-code-pkce)
- [Executing an Authorization Code Grant Flow with PKCE](/api-auth/tutorials/authorization-code-grant-pkce)


**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `grant_type`     | Denotes the flow you are using: `authorization_code`, `client_credentials` or `password`. |
| `client_id`      | Your application's Client ID. |
| `code`  | The Authorization Code received from the initial `/authorize` call. |
| `code_verifier` | Cryptographically random key that was used to generate the `code_challenge` passed to `/authorize`. |
| `redirect_uri`  | The URL must match exactly the `redirect_uri` passed to `/authorize`. |


### Client Credentials

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/oauth/token
Content-Type: 'application/json'
{
  audience: "{YOUR_API_IDENTIFIER}",
  grant_type: "client_credentials",
  client_id: "${account.clientId}",
  client_secret: "${account.clientSecret}"
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/oauth/token' \
  --header 'content-type: application/json' \
  --data '{"audience":"{YOUR_API_IDENTIFIER}", "grant_type":"client_credentials", "client_id":"${account.clientId}", "client_secret":"${account.clientSecret}"}'
```

```javascript
```

> The response is a JSON with a body in this format:

```JSON
{
  "access_token": "eyJz93a...k4laUWw",
  "token_type": "Bearer"
}
```

This is the OAuth 2.0 grant that server processes utilize in order to access an API. For details refer to:
- [Calling APIs from a Service](/api-auth/grant/client-credentials)
- [Setting up a Client Credentials Grant using the Management Dashboard](/api-auth/config/using-the-auth0-dashboard)
- [Asking for Access Tokens for a Client Credentials Grant](/api-auth/config/asking-for-access-tokens)

**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `grant_type`     | Denotes the flow you are using: `authorization_code`, `client_credentials` or `password`. |
| `client_id`      | Your application's Client ID. |
| `client_secret`  | Your application's Client Secret. |
| `audience` | API Identifier that the client is requesting access to. |

### Resource Owner Password

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/oauth/token
Content-Type: 'application/json'
{
  "grant_type":"password",
  "username":{RESOURCE_OWNER_USERNAME},
  "password":{RESOURCE_OWNER_PASSWORD},
  "audience":{YOUR_API_IDENTIFIER},
  "scope":{SCOPES},
  "client_id": ${account.clientId}
}
```

```shell
curl --request POST \
  --url '${account.namespace}/oauth/token' \
  --header 'content-type: application/json' \
  --data '{"grant_type":"password", "username":{RESOURCE_OWNER_USERNAME}, "password":{RESOURCE_OWNER_PASSWORD}, "audience":{YOUR_API_IDENTIFIER}, "scope":{SCOPES}, "client_id": ${account.clientId}}'
```

```javascript
```

> The response is a JSON with a body in this format:

```JSON
{
  "access_token": "eyJz93a...k4laUWw",
  "token_type": "Bearer"
}
```

This is the OAuth 2.0 grant that highly trusted apps utilize in order to access an API. In this flow the end-user is asked to fill in credentials (username/password) typically using an interactive form. This information is later on sent to the Client and the Authorization Server. It is therefore imperative that the Client is absolutely trusted with this information. For details refer to:
- [Calling APIs from Highly Trusted Clients](/api-auth/grant/password)
- [Executing the Resource Owner Password Grant](/api-auth/tutorials/password-grant)

**Query Parameters**

| Parameter        | Description |
|:-----------------|:------------|
| `grant_type`     | Denotes the flow you are using: `authorization_code`, `client_credentials` or `password`. |
| `client_id`      | Your application's Client ID. |
| `audience` | API Identifier that the client is requesting access to. |
| `username` | Resource Owner's identifier. |
| `password` | Resource Owner's secret. |
| `scope` | String value of the different scopes the client is asking for. Multiple scopes are separated with whitespace. |

## Get Token Info

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/tokeninfo
Content-Type: 'application/json'
{
  "id_token": ""
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/tokeninfo' \
  --header 'content-type: application/json' \
  --data '{"id_token":""}'
```

```javascript

```

::: panel-warning Depreciation Notice
This endpoint will soon be depreciated. The `/userinfo` endpoint should be used instead to obtain user information.
:::

This endpoint validates a JSON Web Token (signature and expiration) and returns the user information associated with the user id `sub` property of the token.

<aside class="notice">
For more information, see: <a href="/user-profile/user-profile-details#api">User Profile: In-Depth Details - API</a>.
</aside>

> This command returns a JSON object in this format:

```json
[
  {
    "id": 1
  },
  {
    "id": 2
  }
]
```

**Query Parameters**

| Parameter        | Type       | Description |
|:-----------------|:-----------|:------------|
| `id_token`       | object     | the `id_token` to use |

## Resource Owner

<h5 class="code-snippet-title">Examples</h5>

```http
POST https://${account.namespace}/oauth/ro
Content-Type: 'application/json'
{
  "client_id":   "${account.clientId}",
  "connection":  "",
  "grant_type":  "",
  "username":    "",
  "password":    "",
  "scope":       "",
  "id_token":    "",
  "device":      ""
}
```

```shell
curl --request POST \
  --url 'https://${account.namespace}/oauth/ro' \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{ "client_id": "${account.clientId}", "connection": "", "grant_type": "", "username": "", "password": "", "scope": "", "id_token": "", "device": "" }'
```

```javascript
```

::: panel-warning Depreciation Notice
This endpoint will soon be depreciated. The `/oauth/token { grant_type: password }` should be used instead.
:::

Given the user's credentials, this endpoint will authenticate the user with the provider and return a JSON object with the `access_token` and an `id_token`.

This endpoint only works for database connections, passwordless connections, Active Directory/LDAP, Windows Azure AD and ADFS. For more information, see: [Calling APIs from Highly Trusted Clients](/api-auth/grant/password).

> This command returns a JSON object in this format:

```json
[
  {
    "id": 1
  },
  {
    "id": 2
  }
]
```

**Query Parameters**

| Parameter        | Type       | Description |
|:-----------------|:-----------|:------------|
| `client_id`      | string     | the `client_id` of your client |
| `connection`     | string     | the name of the connection configured to your client |
| `grant_type`     | string     | `password` |
| `username`       | string     | the user's username |
| `password`       | string     | the user's password |
| `scope`          | string     | `openid` or `openid profile email` or `openid offline_access` |
| `id_token`       | string     |  |
| `device`         | string     |  |
