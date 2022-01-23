Set one or several user configuration parameters for the current user.

Parameters are shared for all applications from a single
developer/company, so the same parameter can be set by two different
applications as long as they come from the same developer/company. If
the applications come from different developers/companies, their
parameters will be completely separated and can even have the same name.

If you are always setting a certain set of parameters, it's recommended
to store them as one user configuration parameter and instead use JSON,
XML or some other structured format for the parameter value. Just note
that the value is passed as a string in this request so you need to
ensure characters reserved by the JSON standard is properly escaped,
this usually happens automatically if you you a standard JSON library
for creating the JSON structures.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "setUserConfiguration",
    "params": { < All user configuration parameters to set with the corresponding string value>
        "<param1>": <string value of param1>
        "<param2>": <string value of param2>
    ]
}
```

Some important things to note:

  - To get good performance the caller should set all parameters it
    needs to set in a single call instead of calling this method for
    each parameter
  - Parameters not in included in a call will keep their old value
  - Calling a parameter with the value null will remove the parameter,
    this is the same as calling
    [removeUserConfiguration](../Cloud_Core_Protocol/removeUserConfiguration "wikilink")
    method

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": <true if successful >
}
```

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
