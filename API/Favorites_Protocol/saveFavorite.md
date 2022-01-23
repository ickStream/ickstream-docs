Create or update a favorite

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "saveFavorite",
    "params": {
        "id": <Optional, the globally unique identity, maximum 36 characters>,
        "parentId": <Optional, the identity of the parent favorite, if any>
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
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": <The globally unique identity, maximum 36 characters>,
        "parentId": <Optional, the identity of the parent favorite, if any>
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
    }
}
```

**Specific information:**

  - The **id** attribute is generated if not provided by the caller, if
    the caller provides it, the caller much ensure it's globally unique
    and consists of maximum 36 characters.
  - If the **id** attribute is not provided a new favorite will always
    be created, if **id** is provided a new favorite will be created if
    no previous favorite with the same id exists, if a previous favorite
    with same **id** exists it will be updated.
  - The **type** attribute have a special value **"local"** which
    indicates that this favorite refers to a local menu in the specific
    controller or other controllers of the same model. See documentation
    of [item
    attributes](../Content_Access_Protocol/Item_attributes "wikilink")
    for more information of other available types.
  - The contents of the inner **params** structure is completely
    controller customizable if "type"="local". For the other type values
    it is expected that a controller can issue a [Content Access
    Protocol](../Content_Access_Protocol "wikilink") findItems request
    with the attributes provided in the **params** structure.
  - All favorite types can have childs which refer to them with a
    "parentId" attribute

[Category:Favorites Protocol](Category:Favorites_Protocol "wikilink")
