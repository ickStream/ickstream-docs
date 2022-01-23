Removed in API version 2.0 and later

The getProtocolDescritpion method use used to retrieve information about
available requests which could be passed to the
[findItems](../Content_Access_Protocol/findItems "wikilink") method.
The purpose of having a discovery protocol like this is to make it
possible for a controller to check which queries that are available in a
specific content service and apply it's own user interface layout
principles based on what's available.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getProtocolDescription",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
        "language": <Optional, language which is preferred, if not specified the default language will be used>
    }
}
```

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
        "countAll": <total number of items available, ignoring any limitation specified with
                             offset and count request parameters>,
        "items": [
            { < First context supported >
                "contextId": < The id of this browsing context >
                "name": < The user friendly name for this browsing context >
                "images": [ <Optional, array of images/logos available that should be used to represent this context>
                    {
                        "url": <URL to the image>
                        "type": <Type of image, for example "small", "banner" or "custom">
                        "width": <Width of image>
                        "height": <Height of image>
                    },
                    ...
                ]
                "supportedRequests": [
                    { < First request supported >
                        "type": < Optional, type of object which this request returns,
                                                          typically one of
                                                          [artist|album|track|playlist|category|menu} >,
                        "< some parameter id >": < Optional, hard coded value supported
                                                                           for this parameter in this request >
                        ...
                        "parameters": [ < parameter combinations available >
                            [ < List of parameters for first combination > ],
                            [ < List of parameters for second combination > ],
                            ...
                        ],
                    },
                    ...
                ]
            },
            ...
        ]
    ]
}
```

A bit more detail:

  - **contextId**
      - Can be one of the standard values which all content services
        should offer if applicable
          - **myMusic** Music which has been marked as favorites in the
            backend content service by current user
          - **allMusic** All music available in the content service
          - **allRadio** All continuous internet radio streams available
            in the content service
          - **hotMusic** "Charts" items
  - **name**
      - User friendly name for the context, this typically only used for
        custom contexts which are specific for a specific content
        service, for the standard contexts the controller typically
        render its own text.
  - **supportedRequests**
      - Lists available type of request, typically there is one entry in
        the array for each supported value of "type" attribute
  - **type**
      - There should be one entry in **supportedRequests** for each type
        value supported by the content service
      - The "type" attribute is optional, leaving it out means that any
        of the type of objects specified in other **supportedRequests**
        entries might be returned, see RadioTime sample later in this
        page for an example of this.
  - **parameters**
      - Lists available parameter combinations, see separate chapter
        below for more information:

### Parameter interpretation

\["contextId","type"\]: Means that it's possible to issue a query to
findItems like:

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "type":"album"
    ]
}
```

  - Note that the values of "contextId" parameter is restricted to the
    values the "contextId" parameter at the top
  - Note that the values of "type" parameter is restricted to the value
    specified in the "type" attribute in **supportedRequests** entry

\["type","artistId"\]: Means that it's possible to issue a query to
findItems like:

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "artistId": "someservice:artist:412",
        "type":"album"
    ]
}
```

or

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "artistId": "someservice:artist:412",
        "type":"album"
    ]
}
```

  - Even though "contextId" isn't specified as a parameter, the content
    service will always allow a "contextId" parameter and just ignore it
    if it isn't required.
  - Both "contextId" and "type" is restricted to values defined higher
    up in the hierarchy as described above
  - The "artistId" can use any value which is applicable to this service

The parameter "search" is a special parameter that's used for searching,
so if supportedParameters contains an entry like this:

``` javascript

"supportedRequests": [
    {
        "type": "album",
        "parameters": [
            [ "contextId","type","search" ],
            [ "contextId","type" ],
        ],
    }
]
```

it means that it's possible to get a list of all albums but it's also
possible to search for albums with a title that contains a certain text,
like:

``` javascript
{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "type":"album",
        "search": "Love"
    ]
}
```

For more example of how the getProtocolDescription result looks like,
take a look at [the sample
page](../Content_Access_Protocol/Discovery_Examples "wikilink")

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
