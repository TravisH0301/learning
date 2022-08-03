# OAuth

- [Access delegation](#access-delegation)
- [OAuth actors](#oauth-actors)
- [OAuth tokens](#oauth-tokens)
- [OAuth flows](#oauth-flows)
  - [Front channel](#front-channel)
  - [Back channel](#back-channel)

## Access delegation
OAuth stands for "Open Authorisation\*" and is an open standard for authorisation. 
It works to authorise devices, APIs, servers and applications using access tokens 
rather than user credentials, known as "secure delegated access".

In its most simplest form, OAuth delegates authentication to services like
Facebook, Amazon, Twitter and authorises third-party applications to access 
the user account without having to enter their login and password.

It is mostly utilized for REST/APIs and only provides a limited scope of a user's data.

Ex. OAuth authorises an access to Google Photos to a third-party app by delegating 
authentication to the third-party as a delegated access.
And the third-party app has limited access to only the photos, not other Google services.

\**Authorisation: 허가*
\**Authentication: 인증*

## OAuth actors
<img src="https://user-images.githubusercontent.com/46085656/178127852-3dc05256-581c-4636-81fc-90514e8a8a65.png" height="300px">

In OAuth flows, there are the following actors:
- Resource owner: owns the data in the resource server. (ex. User)
- Resource server: The API which stores data the application wants to access (ex. Google Photos)
- Client: the application that wants to access your data (ex. Third-party app)
- Authorisation Server: The main engine of OAuth (ex. Google authorisation server)

Clients can be public and confidential. 
- Public client: cannot be trusted to store a secret, as they can be reverse engineered to steal the secret. (ex. phone app, desktop app)
- Confidential client: can be trusted to store a secret, as they are not running on a desktop or distributed through an app store. 
People can’t reverse engineer them and get the secret key. They’re running in a protected area where end users can’t access them. 
(ex. web app, smart TV)

## OAuth tokens
- Access token: can be used by any clients (ex. public clients) to access the Resource Server (API). These tokens are short-lived, and cannot be revoked.
- Refresh token: This token is long-lived, and often confidential clients can only get this token. This token can be revoked.

OAuth doesn't limit on the type of tokens. Generally, JWT (JSON Web Tokens) is a secure and trustworthy standards.<br>
The tokens are retrieved at the Authorisation Server, where its Authorise Endpoint will require an authorisation grant and its Token Endpoint will provide the tokens after the authorisation is granted.

## OAuth flows
### Front channel
<img src="https://user-images.githubusercontent.com/46085656/182595480-5b92cc12-8169-457a-8f7f-e43bc34e3271.png" height="300px">

1. Reseouce Owner (client user) starts flow to delegate access to client to access the Resource Server
2. Client sends authorisation request with specified scoped via browser redirect to the Authorise Endpoint of the Authorisation Server
3. The Authorisation Server returns a consent dialog asking the Resource Owner for the authorisation grant with authentication (authentication is not required if cached session cookie exists)
4. The authorisation grant is given to the client via browser redirect

### Back channel
<img src="https://user-images.githubusercontent.com/46085656/182595497-698dfb52-02e7-4380-b131-ab6b1959ac84.png" height="300px">

1. The Client request to the Token End Point of the Authorisation Server for a token
2. Token is exchanged with the authorisation grant
3. The Client uses the token to access the Resource Server
