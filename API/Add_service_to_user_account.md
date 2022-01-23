# Overview

[OAuth 2.0](http://tools.ietf.org/html/draft-ietf-oauth-v2) is used for
adding services to an existing user account, to remove service you use
the removeService method in the [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink").

# Adding a service from a controller perspective

## Controller shows available services

The controller uses the **findAllServices** method in [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink") to show all available
services which aren't currently bound to the user account of the current
user.

The controller picks up the **addServiceUrl** value from the service it
wants to add.

## Controller requests a temporary user code

The controller uses the **createUserCode** method in [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink") to request a temporary
user code which can be used to add services to an existing ickStream
user account. The user code is only valid during a short time, typically
10 minutes, and its only purpose is to represent the user account during
this time.

## Controller opens a web view

The controller creates a URL using the format:

    <ADDSERVICEURL>?user_code=<USER_CODE>&redirect_uri=<REDIRECT_URI>&client_id=<API_KEY>

Where the meaning of the parameters are:

  - **ADDSERVICEURL** - The **addServiceUrl** of a service returned from
    **findAllServices** method in [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink")
  - **USER_CODE** - The **id** of the user code returned from the
    **createUserCode** method in [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink")
  - **REDIRECT_URI** - The uri which the controller later will catch
    when the web view redirects to it after the process are finished
  - **API_KEY** - The API key which has been assigned to the controller
    application

For example, the url could look like this:

    https://api.ickstream.com/ickstream-cloud-deezer/oauth/addservice?user_code=12B4E267-D466-4724-82BE-FF31DADB05CF&redirect_uri=mycontrollerapp://add_service_callback&client_id=987C3A70-A076-4312-8EF9-53E954B65F8B

## Web view redirected when finished

When the authentication process with the external provider in the web
view is finished the web view will be redirected to the specified
redirect_uri with parameters as described below.

**If operation succeeded and the service has been added to the user**

  - ?status=success
      - The redirect would with the above example be:
          - mycontrollerapp://add_service_callback?status=success

**If operation failed and the service was not added to the user**

  - ?error=access_denied
      - If the user wasn't able to authenticate with the authentication
        provided by the service to be added
      - The redirect would with the above example be:
          - mycontrollerapp://add_service_callback?error=access_denied
  - ?error=identity_already_used
      - If the identity already was bound to another existing account
      - The redirect would with the above example be:
          - mycontrollerapp://add_service_callback?error=identity_already_used
  - ?error=unauthorized_client
      - If the API_KEY or REDIRECT_URI specified wasn't valid
      - The redirect would with the above example be:
          - mycontrollerapp://add_service_callback?error=unauthorized_client

[Category:API](Introduction "wikilink")
