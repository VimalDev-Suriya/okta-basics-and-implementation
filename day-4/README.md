# OAuth 2.0 and OIDC:

## OAuth

A **authorization framework**, this will provide the user to get access, but it will never say who the user was. There are 3 main ways this OAuth works

### Authorization Flow

This flow is highly used in Server side web apps, where the main entities are User (browser), Web Server, Okta Authorizer and Protected API.

1. When user click Login
2. The request will pass through the web-server, then it will /redirect to Okta auth app / custom app.
3. User enters the credentials and MFA
4. Web server redirects to okta auth server, post successful validation, it will respond with **auth code**
5. The web server will get that `auth code` from the previous response and combine it with **client secret** (which will be stored in web server protectively), and sends the request to Okta auth server.
6. Now Okta validate the client secret and finally send the okta **access_token**
7. Which is the one where we can use it to call the protected API.

### Authorization Code + PKCE

PKCE - Proof Key for Code Exchange. This is the upgraded version of Authorization flow, where the client secret where not used. This is primarily used in SPA applications (react/vue).

### Client credentials:

Primarily used for Machine to Machine (M2M) communication.

1. Service will have its own Clinet ID, Client Secret, Scope and Grant_type=clinet Credentials
2. The service will hit the Okta auth server with those details
3. If it is valid, then Okta will return the **access_token**
4. So we can use them as **Bearer {access_token}** to access the successive protected API
