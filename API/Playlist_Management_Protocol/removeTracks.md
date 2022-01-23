Remove the specified tracks from a previously existing playlist

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeTracks",
    "params": {
        "playlistId": < Globally unique identity of the playlist to change >,
        "items": [
            {
                "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
                "playlistPos": < Optional, playlist position to remove the track from >
            },
            ...
        ]
    }
}
```

**Specific Information**

  - If **playlistPos** is not specified, all occurrences of the track in
    the playlist will be removed

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully removed >
        "playlistId": < Globally unique identity of the playlist itself >,
    }
}
```

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")