Retrieve the complete playlist together with metadata

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPlaylist",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
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
    "id": < The request identity>
    "result": {
        "playlistId": < Globally unique identity of the playlist itself >,
        "playlistName": <Textual name for the playlist itself >,
        "lastChanged": <timestamp when the playlist was last changed >
        "count": < Number of entries in this response >,
        "countAll" : < Number of entries totally in the playlist >,
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

  - The **lastChanged** attribute is updated if anything in the playlist
    has been updated since the last **playlistChanged** notification or
    last **getPlaylist** query.
  - For more details about individual parameters in beneath the
    **items** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
