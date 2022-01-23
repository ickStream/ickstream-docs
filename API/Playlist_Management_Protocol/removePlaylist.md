Delete a existing playlist

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removePlaylist",
    "params": {
        "playlistId": < Globally unique identity of the playlist to delete >,
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <true if playlist were successfully delete >
}
```

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")