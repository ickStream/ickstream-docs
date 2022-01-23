This can be things like:

  - Tracks have been added, removed, replaced or moved within the
    playback queue which caused currently playing track to change
      - This is normally due to a request from a controller but it can
        also happen when a dynamic playlist is played, see
        [setDynamicPlaybackQueueParameters](../Player_Protocol/setDynamicPlaybackQueueParameters "wikilink")
        for more information about dynamic playlists.
  - Current track position has been changed
      - This can either happen due to a request from a controller or
        because the end of the previous track has played to the end
  - Metadata of current track has been updated
      - This can happen either by a call to **setTrackMetadata** by a
        controller or by the player itself if new metadata is provided
        in the audio stream currently playing.
  - Playback is started or paused
  - Volume is changed or muted/unmuted
  - Player is registered or unregistered with [Cloud
    Core](../Cloud_Core_Protocol "wikilink") service

**Notification:**

``` javascript
{
    "jsonrpc": 2.0,
    "method": "playerStatusChanged",
    "params": {
        "playing": <true for playing, false for pause >,
        "seekPos": < Position within the track >
        "playbackQueuePos": < Position within the playback queue >
        "volumeLevel": < Current volume level of player >
        "muted": < true if player is muted, else false >
        "playbackQueueMode": < The current playback queue mode of the player >
        "userId": <The ickStream user identity which the device belongs to, not defined if the device is not registered yet>
        "lastChanged": <timestamp when the player state was last changed >
        "track" :{
            "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
                "type": < Type of item, one of [track|stream] >,
            "text": < Text representation of this track >,
        "sortText": < Optional, if specified it represents the appropriate sorting order >,
            "image": < Optional, URL to an image which represent this item >
            "streamingRefs": [
                {
                    "format": <format of stream, for example "mp3" >,
                "intermediate": <Optional, true if pointed to a redirected stream >,
                "sampleRate": <Optional, sample rate, for example 44100 >,
                "sampleSize": <Optional, sample size, for example 16 >,
                "channels": <Optional, number of channels, for example 2 >,
                "streamFormatInformation": <Optional, additional format information about the
                                            stream, for example aac container type >
                    "url": Url of this streaming reference >
                }
                ...
            ]
            "itemAttributes": {
                < Model attibute identity >: < Model attribute value >
            }
        }
    }
}
```

**Specific information:**

  - The **userId** attribute indicates that the player is registered and
    which ickStream account it belongs to
  - For more details about the **itemAttributes** structure see the
    [Content Access Protocol](../Content_Access_Protocol "wikilink")
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.
  - This notification looks the same as the answer from the
    **getPlayerStatus** method but without the **id** attribute at the
    top of the JSON-RPC message to indicate that it's a notification and
    not request
  - If the playback queue changes but the changes doesn't affect
    currently playing track no **playerStatusChanged** notification will
    be generated, the **playerStatusChanged** notification is only
    generated if currently playing tracks is affected by the changes in
    the playback queue.
  - The **lastChanged** attribute is updated if anything except for
    \*seekPos\* has been updated since the last **playerStatusChanged**
    notification or last **getPlayerStatus** query.
  - When volume is changed or muted, the **playerStatusChanged**
    notification will be sent but it might be delayed a bit to avoid
    sending out a lot of unnecessary notifications.
  - For valid **playbackQueueMode** attribute values see
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
