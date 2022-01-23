Get the current volume level for the player or the mute state of a
player

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getVolume",
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
        "volumeLevel": < Current volume level >
        "muted": < Current mute state, true means muted, false means unmuted >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")