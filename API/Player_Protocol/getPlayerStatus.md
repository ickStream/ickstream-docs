Get information about the current players status, this includes

  - Indication if player is paused or currently playing something
  - Information about current track including metadata
  - Information about current seek position within current track

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPlayerStatus",
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
    which ickStream user it belongs to
  - For more details about the **itemAttributes** structure see the
    [Content Access Protocol](../Content_Access_Protocol "wikilink")
  - The **lastChanged** attribute is updated if anything except for
    \*seekPos\* has been updated since the last **playerStatusChanged**
    notification or last **getPlayerStatus** query.
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.
  - For valid **playbackQueueMode** attribute values see
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
