Set configuration information about the player, this includes:

  - Change the player name
  - Give the player a device registration token which is used by the
    player to register itself in ickStream Cloud to retrieve an access
    token which the player use to call ickStream Cloud services in the
    future. The player register itself in ickStream Cloud by using the
    addDevice method in the [Cloud Core
    Protocol](../Cloud_Core_Protocol "wikilink").
  - Note that the player does not have to call the
    [setDeviceName](../Cloud_Core_Protocol/setDeviceName "wikilink") in
    the [Cloud Core Protocol](../Cloud_Core_Protocol "wikilink")
    because this has already been done by the controller when this
    method is called.
  - Set the URL of the Cloud Core service which the player should be
    using

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setPlayerConfiguration",
    "params": {
        "playerName": <The name the player should expose over UPnP>,
        "deviceRegistrationToken": <Optional, the device registration token which the player should use when registering itself with ickStream Cloud>
        "cloudCoreUrl": <Optional, the URL to the Cloud Core service which the player should be using>
    }
}
```

**Specific information:**

  - If the method is called with a **deviceRegistrationToken** but
    without a **cloudCoreUrl** the player should default to using any
    previously provided **cloudCoreUrl** value or use the URL documented
    in [Cloud Core Protocol](../Cloud_Core_Protocol "wikilink") if no
    URL has been previously provided.
  - The **cloudCoreUrl** value should be without the protocol version
    number at the end, it's the player which decide which version of the
    protocol to use when accessing the Cloud Core service.
  - If the method is called with a new **cloudCoreUrl** value but
    without a **deviceRegistrationToken** any previously retrieved
    access token should be discarded and no longer be used by the
    player.
  - If the method is called with a **deviceRegistrationToken** the
    player should always register itself in ickStream cloud using the
    [addDevice](../Cloud_Core_Protocol/addDevice "wikilink") method in
    the [Cloud Core Protocol](../Cloud_Core_Protocol "wikilink") and
    use the returned access token in all further accesses to ickStream
    cloud. For further information on the device registration process
    see the page: [Device registration
    process](../Device_registration_process "wikilink")
  - When either a new **cloudCoreUrl** value or a
    **deviceRegistrationToken** is provided, the player is responsible
    to issue a
    [playerStatusChanged](../Player_Protocol/playerStatusChanged "wikilink")
    notification when the registration is completed or failed. The
    registration may execute in the background and in this case the
    setPlayerConfiguration call will typically return before the
    registration is finished and the
    [playerStatusChanged](../Player_Protocol/playerStatusChanged "wikilink")
    notification when the registration is completed or failed

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": <The unique identity of the player>,
        "playerName": <The name the player use when announcing itself over UPnP>
        "playerModel": <The player model identifier, this can be used to identify a certain type of player>
        "userId": <The ickStream user identity which the device belongs to, not defined if the device is not registered yet>

    }
}
```

**Specific information:**

  - It's not possible to retrieve an access token from a player. When a
    controller wants to register a player in ickStream Cloud it's
    responsible to call this method to pass a deviceRegistrationToken to
    the player which the player can use to register itself in ickStream
    Cloud and retrieve an access token.
  - The **playerName** should only be used when discovering unregistered
    devices. For devices that are registered in ickStream Cloud the name
    registered in ickStream Cloud for the device should be used instead.
  - The **playerModel** attribute follows the syntax
    <manufacturer>/<model>, you will be assigned which player model
    identifier to use when applying for an API key
  - The **userId** attribute indicates that the device is registered and
    which ickStream user it belongs to

[Category:Player Protocol](Category:Player_Protocol "wikilink")
