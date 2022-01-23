# Overview

[2.0](http://tools.ietf.org/html/draft-ietf-oauth-v2%7COAuth) is used
for authentication when accessing the cloud from a controller or player.
The communication is secured through HTTPS and the principle is that the
first time a Controller connects to the cloud, the user will be
requested to authenticate itself and after the cloud server has
validated the user it will return a long lived **AccessToken** which the
controller can use to create/retrieve device information using [Cloud
Core Protocol](Cloud_Core_Protocol "wikilink")

# Authentication/Registration from a controller perspective

## Initial user authentication

### Controller open web view

Typically the controller opens a web view with the url

    https://api.ickstream.com/ickstream-cloud-core/oauth?redirect_uri=<REDIRECT_URI>&client_id=<API_KEY>

  - The **API_KEY** value is an API key which have been assigned to the
    specific controller application
  - The **REDIRECT_URI** value is a URI which the
    registration/authentication process will redirect to when the user
    has been authenticated

As an example, the a controller can select to open a web view with a URL
like:

    https://api.ickstream.com/ickstream-cloud-core/oauth?redirect_uri=mycontrollerapp://authentication_callback&client_id=987C3A70-A076-4312-8EF9-53E954B65F8B

As an alternative, if a controller want a native selection of which
service provider to use or want to hard code it to use a specific
provider, it can use the [Cloud Core Authentication
Protocol](Cloud_Core_Authentication_Protocol "wikilink") to retrieve
a list of available providers and pick the **url** from the provider it
wants to use. The parameters sent in the url is the same as above, it's
just the path of the url that's different. Using this the url can for
example look like this:

    https://api.ickstream.com/ickstream-cloud-deezer/oauth?redirect_uri=mycontrollerapp://authentication_callback&client_id=987C3A70-A076-4312-8EF9-53E954B65F8B

Please note that if a controller is hard coded to use a specific
provider, it needs to be aware that this provider might be deactivated
in the future. Due to this, the preferred solution is to use the server
provided selection dialog as described in the beginning of this section.

### Web view redirect and controller catches this redirect

After the authentication process is finished, the web view will be
redirected to the specified \*REDIRECT_URI\* value and add requests
parameters as follows:

  - **If the operations succeeded**
      - ?code=<AUTHORIZATION_CODE>
          - The \*AUTHORIZATION_CODE\* is a short lived code (about 10
            minutes) which can be used to retrieve a permanent access
            token
          - Following the above example, a successful authentication can
            be redirected to:
              - mycontrollerapp://authentication_callback?code=d2ff7663-815d-4d54-ba31-87e4bdf125fa

<!-- end list -->

  - **If the operations fails**
      - ?error=<ERROR_CODE>
          - Where the \*ERROR_CODE\* follows the OAuth specification
            and can be one of
              - \*invalid_request\* \\- The request is missing a
                required parameter, includes an invalid parameter value,
                includes a parameter more than once, or is otherwise
                malformed.
              - \*unauthorized_client\* \\- The client is not
                authorized to request an authorization code using this
                method.
              - \*access_denied\* \\- The resource owner or
                authorization server denied the request.
              - \*unsupported_response_type\* \\- The authorization
                server does not support obtaining an authorization code
                using this method.
              - \*invalid_scope\* \\- The requested scope is invalid,
                unknown, or malformed.
              - \*server_error\* \\- The authorization server
                encountered an unexpected condition that prevented it
                from fulfilling the request. (This error code is needed
                because a 500 Internal Server Error HTTP status code
                cannot be returned to the client via a HTTP redirect.)
              - \*temporarily_unavailable\* \\- The authorization
                server is currently unable to handle the request due to
                a temporary overloading or maintenance of the server.
                (This error code is needed because a 503 Service
                Unavailable HTTP status code cannot be returned to the
                client via a HTTP redirect.)
          - Following the above example, a failed authentication can be
            redirected to:
              - mycontrollerapp://authentication_callback?error=access_denied

### Controller requests an access token

After the controller has got the authorization code, it makes a new HTTP
request to

    https://api.ickstream.com/ickstream-cloud-core/oauth/token?redirect_uri=<REDIRECT_URI>&code=<AUTHORIZATION_CODE>

  - The **REDIRECT_URI** value must be the same as the one used when
    requesting the AUTHORIZATION_CODE
  - The **AUTHORIZATION_CODE** value is the one retrieved from the
    previous step

Following the above example this request might be:

    https://api.ickstream.com/ickstream-cloud-core/oauth/token?redirect_uri=mycontrollerapp://authentication_callback&code=d2ff7663-815d-4d54-ba31-87e4bdf125fa

  - **If successful, the request will return JSON data with the format**

`{`
`   "access_token": "C0A412B4-EF9A-47A3-811A-CC6059D88F51"`
`}`

  -   - The returned access token is a user access token and can be used
        to create a device using the [device registration
        process](Device_registration_process "wikilink")

  - **If not successful, the request will return JSON data with the
    format**

`{`
`   "error": An ERROR_CODE value`
`   "error_description": Optional, a human-readable ASCII text providing additional information, used to assist the client developer in understanding the error that occurred.`
`}`

Where **ERROR_CODE** value follows the OAuth specification and can be
one of:

  -   - **invalid_request** - The request is missing a required
        parameter, includes an unsupported parameter value (other than
        grant type), repeats a parameter, includes multiple credentials,
        utilizes more than one mechanism for authenticating the client,
        or is otherwise malformed.
      - **invalid_client** - Client authentication failed (e.g. unknown
        client, no client authentication included, or unsupported
        authentication method). The authorization server MAY return an
        HTTP 401 (Unauthorized) status code to indicate which HTTP
        authentication schemes are supported. If the client attempted to
        authenticate via the "Authorization" request header field, the
        authorization server MUST respond with an HTTP 401
        (Unauthorized) status code, and include the "WWW-Authenticate"
        response header field matching the authentication scheme used by
        the client.
      - **invalid_grant** - The provided authorization grant (e.g.
        authorization code, resource owner credentials) or refresh token
        is invalid, expired, revoked, does not match the redirection URI
        used in the authorization request, or was issued to another
        client.
      - **unauthorized_client** - The authenticated client is not
        authorized to use this authorization grant type.
      - **unsupported_grant_type** - The authorization grant type is
        not supported by the authorization server.
      - **invalid_scope** - The requested scope is invalid, unknown,
        malformed, or exceeds the scope granted by the resource owner.

## Initial user registration

The user registration flow follows the same principles as the user
authentication flow described in the previous chapter, the only
difference is that the user will get a question if a new account should
be created in the web view before it's redirected to the controller with
the authorization code.

# Authentication/Registration from a cloud server perspective

### 1\. Ask user to select authentication provider

The first step in the authentication process is that the controller
opens the URL:

  - ```
    https://api.ickstream.com/ickstream-cloud-core/oauth
    ```

The cloud server will show a web page asking user to select
authentication provider. To this step the controller app will provide:

  - redirect_uri - The redirect_uri which the controller later will
    catch redirects to
  - client_id - The API key assigned to the controller application

### 2\. Redirect to external OAuth authentication provider

This step starts after the user has selected which authentication
provider to use by selecting on of the urls:

  - Facebook:
        https://api.ickstream.com/ickstream-cloud-core/oauth/facebook
  - Google:
        https://api.ickstream.com/ickstream-cloud-core/oauth/google
  - ickStream username/password:
        https://api.ickstream.com/ickstream-cloud-core/oauth/ickstream

The cloud server will redirect to the selected external provider. In the
call to the external provider different parameters will be provided
depending on the selected provider, but typically this means:

  - redirect_uri - This is set according to:
      - Facebook:
            https://api.ickstream.com/ickstream-cloud-core/oauth/facebook/callback
      - Google:
            https://api.ickstream.com/ickstream-cloud-core/oauth/google/callback
      - ickStream username/password:
            https://api.ickstream.com/ickstream-cloud-core/oauth/ickstream/callback
  - client_id - The API key for the selected provider, this is not the
    same API key as provided by the controller.

Note that ickStream username/password provider is implemented as a
separate module so it's handle the same as all other providers in the
ickStream cloud logic.

### 3\. Authorization code redirected back to ickStream cloud

The external provider will redirect back to the redirect_uri as
descried above and pass an authorization code as request parameter. The
ickStream cloud service will retrieve the authorization code.

### 4\. Getting access token and user identity from the external provider

ickStream cloud will use the authorization code from the previous step
to retrieve an access token from the external provider.

Using the access token for the external provider, ickStream cloud will
now request identity information for the user. Typically it is asking
for e-mail but for providers which doesn't support e-mail it will ask
for an alternative identity which the external provider offers.

### 5\. Generating an authorization code

ickStream cloud now generates a temporary authorization code and then
redirects to the redirect_uri which the controller initially specified
and passes the authorization code as a request parameter. The generated
authorization code is tied to a specific user using the identity
information retrieved in the previous step. Note that the user doesn't
have to exist as an ickStream account in this step, the authorization
code is just tied to the identity information, for example the e-mail
address retrieved from the external authentication provider.

If the user doesn't exist as an ickStream account, an intermediate web
page will be shown to the user asking if a new account should be
created. In this case the redirect will happen after the user has
approved or rejected to create a new account.

### 6\. Controller requests an access token

When the controller requests an access token, ickStream cloud will
lookup the user which is tied to the authorization code and return the
access token for this user. If the user doesn't exist a new user
including access token will be generated in this step.

# Authentication/Registration from a user perspective

## Authentication for an already existing user

The process for an already existing user looks as follows

1.  A web view is opened in the controller which asks the user to select
    authentication provider, currently available providers are:
      - Facebook
      - Google
      - ickStream username/password
2.  After selecting the desired authentication provider the user login
    through the authentication screen provided by the external provider,
    this phase typically includes two steps
      - Enter username/password
      - Approve that ickStream should be allowed to access data from the
        external authentication provider

## Registration of a new user using an external authentication provider

The registration process for a new user authenticating with an external
provider looks as follows

1.  A web view is opened in the controller which asks the user to select
    authentication provider, currently available providers are:
      - Facebook
      - Google
      - ickStream username/password
2.  After selecting the desired authentication provider the user login
    through the authentication screen provided by the external provider,
    this phase typically includes two steps
      - Enter username/password
      - Approve that ickStream should be allowed to access data from the
        external authentication provider
3.  The user is asked if a new account should be created

## Registration of a new user using ickStream username/password provider

The registration process for a new user authenticating with an external
provider looks as follows

1.  A web view is opened in the controller which asks the user to select
    authentication provider, currently available providers are:
      - Facebook
      - Google
      - ickStream username/password
2.  After selecting the ickStream username/password authentication
    provider the user will be asked to login or create a new account, if
    selecting a new account the user have to specify an e-mail address.
    The ickStream cloud server will now send an e-mail to the user.
3.  The user clicks on the link in the received e-mail and enter the
    desired password
4.  The user goes back to the app and registered a new account according
    to the same process as the one used for an external authentication
    provider.

After the the user has registered, the app will typically automatically
register itself as a device using the [device registration
process](Device_registration_process "wikilink")

[Category:API](Introduction "wikilink")
