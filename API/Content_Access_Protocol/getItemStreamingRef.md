The getItemStreamingRef method will return a streaming reference for a
specific track or stream.

Note that the getItemStreamingRef method is optional and only have to be
supported by a content service if it doesn't provide streamingRefs
attribute in the track/stream objects returned from
[getItem](../Content_Access_Protocol/getItem "wikilink") and
[findItems](../Content_Access_Protocol/findItems "wikilink") methods.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getItemStreamingRef",
    "params": {
        "preferredFormats": [ <Optional, list of preferred streaming formats>
            "audio/flac",
            "audio/mpeg"
        ],
        "itemId": < The identity of the item to retrieve >
    }
}
```

**Some important things to notice:**

  - The **preferredFormats** attribute is optional, but if specified the
    streaming ref returned will be selected by looking at the order the
    preferred formats are listed. Please note that the returned
    streaming reference can be of a different format than the preferred
    formats if none of the preferred formats are supported.
  - If **preferredFormats** hasn't been specified, the content service
    is responsible to return a suitable default format.

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < Request identity, see JSON-RPC specification for more information >,
    "result": {
        "format": < format of stream, for example "mp3" >,
        "sampleRate": <Optional, sample rate, for example 44100 >,
        "sampleSize": <Optional, sample size, for example 16 >,
        "channels": <Optional, number of channels, for example 2 >,
        "streamFormatInformation": <Optional, additional format information about the
                                    stream, for example aac container type >
        "url": < Url of this streaming reference >
    }
}
```

**Some important things to notice:**

  - If no streaming references exist, the method will return null
  - The streaming reference returned might only be valid for a short
    period of time, due to this this method must be called just before a
    track/stream is about to be played.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
