# Security Assertion Markup Language (SAML):

Before SAML, every application has its own user and therirown password, so each apps handles its own authentication. Now consider the scenarion of the organization if the org has 50 apps, everything will have its own authn/z validation, which is huge. After SAML, its handles everything into single sign on (SSO).

1. User
2. IdP - Okta, Siteminder
3. SP - Servive provider - the apps that verifies the digital assertion provided by IdP like AWS, Salesforce etc.

## High level flow:

### IdP Initiated

1. User inside the Okta dashboard
2. Clicks on the application which he/she have access, for eg, consider he was the AWS IAM User who will be authenticated with OKTA
3. When User clicks, the IdP sends the **SAML response + Digital Assertion** (since he/she was already in signed in okta dashboard) to SP
4. The SP will validate the token, name and its attributes and if successsfuly, then SP will authenticate . here AWS will allow the user to access its console.
5. here we are not sending the AuthN request 

### SP Initiated:

1. User clicks the SP own login screen, consider the pluralsight. User enters the email / password
2. The SP will redirect the AuthN request to Okta or any registered IdP.
3. The Okta will validate the user and provide the SAML response with Digital assertion to IdP., if the user entered valid credentials
4. Then SP will validate the signature from IdP and post that user will be signed in

## SP and IdP trust:

The SP and IdP must already have the mutal understanding on each other. The SAML request and response will have the important details, which determins their trust.

1. SAML request/ response - The meta data on from where the user comming from, ACS url (redirect URI)
2. `X.509 Certificate`, when Okta signs the assertion, it will attach the signature encrypted with its own private key and it will attach the `x.509` certificate in the SAML metadata and send to SP. Then SP will try to decrypt the signature using the attached certificate in SAML response.

All the private key, certificates can be controlled in Admin Dashboard.
