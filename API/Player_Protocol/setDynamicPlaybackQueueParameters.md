Set the parameters which should be used to retrieve new tracks when
QUEUE_DYNAMIC playback queue mode has been set with
[setPlaybackQueueMode](../Player_Protocol/setPlaybackQueueMode "wikilink")
method.

The result of this method is that the player will start retrieve tracks
to be played based on the specified parameters and automatically add
them to the playback queue.

The purpose is to be able to easily

  - Play random music you have access to
  - Play random music within your collection
  - Play random music which are part of one of your playlists
  - Play random music from your friends libraries
  - Play random music from a specific artist
  - Play random music from a specific playlist
  - Play random music from a specific category/genre

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
    "method": "setDynamicPlaybackQueueParameters",
    "params": {
        "services": [ <Tracks will only be retrieved from the specified services>
            {
                "service": <service identity>,
                "weight": <optional, weight as a float between 0.0 to 1.0, where not specified means 1.0>
                "selectionParameters": { <optional, selection parameters specific to a specific service
                                       this parameter overrides anything specified on the top level selectionParameters attributes>
                    "type": <Type of selection>,
                    "data": <JSON data that defines the selection logic>
                }
            },
            ...
        ],
        "numberOfLeadingItems": <Optional, number of tracks to get ahead of current position in the playback queue.
                                A default value of 10 should be used if not specified.>
        "maximumQueueLength": <Optional, maximum number of tracks totally in the playback queue. >
        "selectionParameters": { <selection parmeters, mandatory to specify unless specific selection parameters
                                               have been specified for each individual service under the services parameter>
            "type": <Type of selection>,
            "data": <JSON data that defines the selection logic>
        }
    }
}
```

**Specific information:**

  - When this method is called, the playback queue mode will
    automatically switched to **QUEUE_DYNAMIC**
  - The tracks which are in the current playback queue before this
    method is called should remain, a controller have to call
    [setTracks](../Player_Protocol/setTracks "wikilink") to clear or
    set the initial set of tracks if it want to initially load tracks
    based on the dynamic playback queue parameters.
  - The player is responsible to use the parameters defined to
    continuously call the
    [getNextDynamicPlaylistTracks](../Content_Access_Protocol/getNextDynamicPlaylistTracks "wikilink")
    method in the [Content Access
    Protocol](../Content_Access_Protocol "wikilink") to retrieve more
    tracks as needed.
      - The player should retrieve tracks according to the weight
        parameter specified in the services entries and ensure that the
        playback queue is always filled with the number of tracks
        specified in **numberOfLeadingItems** parameter.
      - The player should remove played tracks to ensure that the
        playback queue doesn't become larger than the value specified in
        **maximumQueueLength** parameter. If **maximumQueueLength** has
        not been specified the player should let the playback queue grow
        as much as possible before it starts removing played tracks from
        the queue.
  - For more detailed information about the **selectionParameters**
    parameter, see the documentation for
    [getNextDynamicPlaylistTracks](../Content_Access_Protocol/getNextDynamicPlaylistTracks "wikilink")
    method in the [Content Access
    Protocol](../Content_Access_Protocol "wikilink")

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "result": <true if tracks were successfully added >
        "playbackQueuePos": < Position of currently playing track within the playback queue >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")
