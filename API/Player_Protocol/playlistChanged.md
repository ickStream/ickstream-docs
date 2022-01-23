This can be things like:

  - Tracks have been added, removed, replaced or moved within a playlist
      - This is always due to a request from a controller (with the
        possible exception of smart/dynamic playlists which we haven't
        decided if and how to handle yet)
  - Metadata of a track has been updated
      - This can happen either by a call to **setTrackMetadata** by a
        controller or by the player itself if new metadata is provided
        in the audio stream currently playing.

**Notification:**

``` javascript
{
    "jsonrpc": 2.0,
    "method": "playlistChanged",
    "params": {
        "playlistId": < Globally unique identity of the playlist itself >,
        "playlistName" <Textual name for the playlist itself >,
        "lastChanged": <timestamp when the playlist was last changed >
        "countAll": < Total number of items currently in the playlist >
    }
}
```

**Specific information:**

  - If the playlist changes but the the changes doesn't affect currently
    playing track, only **playlistChanged** will be generated, if the
    change also affect currently playing track both **playlistChanged**
    and **playerStatusChanged** will be generated.
  - The **lastChanged** attribute is updated if anything in the playlist
    has been updated since the last **playlistChanged** notification or
    last **getPlaylist** query.

[Category:Player Protocol](Category:Player_Protocol "wikilink")