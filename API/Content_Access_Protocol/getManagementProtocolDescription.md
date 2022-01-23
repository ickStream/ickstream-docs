Retrieves information about which item times that can be added/removed
in which contexts using the
[addItem](../Content_Access_Protocol/addItem "wikilink") and
[removeItem](../Content_Access_Protocol/removeItem "wikilink") methods
in the [Content Access
Protocol](../Content_Access_Protocol "wikilink").

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getManagementProtocolDescription",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
    }
}
```

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "offset": <numeric index of the first item which is part of this response,
                           this is set based on the "offset" parameter provided in the request >
        "count": < number of items in this response, if "count" has been specified in request
                           it is the maximum value of this item >,
        "countAll": <total number of items available, ignoring any limitation specified
                             with offset and count request parameters>,
        "items": [
            { < First context supported >
                "contextId": < The id of this browsing context >
                "supportedTypes": [
                    < List of types which can be added/removed with
                                          addItem and removeItem methods>
                ]
            }
        ]
    }
}
```

A bit more detail:

  - **contextId**
      - Can be one of the standard values which all content services
        should offer if applicable
          - **myMusic** Music which has been marked as favorites in the
            backend content service by current user
  - **supportedTypes**
      - Lists item types which can be added with
        [addItem](../Content_Access_Protocol/addItem "wikilink") and
        removed with
        [removeItem](../Content_Access_Protocol/removeItem "wikilink")
        methods
      - See documentation of [item
        attributes](../Content_Access_Protocol/Item_attributes "wikilink")
        for more information of available types.
          - For example:
              - \["artist","track"\]: Means that it's possible to add
                either tracks or artists to the myMusic context with the
                [addItem](../Content_Access_Protocol/addItem "wikilink")
                method
              - \["artist"\]: Means that it's only possible to add
                artists to the myMusic context
  - If the **items** array is empty, it means that it's not possible to
    use the
    [addItem](../Content_Access_Protocol/addItem "wikilink")/[removeItem](../Content_Access_Protocol/removeItem "wikilink")
    methods for this content service

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
