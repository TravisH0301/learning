# OIDC
OpenID Connect (OIDC) is an identity authentication protocol that sits on top of open authorization (OAuth) 2.0 
to standardise the process for authenticating and authorising users when they sign in to access digital services.<br>
OIDC provides authentication, which means verifying that users are who they say they are. 
And OIDC is also used to provide single sign-on.

## Components
- **User**: Owner of identity, data and resources.
- **OpenId Provider(OP)**: This is an identity server which implements OAuth 2.0 and OpenID Connect protocols.
- **Relying Party(RP)**: Also known as client, this is the application which relies on the identity server for tasks like authentication and authorizing end-users.
- **Scope**: Scope identifies the resources that a relying party wants access to.
- **ID Token**: This is the outcome of the authentication process. OpenID specifies ID token to be a JSON web token. ID token must contain at the minimum an identifier for the end-user called the subject claim and can contain additional information like user details and how the user was authenticated.
- **Access Token**: Access tokens are used to authorize access to a resource. These contain information about the client and APIs use that information to grant access to their data.
- **Refresh Token**: Refresh tokens are used to get a new access token once the existing access token expires. These can only be used once and has a longer validity period than access tokens.

## OIDC Flow
OIDC flows are identical to OAuth 2.0 flows as it extends with the following:
- Use of the **"OpenID" Scope**: OIDC is triggered by including the openid scope in an OAuth request, prompting the server to return an ID Token.
- **ID Token**: The key difference is that OIDC provides an ID Token (a JWT containing user information) alongside the standard OAuth access token.
- **UserInfo Endpoint**: OIDC adds an optional UserInfo endpoint that the client can call to get more details about the user.

And here are the OAuth 2.0 flow types that OIDC flow uses too:
- **Authorization Code flow**: only works with clients that can secure their client secret. First, the client directs the resource owner to an authorization server. Subsequently, the resource owner authenticates with the authorization server, which then redirects the resource owner back to the client with an authorization code.
- **Implicit flow**: is designed for clients who can’t secure their client secret. This process is similar to the Authorization Code flow; however, the access token is returned to the client directly without using an intermediate authorization code.
- **Password flow**: requires the client to collect the resource owner’s authentication details and send them to the authorization server. Only clients that the resource owner highly trusts should employ this process.
- **Client Credentials flow**: is best when the client is also the resource owner. This involves the client undergoing authentication with the authorization server using its credentials.

The primary flow of OIDC (same as OAuth 2.0) is the authorization code flow, where the Relying party (or Client in OAuth) secretly exchanges an authorization code for tokens, instead of direct handover of tokens post user authentication. The below is the authorization code flow with OIDC's authentication layer.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/643397c2-d185-4c9e-b59e-b630eae3baa4" />

1. **Authentication Request**: <br>Relying party sends a HHTP GET request to the authorization endpoint of OpenID provider. A sample request may include the followings:
- **Response type**: This is used to define the flow of the request. When initiating authorization code flow this value must be set as “code”.
- **Scope**: Lists the scopes that the RP is requesting access to. “openid” scope must be included to initiate the openid authorization flow.
- **Client id**: RP must be registered in the OP before initiating authorization code flow. client id is used by the OP to identify the client.
- **State**: This is a string that can be used by the RP to keep track of the session.
- **Redirect uri**: This indicates the URI to which the OP should send the response. This should match the URI provided when the RP is registered on the OP.

2. **Authenticate User**: <br>OP will check if the request at step 1 is valid and try to authenticate the end-user. This will typically be done by prompting the end-user to enter their username and password. After authentication, OP will ask the end-user to allow the information requested by the RP to be shared.
3. **Return Authorization Code**: <br>After successfully authenticating the end-user, the OP will return an authorization code to the redirect URI of the RP by using a HTTP 302 redirect request. This response will also contain a state parameter if the state was present in the authorization request.
4. **Retrieve Tokens Using Authorization Code**: <br>After receiving the authorization code, RP can send a HTTP POST request to the OP’s token endpoint. After validating the authorization code OP will return access, ID, and refresh tokens back to the RP.
5. **Retrieve Userinfo Using Access Token**: <br>Additional information about the user can be requested by sending a HTTP GET request to the userinfo endpoint of the OP. The access token must be sent with this request and it is used by the OP to validate the request.

### Example
This example is from an [Okta blog](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-OAuth-and-oidc).

**Please note that this example uses the terminologies from OAuth 2.0, and here is the mapping**

| OAuth | OIDC |
|---|---|
| Resource Owner | User |
| Client | Relying Party|
| Authroization Server | OIDC Provider |

<img width="800" height="1928" alt="image" src="https://github.com/user-attachments/assets/80bb8670-d12f-444b-9fd5-6b5bcf2e441a" />

1. You, the Resource Owner, want to allow “Terrible Pun of the Day,” the Client, to access your contacts so they can send invitations to all your friends.
2. The Client redirects your browser to the Authorization Server and includes with the request the Client ID, Redirect URI, Response Type, and one or more Scopes it needs (with mandatory scope of openid).
3. The Authorization Server verifies who you are, and if necessary prompts for a login.
4. The Authorization Server presents you with a Consent form based on the Scopes requested by the Client. You grant (or deny) permission.
5. The Authorization Server redirects back to Client using the Redirect URI along with an Authorization Code.
6. The Client contacts the Authorization Server directly (does not use the Resource Owner’s browser) and securely sends its Client ID, Client Secret, and the Authorization Code.
7. The Authorization Server verifies the data and responds with an ID token and Access Token.
8. The Client can now use the Access Token to send requests to the Resource Server for your contacts.

## OIDC vs. OAuth 2.0
|Aspect|OAuth 2.0|OIDC|
|---|---|---|
|Purpose|Authorization|Authentication|
|Answers|"What can they access?"|"Who are they?"|
|Tokens|Access token, Request token|ID token + Access/Refresh tokens|
|Use Cases|API authorization|User login, Single Sing-On (SSO)|
|Stands Alone?|Yes|No (Built on OAuth)|


