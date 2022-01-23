Get configuration information about the player

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPlayerConfiguration",
    "params": {
    }
}
```

**Specific information:**

  - Note that the **params** parameter is optional since this request
    doesn't have any mandatory parameters.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": <The unique identity of the player>,
        "playerName": <The name the player>,
        "playerModel": <The player model identifier, this can be used to identify a certain type of player>
        "userId": <The ickStream user identity which the device belongs to, not defined if the device is not registered yet>
    }
}
```

**Specific information:**

  - The **userId** attribute indicates that the player is registered and
    which ickStream user it belongs to.
  - The **playerModel** attribute follows the syntax
    <manufacturer>/<model>, you will be assigned which player model
    identifier to use when applying for an API key

[Category:Player Protocol](Category:Player_Protocol "wikilink")