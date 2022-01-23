Remove a previously existing device

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeDevice",
    "params": {
        "deviceId": <Identity of the device to remove>,
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": < true if successful, else false >
}
```

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")