# Identity and Access Management (IAM):

1. Who are you? (Identity) (Authentication) (AuthN)
2. What are you allowed to do? (Access) (Authorization) (AuthZ)

**Analogy** <br>

Think of a hospital. When you walk in, the receptionist checks your ID card — that's **identity**. Then they decide whether you can enter the ICU, a general ward, or only the waiting room — that's **access management**. <br>

IAM is the system that handles both of those steps, automatically, for millions of users across hundreds of apps.

## Authentication vs Authorization:

```
User(enters credentials) → AuthN(Are you who you say?) → AuthZ(What can you access?) → Resource(granted or denied)
```

The key insight: AuthN always happens first. If AuthN fails, AuthZ never even runs. In Okta, you'll configure both — who can log in, and what they can access once they do.

## Identity Provider (IdP):

A trusted system, which _creates_, _stores_ and _manages_ the **digital identity** to the users. There are 2 types of identity provider,

1. Social / Public IdP's
   - These are provided for the social accounts for the users
   - Systems like Google, Facebook (Meta), Twitter (X) gives you the social identity
   - We can use these social identity to login with spotify or anyother public facing applications.
2. Enterprice / Workforce
   - Okta, Microsoft Entra ID (formerly Azure AD), Ping Identity, and OneLogin.
   - Using these identity, we can access the enterprice applications using SSO.
   - Using Single Sign-On (SSO) to securely access your company's Slack, Salesforce, and email with one set of credentials

## Federation vs Provisioning

Both of them help us to access multiple apps with same identity. But they have their unique way to do that.

### Federation

**Trust, but DO NOT COPY** <br>

Here the App trust the IdP user at the login time, here **no user will be created within the app**, but the IdP will provide the signed token / assertion to the app. <br>

Hotel key card analogy: your corporate badge works at the hotel door because the hotel trusts your company's security system. The hotel doesn't have your photo on file — it just trusts the badge signal. <br>

**Protocols**: SAML, OIDC and OAuth 2.0 <br>

These are fast and a simple, as the app never care about handling the user details.

### Provisioning:

**Sync, create, update** <br>

Here the App trust the IdP user and also **creates the user account within the app with the details pushed by the IdP.**. Once the user leaves the corresponding app, the details will be deleted and the IdP will no longer push the data to the App. As IdP will always tries to **sync** the data to app whenever it changes. <br>

**Protocols**: SCIM 2.0

### Which one will be used by Okta?

In practice, Okta uses **both together**:

1. SCIM provisions the user account into the app
2. SAML/OIDC handles the actual login.

Think of it as: provisioning creates the seat, federation opens the door.

### Authentication Protocols and its definitions

#### Security Assertion Markup Language (SAML):

SAML 2.0, A 2005 protocal to secure the access of the applications.

Example: When user want to sign into salesforce, you can use SSO with the organization Id into salesforce. The browser redirect to registered IdP (can be any IdP but in our case its Okta), then okta will authenticate the id and send the **signed assertion to salesforce** in XML format. So now salesforec will provide signin you.

- **XML based** authentication protocol
- Enterprice SSO workhorse.
- **Transport**: Browser redirect
- Use case - Enterprise SSO

#### OAuth 2.0

Open Authorization (OAuth) 2.0, is **not an authentication protocol**, but its the **Authorization Framework**. It highly helps to access the API services by providing the minimal/limited access with temporary token to access those services without using its password.

Example: When your Notion wants to access your google docs, we wont be providing the google docs password, instead google will provide the OAuth token to Notion, which give limited access.

- JSON based token
- **token**, will have the TTL, until it expires we can use them
- **Scope**, determins what the token allows (read-write/ read-only)
- **Transport** HTTPS
- Usecases - API access delegation

#### OpenID Connect (OIDC)

Built on top of OAuth 2.0, which provides the missing piece, Authorization. It introduces the OIDC token (JWT), which tells who the user is and it can be resolved to username / email / any user profile.

- JWT Token
- **Transport** - HTTPS
- Use cases - All modern SSO

ALl modern SSO are using OIDC underthewood

#### System for Cross-Domain Identity Management (SCIM) 2.0:

The identity of the user will be pushed into the application, which uses the corresponding IdP.

- JSON REST API
- Transport HTTPS
- **Lifecycle Management** of the user credentials.
- IdP -> App
- Use case : A new engineer joins → HR updates Workday → Okta picks up the change via SCIM → Okta pushes a new user to GitHub, Slack, and Jira automatically. On the last day, Okta deactivates all those accounts simultaneously.

It an create, store and deactivate the user credentials based on the actiosn of the user.

#### Lightwight Directory Access Protocol / Active Directory (LDAP/AD):

LDAP is a protocol for querying a directory of users, groups, and computers. Microsoft Active Directory (AD) is the most widely deployed LDAP-based directory (similar to DB) — essentially every large enterprise runs it on-premises. <br>

In Simple analogy, LDAP is the system or the language to query the AD database <br>

Real-world example: When you log into a Windows domain-joined laptop at work, that's Active Directory authenticating you. The IT department stores all user accounts, group memberships, and computer records in AD. <br>

Okta connects to AD via a lightweight agent, making AD the "master" source of truth for user identities while Okta extends them to cloud apps.
