The purpose of the addItem method is to add an item (artist, album,
track, ...) to a content service, typically this is used as follows:

1.  Search in allMusic context
2.  Call addItem to add the interesting item to the myMusic context
3.  Browse myMusic context to list favorite artists, albums, tracks
    which have been added above

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "addItem",
    "params": {
        "contextId": < Context which the item should be added to>
        "itemId": < Item identifier for the item to add, this corresponds to the *id* attribute returned from findItems method>
    }
}
```

**Specific information**

  - The addItem can only be called with items which have been marked as
    supported in the
    [getManagementProtocolDescription](../Content_Access_Protocol/getManagementProtocolDescription "wikilink")
    method in the [Content Access
    Protocol](../Content_Access_Protocol "wikilink") for the content
    service.

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "result": < true or false >
}
```

**Specific information**

  - **result** will be set to true if the item is successfully added or
    if the item already existed in the content service context and
    didn't have to be added again.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
