Creates a device registration token which later shall be used as
authentication in the
[addDevice](../Cloud_Core_Protocol/addDevice "wikilink") call to
register the device.

This method can be authenticated either using a user access token or a
device access token. Typically a controller should authenticate itself
using a user access token when it makes the call for its own
registration and after it has been registered it uses the device access
token as authentication when making calls for players it wants to
register.

The whole device registration process is described on the page: [Device
registration process](../Device_registration_process "wikilink")

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "createDeviceRegistrationToken",
    "params": {
        "id": <Device identity of the new device, maximum 40 character, this must be globally unique>
        "name": <The user friendly name of the new device>,
        "applicationId": <The unique identity for the application makes the call, only required if authenticated with a user access token. >
    }
}
```

Some important things to note:

  - **id**: This is must be a globally unique identity, the
    recommendation is to generate UUID with upper case characters and us
    it as id, for example using syntax like
    "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX".
  - **applicationId**: This is the API key which you have got for your
    application.

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <device registration token>
}
```

Some important things to note:

  - The returned device registration token is only valid for a short
    period and can only be used in a single call to the
    [addDevice](../Cloud_Core_Protocol/addDevice "wikilink") method.
    After used once or being expired a new device registration token
    must be requested.

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
