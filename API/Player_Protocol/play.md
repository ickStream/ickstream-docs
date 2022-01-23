Start or stop playing current track in the playlist at the current seek
position Typically used by a Controller for Play and Pause buttons

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "play",
    "params": {
        "playing": <true for playing, false for pause >
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "playing": <true if player has started to play, else false >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")