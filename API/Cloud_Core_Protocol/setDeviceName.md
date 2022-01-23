Update the user friendly name for a specific device owned by current
user

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setDeviceName",
    "params": {
        "devciceId": <id of the device to update>
        "name": <The new user friendly name of the device>
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
        "name": < The new user friendly name of the device >,
        "model": < Model identification for this device >,
        "address": < Current local IP-address of this device >,
        "publicAddress": < Current public IP-address of this device (may not be accessible due to firewalls) >,
    }
}
```

Some important things to note:

  - Note that a player doesn't have to call this method itself, it's
    handled by a controller so the player will get the correct name in
    the controller user interface even if this method isn't called. The
    controller will also update the name on the player by calling the
    [setPlayerConfiguration](../Player_Protocol/setPlayerConfiguration "wikilink")
    method in the [Player Protocol](../Player_Protocol "wikilink").
  - **id**: The device id is not the same as the access token which is
    used in the **Authorization** header, the access token can only be
    retrieved by registering the device with the
    [addDeviceWithHardwareId](../Cloud_Core_Protocol/addDeviceWithHardwareId "wikilink")
    method

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
