Get information of a track in the cloud library. The intention with this
method is to make it possible to easily show current rating, tags,
loved/banned status in a now playing screen displaying currently playing
track. You don't want to call this method for a long list of tracks as
it will result in too many round-trips and likely poor performance.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getTrack",
    "params": {
        "trackId": <the track identity if the track to retrieve information about>,
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "lastChanged": <the time when the track was modified last>,
        "loved": <true if the track is loved>
        "banned": <true if the track is banned>
        "rating": <Optional, an integer between 1-100 that represents the rating of the track>
        "tags": [ <An array of strings representing the tags which are attached to the track>
                ...
        ],
        "track": {
                "id": < Globally unique track identity, for example soundcloud:track:somenicetrack >,
            "type": < Type of item, one of [track|stream] >,
                "text": < Text representation of this track >,
                "sortText": < Optional, if specified it represents the appropriate sorting order >,
                "image": < Optional, URL to an image which represent this item >
                "streamingRefs": [
                        {
                                "format": <format of stream, for example "mp3" >,
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

  - **result** is not provided unless the track exists in the cloud
    library

[Category:Library Management
Protocol](Category:Library_Management_Protocol "wikilink")