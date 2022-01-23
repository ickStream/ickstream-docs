Get information about a specific device own by current user

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getDevice",
    "params": {
        "devciceId": <Optional, id of the device to retrieve information, if not specified information will
                              instead be retrieved of the device which was authenticated in the HTTP Authorization header >
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >
    "result": {
        "id": < Unique identity for this device >,
        "name": < The user friendly identity of this device >,
        "model": < Model identification for this device >,
        "address": < Current local IP-address of this device >,
        "publicAddress": < Current public IP-address of this device (may not be accessible due to firewalls) >,
    }
}
```

Some important things to note:

  - **id**: The device id is not the same as the access token which is
    used in the **Authorization** header, the access token can only be
    retrieved by registering the device with the
    [addDeviceWithHardwareId](../Cloud_Core_Protocol/addDeviceWithHardwareId "wikilink")
    method.
  - The **model** attribute follows the syntax <manufacturer>/<model>,
    you will be assigned which model identifier to use when applying for
    an API key

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
