Add or update a track in the cloud library

The number of typical usage scenarios are:

  - A user pushes a "love" icon in the user interface: Call this method
    with **loved**=true and **track** structure containing all metadata
    for the track and leave all other attributes empty.
  - A user pushes a "ban" icon in the user interface: Call this method
    with **banned**=true and **track** structure containing all metadata
    for the track and leave all other attributes empty.
  - A user pushes a rating star icon in the user interface: Call this
    method with **rating** attribute set to the rating number and
    **track** structure containing all metadata for the track and leave
    all other attributes empty.
  - A user pushes an "add tag" button in the user interface: Call this
    method with **tagsToAdd** attribute set to the tag text and
    **track** structure containing all metadata for the track and leave
    all other attributes empty.
  - A user pushes an "remove tag" button in the user interface: Call
    this method with **tagsToRemove** attribute set to the tag text and
    **track** structure containing all metadata for the track and leave
    all other attributes empty. In the remove track scenario, it's
    actually also possible to just provide the **id** attribute in the
    **track** structure as we already know that the track exists and
    just want to remove the tag from it.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "saveTrack",
    "params": {
        "occurrenceTimestamp": <Optional, the time when the track was modified>,
        "loved": <true if the track is loved, false to remove a previously set love indication,
                  if not specified the previously set love indication will be kept>
        "banned": <true if the track is banned, false to remove a previously set ban indication,
                   if not specified the previously set ban indication will be kept>
        "rating": <Optional, an integer between 0-100,
                   0 is used to remove a previously set rating,
                   1-100 is used to set the rating, if not specified the previously set rating wil be kept>
        "tags": [ <An array of strings representing the tags which should be attached to the track,
                   if not specifed the previously set tags will be kept>
            ...
        ],
        "tagsToAdd": [ <An array of string representing the tags that should be added on top of
                        previously set tags, this attribute will never remove any previously set tags>
            ...
        ],
        "tagsToRemove": [ <An array of string representing the tags that should be removed from the
                           previously set tags, this attribute will never add any new tags and it
                           won't fail if a tag that isn't previously set is specified>
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

  - The **occurrenceTimestamp** attribute is normaly not provided, it
    just exists to be able to upload a previously defined library from a
    local server
  - The **occurrenceTimestamp** will be stored in the cloud library as a
    **lastChanged** attribute and due to this it will only be used if
    the previous **lastChanged** value is older than the one provided
    with this method call.
  - The library data registered with this function will be maintained
    per user, none of it will be tied to a specific device
  - All attributes except for the **id** in the **track** structure can
    be left empty if you don't want to modify previously existing data,
    but note that doing so will result in pretty useless information in
    case no previous data exists.
  - For more details about individual parameters in beneath the
    **track** structure see [Content Access
    Protocol](../Content_Access_Protocol "wikilink")
    [findItems](../Content_Access_Protocol/findItems "wikilink")
    method.

[Category:Library Management
Protocol](Category:Library_Management_Protocol "wikilink")
