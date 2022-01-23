Find all favorites matching the search criterias

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findFavorites",
    "params": {
        "offset": <Optional, the index of the first favorite to return>
        "count": <Optional, maximum number of favorites returned>
        "id": <Optional, the globally unique identity of a specific favorite to get>,
        "parentId": <Optional, get favorite which refers to this favorite as their parentId, or specify "" to get only top level nodes>,
        "search": <Optional, textual which must be part of the "text" attribute of the favorite>,
        "type": <Optional, restrict result to favorites of a specific type, can also be set to "local">
        "serviceId": <Optional, restrict result to favorites applicable to a certain service>
    }
}
```

**Specific information:**

  - If specifying **parentId**, only the immediate childs will be
    retrieved, "" in **parentId** means that you only want to get the
    top level nodes
  - The **type** parameter can be set to "local" or to the type of items
    to retrieve. See documentation of [item
    attributes](../Content_Access_Protocol/Item_attributes "wikilink")
    for more information of available item types.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "offset": <numeric index of the first item which is part of this response, this is set based on the "offset" parameter provided in the request >
        "count": < number of items in this response, if "count" has been specified in request it is the maximum value of this item >,
        "countAll": <Optional, total number of items available within the specified "context">,
        "items": [ <List of matching favorites >
            {
                "id": <The globally unique identity, maximum 36 characters>,
                "parentId": <Optional, the globally unique identity of the parent favorite>,
                "text": <Textual name of favorite to display to user>,
                "image": <Optional, Image representation of favorite>,
                "type": <Type of item or "local">
                "serviceId": <identity of service which the favorite refers to, might be omitted if type=local>
                "params": { <Parameter value pairs which is used to execute/browse to the favorite>
                        "attr1":"value1",
                        "attr2":42,
                        "attr3":false,
                        ...
                }
            },
            ...
        ]
    }
}
```

[Category:Favorites Protocol](Category:Favorites_Protocol "wikilink")
