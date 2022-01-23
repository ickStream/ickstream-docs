Remove the specified tracks from the playback queue

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeTracks",
    "params": {
        "items": [
            {
                "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
                "playbackQueuePos": < Optional, playback queue position to remove the track from >
            },
            ...
        ]
    }
}
```

**Specific Information**

  - If **playbackQueuePos** is not specified, all occurrences of the
    track in the playback queue will be removed
  - Any removed tracks should be removed both from current playback
    queue and originally ordered playback queue independent which
    playback queue mode that is currently active.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully removed >
        "playbackQueuePos": <Position of currently playing track within the playback queue >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")