Move the specified tracks from their current position to a new position
in a previously existing playlist

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "moveTracks",
    "params": {
        "playlistId": < Globally unique identity of the playlist to change >,
        "playlistPos": <Optional, playlist position to move tracks before >
        "items": [
            {
                "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
                "playlistPos": < The old playlist position of this track >,
            },
            ...
        ]
    }
}
```

**Specific Information**

  - If new **playlistPos** on top level is not defined, the tracks are
    moved to the end of the playlist

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully moved >
        "playlistId": < Globally unique identity of the playlist itself >,
    }
}
```

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")