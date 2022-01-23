Get current seek position within the current track. This is typically
used by a Controller to display a progress bar which indicates how large
amount of the track that has been played.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getSeekPosition",
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
        "playbackQueuePos": < The playback queue position of the current track >
        "seekPos": < Position within the track, seconds from start of track as a float >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")