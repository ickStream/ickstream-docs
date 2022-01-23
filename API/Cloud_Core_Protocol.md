# Principles

The Cloud Core provides a JSON-RPC interface with the functions
described on the following page, the endpoint url for this interface is:

    http://api.ickstream.com/ickstream-cloud-core/jsonrpc

A specific version of the Cloud Core protocol can always be accessed by
appending the protocol version number at the end of the url, for example
to use version 1.0 of the protocol the following url can be used:

    http://api.ickstream.com/ickstream-cloud-core/jsonrpc/1.0

The **getProtocolVersions** method can be use the check supported
protocol versions and based on the result the appropriate URL to use can
be decided. To be able to survive future changes of the protocol, a
client should a URL with the appended version number. Using the URL
without version number will be the same as using the URL with the
maximum supported version number.

The most of the methods are protected by a OAuth Authorization header,
so to be able to use it the Controller first needs to have a valid OAuth
access token.

The "Authorization" header can be composed as:

  - Bearer <accessToken>

If the access token represent a user and not a device the available
methods are a bit more limited, it's basically possible to get
information about current user, list devices for current user and
request a device registration token and remove a device.

To get a user access token, you need to let the user authenticate in a
web view as described on: [User authentication and
registration](User_authentication_and_registration "wikilink"). The
purpose of a user access token is that there are some operations that
aren't accessed from a device, one example can be an administration web
site, but also that you need it to be able to request a device
registration token using the **createDeviceRegistrationToken** method to
be able to register the device itself with the **addDevice** method to
get a device access token.

After the device is registered and you have got a device access token,
you should use the device access token and discard the user access
token. The user access token is typically never stored persistently in
an application, it's only the device access token which needs to be
stored persistently in the local storage of the application.

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

## Device related methods

  - [findDevices](Cloud_Core_Protocol/findDevices "wikilink") - Get
    devices of current user
  - [getDevice](Cloud_Core_Protocol/getDevice "wikilink") - Get
    information about current or a specific device owned by current user
  - [setDeviceAddress](Cloud_Core_Protocol/setDeviceAddress "wikilink")
    - Set device address of current device
  - [setDeviceName](Cloud_Core_Protocol/setDeviceName "wikilink") -
    Set device name of a device
  - [createDeviceRegistrationToken](Cloud_Core_Protocol/createDeviceRegistrationToken "wikilink")
    - Request a device registration token for a device that should be
    registered
  - [addDevice](Cloud_Core_Protocol/addDevice "wikilink") - Register
    itself as a device
  - [removeDevice](Cloud_Core_Protocol/removeDevice "wikilink") -
    Remove a previously registered device

## User related methods

  - [getUser](Cloud_Core_Protocol/getUser "wikilink") - Get
    information about current user
  - [setUserData](Cloud_Core_Protocol/setUserData "wikilink") - Set
    information related to current user
  - [removeUserIdentity](Cloud_Core_Protocol/removeUserIdentity "wikilink")
    - Remove a user identity from current user
  - [createUserCode](Cloud_Core_Protocol/createUserCode "wikilink")
    - Create a user code which is needed to add new user identities or
    new services
  - [findAccountChanges](Cloud_Core_Protocol/findAccountChanges "wikilink")
    - Get information about changes in the account of the current user
  - [setUserConfiguration](Cloud_Core_Protocol/setUserConfiguration "wikilink")
    - Set one or several user configuration parameters for the current
    user
  - [getUserConfiguration](Cloud_Core_Protocol/getUserConfiguration "wikilink")
    - Get one or several user configuration parameters for the current
    user
  - [removeUserConfiguration](Cloud_Core_Protocol/removeUserConfiguration "wikilink")
    - Remove one or several user configuration parameters for the
    current user

## Service related methods

  - [findService](Cloud_Core_Protocol/findServices "wikilink") - Get
    services for current user
  - [findAllServices](Cloud_Core_Protocol/findAllServices "wikilink")
    - Get all available services which can be added to current user
  - [removeService](Cloud_Core_Protocol/removeService "wikilink") -
    Remove service from current user

# Sample responses

See [the separate samples
page](Cloud_Core_Protocol/Examples "wikilink")

[Category:API](Introduction "wikilink")
