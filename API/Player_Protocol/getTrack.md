Get currently playing track together with metadata. Can also be used to
get information about another track at a specific position in the
current playlist

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getTrack",
    "params": {
        "playbackQueuePos": < Playback queue position to get track for >
    }
}
```

**Specific information:**

  - If **playbackQueuePos** is not specified the currently playing track
    is returned
  - Note that the **params** parameter is optional since this request
    doesn't have any mandatory parameters.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "playlistId": < Identity of the playlist which the playback queue is filled from >
        "playbackQueuePos": < Position within the playback queue >
        "track" :{
            "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
            "text": < Text representation of this track >,
            "type": < Type of item, one of [track|stream] >,
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

  - For more details about the **itemAttributes** structure see the
    [Content Access Protocol](../Content_Access_Protocol "wikilink")
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
