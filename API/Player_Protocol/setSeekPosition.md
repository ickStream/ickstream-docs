Move the current seek position within the current track

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setSeekPosition",
    "params": {
        "playbackQueuePos": < The playback queue position of the track to seek within >
        "seekPos": < Seek position within the track, seconds from start of track as a float >
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "playbackQueuePos": < The playback queue position after the change >
        "seekPos": < Seek position within the track after the change, seconds from start of track as a float >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")