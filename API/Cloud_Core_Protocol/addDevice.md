Registers a completely new device which have a hardwareId

When calling this method, the authentication must be done with a device
registration token which have been previously retrieved using the
[createDeviceRegistrationToken](../Cloud_Core_Protocol/createDeviceRegistrationToken "wikilink")
method instead of using a user access token.

This method must be called by each individual device, one device is not
allowed to call this method to register another device.

The whole device registration process is described on the page: [Device
registration process](../Device_registration_process "wikilink")

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "addDevice",
    "params": {
        "address": <Optional, current IP-address of the new device>,
        "hardwareId": <Unique hardware identity of the new device, maximum 100 characters>
        "applicationId": <The unique key for the application that implements this device >
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": < The unique identity for this device which should be used in any further communication >,
        "accessToken": < The device access token which should be used in any further communication >,
        "name": < The new user friendly name of the device >,
        "model": < Model identification for this device >,
        "address": < Optional,current local IP-address of this device >,
        "publicAddress": < current public IP-address of this device, only available if address is specified >
                "userId": < The ickStream user identity which the device belongs to >
    }
}
```

Some important things to note:

  - **id**: The globally unique identity for this device
  - **accessToken**: This is the value which should be used in the OAuth
    access token which should be used in the HTTP **Authorization**
    header in future communication, this should be stored persistently
    on the device and kept as a secret.
  - **hardwareId**: Hardware identity that will be used to return an
    existing device owned by current user instead of creating a new one
    if there already is a device registered for the same hardwareId,
    this is useful to simplify the recovery process after a factory
    reset. The hardware identity must always be the same for the same
    hardware device.
  - **applicationId**: This is the API key which you have got for your
    application.
  - **model**: Identifies model and follows the syntax
    <manufacturer>/<model>, you will be assigned which model identifier
    to use when applying for an API key

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
