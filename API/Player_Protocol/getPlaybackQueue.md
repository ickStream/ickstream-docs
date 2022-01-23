Retrieve the complete playback queue together with metadata

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPlaybackQueue",
    "params": {
        "order": <Optional, if not specified PLAYBACK will be used as default>
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
    }
}
```

**Specific information:**

  - Note that the **params** parameter is optional since this request
    doesn't have any mandatory parameters.
  - The **order** parameter can have the following values
      - **CURRENT** - Result is ordered according to the current
        playback queue, basically in the same order tracks are going to
        be played.
      - **ORIGINAL** - Result is ordered according to the originally
        ordered playback queue, basically in the same order tracks where
        initially loaded to the current playback queue initially. If
        shuffle is disabled, ORIGINAL will return the same order as
        CURRENT. The ORIGINAL value is typically used by a controller if
        it wants to save the playback queue to a playlist in the
        [Playlist Management
        Protocol](../Playlist_Management_Protocol "wikilink")
  - The player will keep track of the order the playback queue is going
    to be played but it's also responsible to keep track of the original
    order. See the
    [addTracks](../Player_Protocol/addTracks "wikilink"),
    [removeTracks](../Player_Protocol/removeTracks "wikilink") and
    [moveTracks](../Player_Protocol/moveTracks "wikilink") methods for
    more information about how the original order is changed if
    modifying the playback queue after it has been initially loaded from
    a playlist using the
    [setTracks](../Player_Protocol/setTracks "wikilink") method.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity>
    "result": {
        "playlistId": < Globally unique identity of the playlist which the tracks in the playback queue comes from >,
        "playlistName": <Textual name for the playlist which the tracks in the playback queue comes from >,
        "order": <The order of the returned tracks, see description above >,
        "lastChanged": <timestamp when the playback queue was last changed >
        "count": < Number of entries in this response >,
        "countAll" : < Number of entries totally in the playback queue >,
        "offset" : < The offst of the first entry in this response >,
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
                        "url": Url of this streaming reference >
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

**Specific information:**

  - The **lastChanged** attribute is updated if anything in the playback
    queue has been updated since the last **playbackQueueChanged**
    notification or last **getPlaybackQueue** query.
  - For more details about individual parameters in beneath the
    **items** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
