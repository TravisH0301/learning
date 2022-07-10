# OAuth

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
![image](https://user-images.githubusercontent.com/46085656/178127852-3dc05256-581c-4636-81fc-90514e8a8a65.png)

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
refer to https://developer.okta.com/blog/2017/06/21/what-the-heck-is-oauth

## OAuth flows
refer to https://developer.okta.com/blog/2017/06/21/what-the-heck-is-oauth


## Difference between OAuth 1.0 & OAuth 2.0


# Todo: Add headers
