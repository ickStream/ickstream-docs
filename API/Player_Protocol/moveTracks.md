Move the specified tracks from their current position to a new position
in the playback queue

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "moveTracks",
    "params": {
        "playbackQueuePos": <Optional, playback queue position to move tracks before >
        "items": [
            {
                "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
                "playbackQueuePos": < The old playback queue position of this track >,
            },
            ...
        ]
    }
}
```

**Specific Information**

  - If new **playbackQueuePos** on top level is not defined, the tracks
    are moved to the end of the playback queue
  - The **playbackQueuePos** is specified based on the playback queue
    order before the operation. So when moving tracks towards the end of
    the playback queue, the insertion positions will be higher than the
    position number the track ends up in the final playback queue after
    removing the original tracks. What this essentially means is that
    both player and controller have to process the positions as if the
    moving tracks were still part of the playback queue and then remove
    them in a second step to consolidate the playback queue and generate
    a new set of positions.
  - If playback queue mode, set with
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink"),
    is either **QUEUE_SHUFFLE** or **QUEUE_REPEAT_SHUFFLE** only the
    current playback queue is modified while the originally ordered
    playlist queue is left unchanged.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully removed >
        "playbackQueuePos": < Position of currently playing track within the playback queue >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")
