# LightFi API

The main documentation and interactive API test page can be found at:

- [https://apiv2.lightfi.io/docs](https://apiv2.lightfi.io/docs) 

Here you can test the routes directly from the documentation page
(Note: API calls that return a large amount of data can be very slow to render on the interactive page,
real API use will be much faster).


## Authentication (OAuth2)

The primary authentication method for the LightFi API uses OAuth2.
OAuth2 is an open standard for authorisation that provides a way for applications to access resources from
a server on behalf of a user.
OAuth2 uses tokens to authenticate requests for resources,
an access token to make requests to the API
and a refresh token can be used to obtain a new access token when the original one expires.

### OAuth2 clients

In order to use the API you must have an OAuth2 client id and client secret for your organisation to use.
This can be obtained by contacting LightFi support.

Each client can be configured according to the needs of the customer,
a typical set of configuration parameters is shown below:
 
 - **access token lifetime** - 240 minutes (must be between 5 minutes and 1 day)
 - **refresh token lifetime** - 3650 days (must be between 60 minutes and 3650 days)
 - **callback URLs** - `http://localhost:3000/, https://apiv2.lightfi.io/docs/redirect, https://oauth.pstmn.io/v1/callback`
 - **sign out URLs** - none

#### Token lifetime

The lifetime of the tokens can affect the security level of your data in case of a token being
obtained by a malicious party.
A shorter **access token** lifetime will mean that the token can only be used for a shorter period
before the user must log in again or the **refresh token** should be used to obtain a new access token.

A shorter **refresh token** lifetime also enhances security in a similar way, however, the user
must always log in again manually after the token expires to obtain a new token.
For programmatic usage this may not be desirable and a longer lifetime refresh token can be used
(up to 10 years), this means the same token can be used to obtain access tokens and use the API
for the lifetime of the refresh token without any further user input.
The refresh token should always be stored securely and should not be shared with anyone.

#### Callback URLs

These URLs are useful for allowing a user to log in using the hosted web UI.
Thus you don't need to handle user authentication manually.
These URLs should ideally be kept to a minimum set to enhance security.
A default set of URLs are provided which allow login for the API test page, Postman and local development.
These URLs should help start using the API but can be disabled or changed as your application develops.

### Authentication Examples

#### Implementing API requests on behalf of a user

It is possible to use your OAuth2 client to allow a user to login and access data from their own account,
such as is done with the [LightFi Dashboard](/dashboard/).

For an application of this type, where user interaction is expected regularly, using a shorter
refresh token lifetime such as 30 days will enhance security.

#### Implementing a machine-to-machine API shim using a refresh token

To implement a machine-to-machine API shim using OAuth2 refresh tokens, you would need to do the following:

  1. Obtain an OAuth2 client ID and client secret from LightFi for this purpose.

  2. Use the client ID and secret to request an access token and refresh token from the OAuth2 server.
  This is done by sending a request to the server's OAuth2 token endpoint
  (`oauth_url` provided with your client credentials),
  along with the appropriate grant type ("Authorization Code") and other required parameters.
  An example of how to do this using Postman can be found [here](/API/postman_refresh_token/).

  3. Store the refresh token securely.
  This is important because the refresh token can be used to obtain new access tokens,
  so it should not be shared with anyone else.

  4. Use the access token to make requests to the API.
  This is done by including the access token in the authorization header of the HTTP request.

  5. When the access token expires, use the refresh token to request a new access token from the server.
  This is done by sending a request to the server's OAuth2 token endpoint,
  along with the appropriate grant type ("refresh_token").
  An example of how to do this using Postman can be found [here](/API/postman_refresh_token/#using-the-refresh-token),
  but this should be built in to your application so that it happens automatically when the access token is close
  to expiry.

  6. Update your application to use the new access token for subsequent API requests.

By implementing this process, your application will be able to securely access the API using OAuth2 refresh tokens,
allowing it to continue making requests even after the original access token has expired.
For an application of this type, where user interaction is not expected, using a longer
refresh token lifetime such as 10 years will allow the shim layer to keep operating without any need for
user interaction.
