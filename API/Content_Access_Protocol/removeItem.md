The purpose of the **removeItem** method is to remove an item (artist,
album, track, ...) from a content service context which has previously
been added with the
[addItem](../Content_Access_Protocol/addItem "wikilink") method.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeItem",
    "params": {
        "contextId": < Context which the item should be added to>
        "itemId": < Item identifier for the item to remove, this corresponds to the *id* attribute returned from findItems method>
    }
}
```

**Specific information**

  - The removeItem can only be called with items which have been marked
    as supported in the
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

  - **result** will be set to true if the item is successfully removed
    or if the item didn't exist in the requested content service context
    and didn't have to be removed.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
