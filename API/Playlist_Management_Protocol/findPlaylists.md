Retrieve the list of the playlists available for the current user

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPlaylist",
    "params": {
        "search": < Optional, text that have to be part of the playlistName >,
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity>
    "result": {
        "count": < Number of entries in this response >,
        "countAll" : < Number of entries totally in the playlist >,
        "offset" : < The offst of the first entry in this response >,
        "items": [
            {
                "playlistId": < Globally unique identity of the playlist >,
                "playlistName": <Textual name for the playlist >,
                "lastChanged": <timestamp when the playlist was last changed >
            },
            ...
        ]
    }
}
```

**Specific information:**

  - The **lastChanged** attribute is updated if anything in the playlist
    is updated
  - The **search** attribute is optional, if specified only the
    playlists which contains the text specified in **search** attribute
    as part of their **playlistName** will be returned

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")