Retrieve tracks for a dynamic playlist using a specified set of
parameters

The purpose is to be able to easily

  - Get random music you have access to
  - Get random music within your collection
  - Get random music which are part of one of your playlists
  - Get random music from your friends libraries
  - Get random music from a specific artist
  - Get random music from a specific playlist
  - Get random music from a specific category/genre

It's not mandatory for all content services to support all of these
playlist types. So even if the controller specifies a long list of
services the player might only be able to get tracks from a few of them
because the rest might not support the selection parameter type
specified.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getNextDynamicPlaylistTracks",
    "params": {
        "count": < number of tracks to retrieve >,
        "previousItems" [ Optional, the complete structure for the previous items played
            {
               ...
            },
            ...
        ],
        "selectionParameters": { <selection parmeters to use when finding tracks >
            "type": <Type of selection>,
            "data": <JSON data that defines the selection logic>
        }
    }
}
```

**Specific information:**

  - The **previousItems** parameter is an array of track items
    previously played, the caller decides how many tracks to provide but
    it's generally not recommended to provide more than 10 as it will
    increase the the request size too much to be effective. The purpose
    of the **previousItems** parameter is to allow smart playlists which
    are based on previously played tracks, for example
      - A playlist which uses a previous track as a seed to future
        tracks
      - A playlist which doesn't repeat tracks from an already played
        artist for a while

The **type** parameter in the **selectionParameters** parameter can have
the following values

  - '''RANDOM_ALL
      - Get random tracks from the complete content offered by the
        content service.
      - The **data** parameter does not have to be specified for this
        type value.

<!-- end list -->

  - **RANDOM_MY_LIBRARY**
      - Get random tracks from the library of the user from the content
        service, it's up to each content service to decide what "My
        Library" means but typically it's represented by the myMusic
        context exposed by the content service.
      - The **data** parameter does not have to be specified for this
        type value.

<!-- end list -->

  - **RANDOM_MY_PLAYLISTS**
      - Get random tracks from the user playlists handled by the content
        service, it's up to each content service to decide what "My
        Playlists" means but typically it's represented by the playlists
        available under the myMusic context exposed by the content
        service.
      - The **data** parameter does not have to be specified for this
        type value.

<!-- end list -->

  - **RANDOM_FRIENDS**
      - Get random tracks from the friends of the user handled by the
        content service, it's up to each content service to decide what
        "My Friends" means but typically it's represented by the
        friendsMusic context exposed by the content service.
      - The **data** parameter does not have to be specified for this
        type value.

<!-- end list -->

  - **RANDOM_FOR_ARTIST**
      - Get random track from a specific artist.
      - For this type, the **data** parameter is expected to be a
        structure as follows

<!-- end list -->

``` javascript
{
    "artist": <The textual name of the artist to restrict retrieved tracks to>
    "artistId": <Optional, the identity fo the artist. Please note that this is service specific,
                      so it can only be used if a single service is specified in the services attribute or if the selectionParameters are specified per service >
}
```

  - **RANDOM_FOR_PLAYLIST**
      - Get random track from a specific playlist.
      - For this type, the **data** parameter is expected to be a
        structure as follows

<!-- end list -->

``` javascript
{
    "playlist": <The textual name of the playlist to restrict retrieved tracks to>
    "playlistId": <Optional, the identity fo the playlist. Please note that this is service specific,
                         so it can only be used if a single service is specified in the services attribute or if the selectionParameters are specified per service >
}
```

  - **RANDOM_FOR_CATEGORY**
      - Get random track from a specific category/genre.
      - For this type, the **data** parameter is expected to be a
        structure as follows

<!-- end list -->

``` javascript
{
    "category": <The textual name of the category to restrict retrieved tracks to>
    "categoryId": <Optional, the identity fo the category. Please note that this is service specific,
                           so it can only be used if a single service is specified in the services attribute or if the selectionParameters are specified per service >
}
```

Except for the above mentioned **type** parameter values, a content
service can also provide its own type values. These should always be
prefixed with the service identity to indicate that they are service
specific, for example "myservice.supersmartplaylisttype".

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "countAll": <Optional, total number of items available within the specified "context">,
        "lastChanged": <Optional, UTC time in seconds that indicates when this data
                                        was last changed, this can be used to detect if local cache needs to be refreshed. If this attribute is not
                                        specified the data should not be cached>
        "items": [
            {
                "id": < Unique identity for this item within the ickStream concept >,
                "text": < Textual representation of this item >,
                "sortText": < Optional, if specified it represents the appropriate sorting order >,
                "type": < Type of item, set to "track" or "stream" >,
                "streamingRefs": [ < Optional, streaming references for this item,
                                                     can be a single track or a continuous stream >
                    {
                        "format": < format of stream, for example "mp3" >,
                        "intermediate": <Optional, true if pointed to a redirected stream >,
                        "sampleRate": <Optional, sample rate, for example 44100 >,
                        "sampleSize": <Optional, sample size, for example 16 >,
                        "channels": <Optional, number of channels, for example 2 >,
                        "streamFormatInformation": <Optional, additional format information about the
                                                    stream, for example aac container type >
                        "url": < Url of this streaming reference >
                    },
                    ...
                ],
                "image": < Optional, URL to an image which represent this item >,
                "itemAttributes": {
                    < Model attribute identity >: <Model attribute value>
                    ...
                }
            },
            ...
        ]
    }
}
```

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")