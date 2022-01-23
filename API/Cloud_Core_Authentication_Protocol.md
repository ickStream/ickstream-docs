# Principles

The Cloud Core Authentication protocoll provides a JSON-RPC interface
with the functions described on the following page, the endpoint url for
this interface is:

    http://api.ickstream.com/ickstream-cloud-core/public/jsonrpc

A specific version of the Cloud Core Authentication protocol can always
be accessed by appending the protocol version number at the end of the
url, for example to use version 1.0 of the protocol the following url
can be used:

    http://api.ickstream.com/ickstream-cloud-core/public/jsonrpc/1.0

The **getProtocolVersions** method can be use the check supported
protocol versions and based on the result the appropriate URL to use can
be decided. To be able to survive future changes of the protocol, a
client should use URL with the appended version number. Using the URL
without version number will be the same as using the URL with the
maximum supported version number.

This service is public but require an API-key as authentication in the
OAuth Authorization header. The service is used to retrieve
authentication providers to be able to display a native
login/registration dialog to the user.

The actual login/registration process must be handled in a web view as
described on: [User authentication and
registration](User_authentication_and_registration "wikilink").

After the user is registered and you have got a user access token, you
should use the user token in the [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink") to register the current
device, add services and access other kind of functionality. The user
access token is typically never stored persistently in an application
running on a device, instead an application should immediately after
registration use the [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink") to register the device and
use the device token in all further accesses.

**Please note\!**\\\\ If the application doesn't need a native selection
dialog for available authentication providers to use, it can instead use
the selection web page provided by the Cloud Core protocol service, see
[User authentication and
registration](User_authentication_and_registration "wikilink") for
more information about this. Typically, the Cloud Core Authentication
provider service is only needed if the selection of authentication
providers needs to be integrated in a bigger web page.

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

## Authentication provider methods

  - [findAuthenticationProviders](Cloud_Core_Authentication_Protocol/findAuthenticationProviders "wikilink")
    - Find all available authentication providers

[Category:API](Introduction "wikilink")
