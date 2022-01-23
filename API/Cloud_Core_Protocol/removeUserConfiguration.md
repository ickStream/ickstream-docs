Removes user configuration for the current user.

The parameters remove can be any parameters set with the
[setUserConfiguration](../Cloud_Core_Protocol/setUserConfiguration "wikilink")
method by an application from the same developer/company. This means
that if you are developing both a player app and a controller app, both
these applications will be able to remove configuration set by the other
one. You won't be able to remove configuration set by apps developed by
other developers/companies.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeUserConfiguration",
    "params": [ < List of user configuration parameters to remove >
              "<param1>",<"param2">, ...
    ]
}
```

Some important things to note:

  - The **params** node is an array and not an object in this request
  - To get good performance the caller should remove all parameters it
    needs to remove in a single call instead of calling this method for
    each parameter

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <true if successful, else false>
}
```

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
