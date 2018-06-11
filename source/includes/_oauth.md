# Authorization

Dwolla utilizes the [OAuth 2 protocol](https://oauth.net/2/) to facilitate authorization. OAuth is an authorization framework that enables a third-party application to obtain access to protected resources (Transfers, Funding Sources, Customers etc.) in the Dwolla API. This section covers application auth which is meant for server-to-server applications using the Dwolla API.

#### Creating an application
Before you can get started with making OAuth requests, you’ll need to first register an application with Dwolla by logging in and navigating to the applications page. Once an application is registered you will obtain your `client_id` and `client_secret` (aka App Key and Secret), which will be used to identify your application when calling the Dwolla API. The Sandbox environment provides you with a created application once you have signed up for an account. Learn more in our [getting started guide](https://developers.dwolla.com/guides/sandbox-setup/). **Remember:** Your client_secret should be kept a secret! Be sure to store your client credentials securely.

#### Token lifetime

**Access tokens** are *short lived*: 1 hour. To refresh authorization on an application access token, your application will simply exchange its client credentials for a new app access token which will invalidate the previous token.

## Application authorization

The [client credentials flow](https://tools.ietf.org/html/rfc6749#section-4.1) is used when an application needs to obtain permission to act on its own behalf. To obtain an application access token, your application will send a POST requests with the Authorization header that contains the word `Basic` followed by a space and a base64-encoded string client_id:client_secret. An application access token can then be used to make calls to the Dwolla API on behalf of the application, for example, create a webhook subscription, retrieve events, and make calls to [Customer](https://docsv2.dwolla.com/#customers) related endpoints.

`Authorization: Basic Base64(client_id:client_secret)`

#### HTTP request

**Production:** `POST https://accounts.dwolla.com/token`

**Sandbox:** `POST https://accounts-sandbox.dwolla.com/token`

Including the `Content-Type: application/x-www-form-urlencoded` header, the request is sent to the token endpoint with `grant_type=client_credentials` in the body of the request:

#### Request parameters
| Parameter | Required | Type | Description |
|-----------|----------|----------------|-------------|
| client_id | yes | string | Application key. Navigate to `https://www.dwolla.com/applications` (production) or `https://dashboard-sandbox.dwolla.com/applications` (Sandbox) for your application key. |
| client_secret | yes | string | Application secret. Navigate to `https://www.dwolla.com/applications` (production) or `https://dashboard-sandbox.dwolla.com/applications` (Sandbox) for your application secret. |
| grant_type | yes | string | This must be set to `client_credentials`. |

#### Response parameters

Parameter | Description
----------|------------
access_token | A new access token that is used to authenticate against resources that belong to the app itself.
expires_in | The lifetime of the access token, in seconds.  Default is 3600.
token_type | Always `bearer`.

### HTTP status and error codes
| HTTP Status | Message | Description |
|--------------|-------------|-------------|
| 200 | OK | Valid request. |
| 400 | invalid_request | Malformed request. |
| 401 | invalid_client | Not authorized. Missing or incorrect credentials. |

#### Request

```raw
POST https://accounts-sandbox.dwolla.com/token
Authorization: Basic YkVEMGJMaEFhb0pDamplbmFPVjNwMDZSeE9Eb2pyOUNFUzN1dldXcXUyeE9RYk9GeUE6WEZ0bmJIbXR3dXEwNVI1Yk91WmVOWHlqcW9RelNSc21zUU5qelFOZUFZUlRIbmhHRGw=
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```
```python
# Using dwollav2 - https://github.com/Dwolla/dwolla-v2-python
# This example assumes you've already initialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
application_token = client.Auth.client()
```
```javascript
// Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-node
// This example assumes you've already initialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
client.auth.client()
  .then(function(appToken) {
    return appToken.get('webhook-subscriptions');
  })
  .then(function(res) {
    console.log(JSON.stringify(res.body));
  });
```
```ruby
# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby
# This example assumes you've already initialized the client. Reference the SDKs page for more information: https://developers.dwolla.com/pages/sdks.html
application_token = $dwolla.auths.client
# => #<DwollaV2::Token client=#<DwollaV2::Client id="..." secret="..." environment=:sandbox> access_token="..." expires_in=3600 scope="...">
```
```php
/**
 *  No support for this language yet. We recommend using an external REST client for making OAuth requests.
 **/
```

#### Successful response

```noselect
{
  "access_token": "SF8Vxx6H644lekdVKAAHFnqRCFy8WGqltzitpii6w2MVaZp1Nw",
  "token_type": "bearer",
  "expires_in": 3600
}
```
* * *
