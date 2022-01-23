Update specified track with new metadata. Often this is only used
internally by the player when new metadata is provided within the
current audio stream, but additional metadata could also be provided by
the Controller using this method. It's possible to specifify if all
metadata should be replaced or if it should only modify the provided
metadata attributes

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setTrackMetadata",
    "params": {
        "playbackQueuePos": <Optional, position in the playback queue to update >
        "replace": <Optional, true means that all metadata for this track should be replaced, else only provided attributes will be replaced >
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
                    "url": Url of this streaming reference >,
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

  - If **playbackQueuePos** is not specified occurencces of the same
    track identity in the playback queue will be updated
  - If **playbackQueuePos** is specified, the metadata of the track at
    the specified position in the playback queue will be updated
  - For more details about the **itemAttributes** structure see the
    [Content Access Protocol](../Content_Access_Protocol "wikilink")
  - If a **streamingRefs** url attribute is a service: URI, the player
    is responsible to use the **getServiceInformation** method to get
    the real the mapping to the real streaming url from the [Content
    Access Protocol](../Content_Access_Protocol "wikilink")
    implementation of the content service.
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
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

**Specific information:**

  - The updated track information is returned, this is mainly of
    interest when not specifying **replace:true** because then the data
    can be a mix of old and new values.
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Player Protocol](Category:Player_Protocol "wikilink")
