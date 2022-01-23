Remove a track in the cloud library

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeTrack",
    "params": {
        "trackId": <the track identity if the tracks to remove, the same value as reported in track.id
                    in the saveTrack method>,
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

  - If the track doesn't exist the method will still succeed, the only
    scenario where it fails is if the track exist but couldn't be
    removed for some reason

[Category:Library Management
Protocol](Category:Library_Management_Protocol "wikilink")