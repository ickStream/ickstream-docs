# Overview

[OAuth 2.0](http://tools.ietf.org/html/draft-ietf-oauth-v2) is used for
adding additional identities to an existing user account, to remove an
identity you use the removeUserIdentity method in the \[Cloud Core
Protocol|Cloud Core Protocol\]. Identity typically means an e-mail
address but it can also be other type of identities in the future.

# Adding an identity from a controller perspective

## Controller requests a temporary user code

The controller uses the **createUserCode** method in \[Cloud Core
Protocol\] to request a temporary user code which can be used to add
identities to an existing ickStream user account. The user code is only
valid during a short time, typically 10 minutes, and its only purpose is
to represent the user account during this time.

## Controller opens a web view

The controller creates a URL using the format:

    https://api.ickstream.com/ickstream-cloud-core/oauth/addidentity?user_code=<USER_CODE>&redirect_uri=<REDIRECT_URI>&client_id=<API_KEY>

Where the meaning of the parameters are:

  - **USER_CODE** - The **id** of the user code returned from the
    \*createUserCode\* method in \[Cloud Core Protocol\]
  - **REDIRECT_URI** - The uri which the controller later will catch
    when the web view redirects to it after the process are finished
  - \*API_KEY\* \\- The API key which has been assigned to the
    controller application

For example, the url could look like this:

    https://api.ickstream.com/ickstream-cloud-core/oauth/addidentity?user_code=12B4E267-D466-4724-82BE-FF31DADB05CF&redirect_uri=mycontrollerapp://add_identity_callback&client_id=987C3A70-A076-4312-8EF9-53E954B65F8B

As an alternative, the controller can also use the [Cloud Core
Authentication
Protocol](Cloud_Core_Authentication_Protocol "wikilink") to get a
list of available authentication providers and pick the
**addIdentityUrl** attribute from the one it wants to use and open a web
view using this URL instead of the one mentioned above. Typically this
is only used by controllers which needs to integrate the selection of
authentication providers on a bigger web page, the preferred solution is
to use the above mentioned server provided selection dialog. When using
a controller specific dialog, the URL to add an identity using a
specific authentication provider can for example look like this:

    https://api.ickstream.com/ickstream-cloud-deezer/oauth/addidentity?user_code=12B4E267-D466-4724-82BE-FF31DADB05CF&redirect_uri=mycontrollerapp://add_identity_callback&client_id=987C3A70-A076-4312-8EF9-53E954B65F8B

## Web view redirected when finished

When the authentication process with the external provider in the web
view is finished the web view will be redirected to the specified
redirect_uri with parameters as described below.

**If operation succeeded and the identity has been added to the user**

  - ?status=success
      - The redirect would with the above example be:
          - mycontrollerapp://add_identity_callback?status=success

**If operation failed and the identity was not added to the user**

  - ?error=access_denied
      - If the user wasn't able to authenticate with the authentication
        provider used for adding the identity
      - The redirect would with the above example be:
          - mycontrollerapp://add_identity_callback?error=access_denied
  - ?error=identity_already_used
      - If the identity already was bound to another existing account
      - The redirect would with the above example be:
          - mycontrollerapp://add_identity_callback?error=identity_already_used

[Category:API](Introduction "wikilink")
