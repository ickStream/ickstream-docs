Change the current track

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setTrack",
    "params": {
        "playbackQueuePos": < Absolute position within the playback queue >
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "playbackQueuePos": < Playback queue position after the change >,
    }
}
```

**Specific information:**

  - When switching track, you will also get a **playerStatusChanged**
    notification which contains more information about the new track

[Category:Player Protocol](Category:Player_Protocol "wikilink")