# Okta Classic Engine

A fixed, hardcoded state machine. There are some handfull of events like `SUCCESS`, `MFA REQUIRED`, `MFA ENROLL`, `PASSWORD EXPIRED` etc, based on these actions the state machine will be branched out by the clinet UI. <br>

here the primary api will be `/api/v1/authn` call, which results in either o the aove actions. In classic engine we call the MFA as the **Factors**, once we re good with the authN call and authz call, we wil be getting the `sessionToken`, based on that toke if we are using OIDC auth we will get the `access_token / id token` from calling `/authorize` endpoint.
