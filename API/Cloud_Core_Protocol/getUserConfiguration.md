Retrieves user configuration for the current user.

The parameters returned can be any parameters set with the
[setUserConfiguration](../Cloud_Core_Protocol/setUserConfiguration "wikilink")
method by an application from the same developer/company. This means
that if you are developing both a player app and a controller app both
these applications will be able to get configuration set by the other
one. You won't be able to get configuration set by apps developed by
other developers.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getUserConfiguration",
    "params": [ < List of user configuration parameters to retrieve >
              "<param1>",<"param2">, ...
    ]
}
```

Some important things to note:

  - The **params** node is an array and not an object in this request
  - To get good performance the caller should retrieve all parameters it
    needs in a single call instead of calling this method for each
    parameter

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "<param1>": < String value of user configuration parameter param1 >,
        ...
    }
}
```

Some important things to note:

  - All parameter values will be in string format
  - Only parameters which have been set will be returned

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
