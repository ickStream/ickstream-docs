Shuffle all tracks in the playback queue and put the currently playing
track as the first track. Playback will not be disrupted when this
method is called.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "shuffleTracks",
    "params": {
    }
}
```

**Specific information:**

  - The playback queue mode will not be affected by this method, to
    change the playback queue mode use the
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
    method.
  - If playback queue mode is QUEUE_SHUFFLE or QUEUE_REPEAT_SHUFFLE,
    only the current playback queue should be shuffled.
  - If playback queue mode is something else than QUEUE_SHUFFLE and
    QUEUE_REPEAT_SHUFFLE, both the current playback queue and the
    originally ordered playback queue should be shuffled the same way.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully shuffled >
        "playbackQueuePos": < Position of the currently playing track within the playback queue >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")
