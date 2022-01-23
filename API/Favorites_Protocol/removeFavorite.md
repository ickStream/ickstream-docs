Remove a favorite

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeFavorite",
    "params": {
        "id": <The globally unique identity of the favorite to remove>,
    }
}
```

**Specific information:**

  - If a favorite have other favorites which refers to it with a
    **parentId**, the whole tree will be removed.

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <true if success or not existing, else false>
}
```

[Category:Favorites Protocol](Category:Favorites_Protocol "wikilink")