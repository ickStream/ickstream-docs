Report a track as played

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "playedTrack",
    "params": {
        "occurrenceTimestamp": <Optional, the time when the track was played>,
        "playedPercentage": <Optional, the percentage of the whole track duration that was played (an integer between 0-100)>
        "track": {
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

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <true if the reporting succeeds, else false >
}
```

**Specific information:**

  - The **occurrenceTimestamp** attribute will be automatically set by
    the service to the current time if not provided
  - The playback history will be tied both to the current user and the
    current device making the call, the information about which device
    that makes the call is taken from the OAuth authentication data.
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Scrobble Protocol](Category:Scrobble_Protocol "wikilink")
