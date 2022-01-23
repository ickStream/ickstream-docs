Set the current repeat mode of the player

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setRepeatMode",
    "params": {
        "repeatMode": < A valid repeat mode >
    }
}
```

**Specific information:**

  - Valid repeatMode values are:
      - REPEAT_OFF - Player plays tracks to the end of playlist and
        then stop
      - REPEAT_PLAYLIST - Player plays tracks to the end of playlist
        and then restart from the beginning
      - REPEAT_PLAYLIST_AND_SHUFFLE - Player plays tracks to the end
        of the playlist, shuffle the complete playlist and then restart
        from the beginning

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "repeatMode": < The current repeat mode after the operation >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")