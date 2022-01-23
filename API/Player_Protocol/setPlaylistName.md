Set name and identity of current playlist

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setPlaylistName",
    "params": {
        "playlistId": < Globally unique identity of the playlist itself >,
        "playlistName": <Textual name for the playlist itself >
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity>
    "result": {
        "playlistId": < Globally unique identity of the playlist itself >,
        "playlistName": <Textual name for the playlist itself >,
        "countAll" : < Number of entries totally in the playlist >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")