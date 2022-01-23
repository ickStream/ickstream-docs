Add new tracks to the playback queue

Position to add them at can be specified either as a specific position
or they are added at the end of the playback queue

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "addTracks",
    "params": {
        "playbackQueuePos": <Optional, playback queue position to add tracks before >
        "items": [
            {
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
                        "url": Url of this streaming reference >,
                    }
                    ...
                ]
                "itemAttributes": {
                    < Model attibute identity >: < Model attribute value >
                }
            },
            ...
        ]
    }
}
```

**Specific Information**:

  - If **playbackQueuePos** is not defined, the behaviour differs
    depending on the current playback queue mode previously set with
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
      - If playback queue mode is **QUEUE_SHUFFLE** or
        **QUEUE_REPEAT_SHUFFLE**: Tracks are added at a random
        position after currently playing track in the current playback
        queue and they are always added to the end of the originally
        ordered playback queue.
      - If playback queue mode is something else: Tracks are added at
        the end of both the current playback queue and the original
        playback queue.
  - If **playbackQueuePos** is defined, the behaviour differs depending
    on the current playback queue mode previously set with
    [setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
      - If playback queue mode is **QUEUE_SHUFFLE** or
        **QUEUE_REPEAT_SHUFFLE**: Tracks are added at the specified
        position in the current playback queue and they are always added
        to the end of the originally ordered playback queue.
      - If playback queue mode is something else: Tracks are added at
        the specified position both in the current playback queue and
        the original playback queue.
  - If a **streamingRefs** url attribute is a service: URI, the player
    is responsible to use the **getServiceInformation** method to get
    the real the mapping to the real streaming url from the [Content
    Access Protocol](../Content_Access_Protocol "wikilink")
    implementation of the content service.
  - For more details about individual parameters in beneath the
    **items** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.
  - The **type** attribute can have the following values:
      - stream - Indicates a continuous stream, when such track is
        reached the player will continue to play it forever and never
        automatically move to the next track in the current playlist.
      - track - Indicates a track with a limited length, when it reaches
        its end the player will move on to the next track in the current
        playlist.
  - The **intermediate** flag in the **streamingRef** entries is a
    special indication which is used to indicate to the player that it
    have to provide the ickStream device access token in the header of
    the request. The stream will URL will be redirected to the real
    stream when accessed. The real stream is typically temporary and
    won't be valid long unless you start playing it so the player need
    to repeat the procedure every time the stream is played if
    intermediate is true. So when accessing a streaming reference with
    intermediate=true you always have to provide an **Authorization**
    header in same way as for other requests. This is not required for
    streaming references without intermediate flag set.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully added >
        "playbackQueuePos": < Position of currently playing track within the playback queue >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")
