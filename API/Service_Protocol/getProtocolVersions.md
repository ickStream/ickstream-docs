### getProtocolVersions

Get information about the supported protocol versions

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getProtocolVersions",
    "params": {
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "minVersion": <The minimum protocol version supported>,
        "maxVersion": <The maximum protocol version supported>
    }
}
```

**Specific information:**

  - The version numbers will be exposed as a <major> and a <minor>
    parts, for example 1.2 (where major=1 and minor=2)
  - A client can assume that it can use a protocol with same
    <major>/<minor> version which it has been designed for or with the
    same <major> version but a higher <minor> version
      - <minor> part is changed when introducing new methods/parameters
        or other compatible changes.
      - <major> part is changed when removing/renaming methods or
        attributes or other incompatible changes.
  - A client can use the version information to detect if new methods
    are supported or not

[Category:Service Protocol](Category:Service_Protocol "wikilink")