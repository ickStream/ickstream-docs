Adjust the current playback volume of the player Can be provided either
as a specific value level or as a +/- delta which indicates how much to
increase/decrease the volume from its current level Also used to
mute/unmute a player

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setVolume",
    "params": {
        "volumeLevel": < Optional, absolute volume level, a float between 0 to 1 >
        "relativeVolumeLevel" <Optional, relative volume level, a float between 0 to 1 >
        "muted": <Optional, true indicates that the device should be muted, false that it should be unmuted >
    }
}
```

**Specific information:**

  - Either **volumeLevel** or **relativeVolumeLevel** or **muted** needs
    to be specified
  - Specifying 0.1 in **relativeVolumeLevel** means that the volume
    level will be increased with 10%, specifying -0.1 means it will be
    decreased with 10%
  - If a controller wants to change volume and unmute in a single
    command it specifies one of of the volume level attributes and also
    set \*muted\* attribute to **true**

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "volumeLevel": < Current volume level after the change >
        "muted": < true means that the player is currently muted, false means that it's not muted >
    }
}
```

[Category:Player Protocol](Category:Player_Protocol "wikilink")