Set the current playback queue mode of the player

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setPlaybackQueueMode",
    "params": {
        "playbackQueueMode": < A valid playback queue mode >
    }
}
```

**Specific information:**

  - Valid playbackMode values are:
      - **QUEUE** - Player plays tracks to the end of playback queue and
        then stop. When this mode is set and the previous mode was
        either QUEUE_SHUFFLE or QUEUE_REPEAT_SHUFFLE the playback
        queue is also restored to the original playback queue order, see
        the separate page for [playback queue
        concepts](../Player_Protocol/Playback_queue_concepts "wikilink")
        for more information about this.
      - **QUEUE_SHUFFLE** - Player plays tracks to the end of playback
        queue and then stop: This mode results in the same behaviour as
        QUEUE but it results in that the queue is shuffled initially
        when it's set, shuffling will be done using the same logic as
        the
        [shuffleTracks](../Player_Protocol/shuffleTracks "wikilink")
        method.
      - **QUEUE_REPEAT** - Player plays tracks to the end of playback
        queue and then restart from the beginning
      - **QUEUE_REPEAT_SHUFFLE** - Player plays tracks to the end of
        the playback queue, shuffle the complete playback queue and then
        restart from the beginning. If the previous mode is something
        else than QUEUE_SHUFFLE or QUEUE_REPEAT_SHUFFLE the playback
        queue will be automatically shuffled initially by the player
        using the same logic as the
        [shuffleTracks](../Player_Protocol/shuffleTracks "wikilink")
        method.
      - **QUEUE_DYNAMIC** - Player plays tracks in the playback queue,
        but the queue is automatically filled with more tracks when
        needed by the player based on the parameters which have been
        specified in the
        [setDynamicPlaybackQueueParameters](../Player_Protocol/setDynamicPlaybackQueueParameters "wikilink")
        method to request more tracks from the available content
        services.
  - See the separate page for [playback queue
    concepts](../Player_Protocol/Playback_queue_concepts "wikilink")
    for more information about how the different modes affect the
    behaviour of other methods.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "playbackQueueMode": < The current playback queue mode after the operation >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")
