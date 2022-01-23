Update specified track with new metadata. This is used to
refresh/enhance a playlist with more extensive metadata, normally this
function is not something that's used by a Controller directly. It's
possible to specify if all metadata should be replaced or if it should
only modify the provided metadata attributes

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setTrackMetadata",
    "params": {
        "playlistId": < Globally unique identity of the playlist to update >,
        "playlistPos": <Optional, position in the playlist to update >
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

  - If **playlistPos** is not specified occurencces of the same track
    identity in the playlist will be updated
  - If **playlistPos** is specified, the metadata of the track at the
    specified position in the playlist will be updated
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.
  - For more details about the **itemAttributes** structure see the
    [Content Access
    Protocol](../Content_Access_Protocol "wikilink")

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "track": {
            "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
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

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")
