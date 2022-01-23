Replace all tracks in the playlist, if the playlist doesn't exist it
will be created

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setTracks",
    "params": {
        "playlistId": < Optionally, globally unique identity of the playlist >,
        "playlistName": <Optionally, textual name for the playlist  >,
        "overwrite": < false or not specified if an existing playlist should not be overwritten >
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

**Specific Information**

  - If **overwrite** is set to false or not specified, an existing
    playlist will never be overwritten, a controller can use this if it
    want to be sure to create a new playlist.
  - If **overwrite** is set to true any existing playlist with same
    **playlistId** (if specified) or **playlistName** (if playlistId is
    not specified) will be overwritten
  - If **playlistId** is not specified a unique identity will be
    generated in the service and returned if the operation succeeds
  - The **playlistName** attribute must be unique in the context of
    current user. Trying to call this function without having
    overwrite=true with the same name as an existing playlist will throw
    an error. Trying to call this function with overwrite=true and
    providing both **playlistId** and **playlistName** where the
    **playlistName** value matches another playlist will also throw an
    error.
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
        "result": <true if tracks were successfully added >
        "playlistId": < Globally unique identity of the playlist itself >,
    }
}
```

[Category:Playlist Management
Protocol](Category:Playlist_Management_Protocol "wikilink")
