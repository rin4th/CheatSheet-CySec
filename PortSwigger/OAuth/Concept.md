OAuth is a login mechanism, that allow client application access information from another application. OAuth have 3 parties
1. client applications : The application that wants to access the user's data
2. Resource owner : The user whose data the client application wants to access.
3. OAuth serviced provider : The application that control the user's data and access it

General stages OAuth mechanism:
1. The client application request access to a subset of user's data, specifying which grant type they want to use and what kind of access they want
2. The user is prompted to log in to the OAuth service and explicitly give their consent for the requested access.
3. The client application receives a unique access token that proves they have permission from the user to access the requested data. Exactly how this happen varies significantly depending on the grant type.
4. The client application uses this access token to make API calls fetching the relevant data from the resource server.

Recon OAuth:
1. Try to request to server provider -`/.well-known/oauth-authorization-server` or `/.well-known/openid-configuration`, these will return a JSON configuration file containing key information, such as details of additional features that may be supported.
2. 