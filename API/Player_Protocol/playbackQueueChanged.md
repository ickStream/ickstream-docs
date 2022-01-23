This can be things like:

  - Tracks have been added, removed, replaced or moved within the
    playback queue
      - This is normally due to a request from a controller but will
        also happen if a smart/dynamic playlist is used to dynamically
        fill the playback queue with tracks
  - Metadata of a track has been updated
      - This can happen either by a call to **setTrackMetadata** by a
        controller or by the player itself if new metadata is provided
        in the audio stream currently playing.

**Notification:**

``` javascript
{
    "jsonrpc": 2.0,
    "method": "playbackQueueChanged",
    "params": {
        "playlistId": < Globally unique identity of the playlist which have been used to fill the playback queue >,
        "playlistName" <Textual name for the playlist which have been used to fill the playback queue >,
        "lastChanged": <timestamp when the playback queue was last changed >
        "countAll": < Total number of items currently in the playback queue >
    }
}
```

**Specific information:**

  - If the playback queue changes but the the changes doesn't affect
    currently playing track, only **playbackQueueChanged** will be
    generated, if the change also affect currently playing track both
    **playbackQueueChanged** and **playerStatusChanged** will be
    generated.
  - The **lastChanged** attribute is updated if anything in the playback
    queue has been updated since the last **playbackQueueChanged**
    notification or last **getPlaybackQueue** query.

[Category:Player Protocol](Category:Player_Protocol "wikilink")