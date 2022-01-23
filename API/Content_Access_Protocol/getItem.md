The getItem method will return an item if it exist in the specified
context, one typical scenario where this method is of interest is to
verify if a certain item is part of myMusic context or not to be able to
offer menus to add/remove it to myMusic context.

Note that the getItem method is not guaranteed to be fast, so it's
strongly recommended to not call it for a long list of items.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getItem",
    "params": {
        "contextId": < The context to retrieve the item from >
        "itemId": < The identity of the item to retrieve >
        "language": <Optional, language which is preferred, if not specified the default language will be used>
    }
}
```

Some important things to notice:

  - **contextId**
      - See more information in the description about findItems method
      - Context is a mandatory parameter here and the method will only
        return the item if it exists in the specified context

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < Request identity, see JSON-RPC specification for more information >,
    "result": {
        "id": < Unique identity for this item within the ickStream concept >,
        "text": < Textual representation of this item >,
        "sortText": < Optional, if specified it represents the appropriate sorting order >,
        "type": < Type of item >,
                "preferredChildItems": [ < Optional, list of item types that are most suitable to offer a method to access based on this item. Deprecated, replaced with preferredChildRequest in API version 2.0 and later >
                ]
        "preferredChildRequest": <Optional, reference to request identity in getProtocolDescription2 method. Introduced in API version 2.0 >
        "streamingRefs": [ < Optioanl, streaming references for this item, can be a single track or a continuous stream >
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
        "tracks": { < Optional, parameters to use in findItems method to retrieve all track objects below this object
            ...
        },
    },
}
```

Or if the item doesn't exist, it simply returns:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < Request identity, see JSON-RPC specification for more information >,
}
```

For more details of each parameter in the result, see the documentation
of the result from
[findItems](../Content_Access_Protocol/findItems "wikilink") method

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
