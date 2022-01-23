The findItems command will return items with in a specified context.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findItems",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
        "contextId": < Optional, restrict result to specified context >
        "language": <Optional, language which is preferred, if not specified the default language will be used>
        ...
    }
}
```

Some important things to notice:

  - **contextId**
      - A number of standard values exists:
          - **myMusic** : Context which contains all artists, albums,
            tracks in the service which current user has marked as
            favorite
          - **friendsMusic** : Context which contains information about
            your friends music
          - **allMusic** : Context which contains all artists, albums,
            tracks available in the service
          - **allRadio** : Context which contains all internet radio
            stations available in the service
      - Additional context can be supported by a content server, the
        above is just the default which should be possible to use
        towards all content services where they are applicable.

If artist objects are available, the Controller should be able to
request them with:

``` javascript
"params": {
    "contextId": "myMusic",
    "type": "artist"
}
```

The same should also work for other "type" value supported by the
content service.

For more information about exactly what additional parameters the
findItems method support for a specific content service, see the
[getProtocolDescription2](../Content_Access_Protocol/getProtocolDescription2 "wikilink")
method.

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "offset": <numeric index of the first item which is part of this response,
                           this is set based on the "offset" parameter provided in the request >
        "count": < number of items in this response, if "count" has been specified in request
                           it is the maximum value of this item >,
        "countAll": <Optional, total number of items available within the specified "context">,
        "lastChanged": <Optional, UTC time in seconds that indicates when this data
                                        was last changed, this can be used to detect if local cache needs to be refreshed. If this attribute is not
                                        specified the data should not be cached>
        "items": [
            {
                "id": < Unique identity for this item within the ickStream concept >,
                "text": < Textual representation of this item >,
                "sortText": < Optional, if specified it represents the appropriate sorting order >,
                "type": < Type of item >,
                            "preferredChildItems": [ < Optional, list of item types that are most suitable to offer a method to access based on this item. Deprecated, replaced with preferredChildRequest in API version 2.0 and later >
                            ]
                "preferredChildRequest": <Optional, reference to request identity in getProtocolDescription2 method. Introduced in API version 2.0 >
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
                },
                "tracks": { < Optional, parameters to use in findItems method to retrieve
                                              all track objects below this object
                    ...
                },
            },
            ...
        ]
    }
}
```

Some important things to notice:

  - **id**
      - Should be unique within the ickStream concept
      - Shall be composed as: <serviceId>:<uniqueItemId>
          - <serviceId> should be the same as exposed from ickStream
            Cloud Core findServices method
          - <uniqueItemId> should be unique within the context of this
            content service
      - For example: soundcloud:artist:someartist
  - **type**
      - Type of item, see documentation of [item
        attributes](../Content_Access_Protocol/Item_attributes "wikilink")
        for more information of available types.
      - Note that when browsing for other items related to the selected
        item, the next call to findItems should contain a parameter
        named as <type>Id which matches the **id** attribute. For
        example, let's say the id=soundcloud:artist:someartist and
        type=artist then browsing for albums by this artist would be
        issues using type=album and
        artistId=soundcloud:artist:someartist.
      - It's important that a controller as a minimum can support
        browsing using the structure returned from
        [getPreferredMenus](../Content_Access_Protocol/getPreferredMenus "wikilink")
        method also for new item types which wasn't known when the
        controller was designed.
  - **preferredChildItems**
      - Replaced with **preferredChildRequest** in API version 2.0 and
        later
      - Optional, contains one or several item type which is suitable to
        offer a way to show the user based on the current item
      - The controller can't rely on this attribute existing, but when
        it exists it's recommended that it uses it
      - This is typically set to end a recursion to indicate to the
        controller that it's most suitable that sub menus under this
        item contains items of a certain type
  - **preferredChildRequest**
      - Introduced in API version 2.0
      - Optional, contains a reference to the request in
        getProtocolDescription2 method which is suitable to use to
        retrieve sub items based on current item
      - The controller can't rely on this attribute existing, but when
        it exists it's recommended that it uses it
      - This is typically set to end a recursion to indicate to the
        controller that it's most suitable that sub menus under this
        item contains items of a certain type
  - **tracks**
      - Optional, contains the parameter combination that should be used
        in call to findItems to get all items of type=track one or
        several levels below the current item. This is typically used
        when the user hit "Play" button and the controller want to get
        all tracks to send to the current playlist.
  - **itemAttributes**
      - The model attributes can be flat or hierarchical and the
        Controller have to look at the "itemtype" attribute to know how
        to interpret the contents inside the "itemAttributes" value. See
        below for more information about the model attributes.
  - **streamingRefs**
      - This is an array of streaming references, can either be an
        individual track or a continuous internet radio stream
      - The **streamingRefs** attribute is optional, if not provided or
        set to null for an object of type=track or type=stream, the
        streaming references are instead returned from the
        [getItemStreamingRef](../Content_Access_Protocol/getItemStreamingRef "wikilink")
        method.
      - The "url" might redirected to the final streaming url when first
        accessed, this is typically the case for many streaming services
        where the "url" field points to ickStream Cloud but ickStream
        Cloud will just redirect to the final url provided by the
        premium streaming provider when the player starts to play.
      - The "format" attribute needs to be one of:
          - audio/flac - For FLAC files
          - audio/ogg - For OGG files or streams
          - audio/mpeg - For MP3 files or streams
          - audio/m4a - For ALAC files or streams
          - audio/wav - For WAV files or streams
          - audio/x-pcm - For PCM streams
          - audio/x-aiff - For AIFF files or streams
          - audio/aac - For AAC files or streams
          - audio/x-ms-wma - For WMA files or streams
          - audio/rdio - For Rdio references, in this case the "url"
            field contains "rdio:t3923655" where t3923655 is the Rdio
            track identifier
          - Other values than the above may be used but will typically
            not work in all controllers or players.
      - The **intermediate** is a special indication which is used to
        indicate to the player that it have to provide the ickStream
        device access token in the header of the request. The stream
        will URL will be redirected to the real stream when accessed.
        The real stream is typically temporary and won't be valid long
        unless you start playing it so the player need to repeat the
        procedure every time the stream is played if **intermediate** is
        true. When accessing a streaming reference with
        **intermediate**=true you always have to provide an
        Authorization header in same way as for other requests. This is
        not required for streaming references without **intermediate**
        flag set.
      - Streaming references is normally only provided for individual
        tracks and continuous internet radio streams, for albums the
        Controller has to make a separate request to the proper
        "itemlists" entry to request the tracks for the album and then
        put all the individual tracks in the playlist.
      - For example:

<!-- end list -->

``` javascript
"streamingRefs": [
    {
        "url": "http://example.org/track.flac",
        "format": "audio/flac"
    },
    {
        "url": "http://example.org/track.mp3",
        "format": "audio/mpeg"
    }
}
```

  - **countAll**
      - The countAll item is optional and the Controller can't trust it
        will be there, if no countAll item exist the Controller need to
        keep requesting new data chunks until it receives a response
        where count is set to 0.
  - **count**
      - If no count is specified in the request or the specified count
        is too large, ickStream Cloud will reject the request. This risk
        only occurs if count is not specified or a count larger than 200
        is specified.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
