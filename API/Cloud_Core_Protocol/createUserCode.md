Create a temporary user code which should be used when adding services
or identities to an existing user account, see [Add service to user
account](../Add_service_to_user_account "wikilink") and [Add identity
to user account](../Add_identity_to_user_account "wikilink") for more
information.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "createUserCode",
    "params": {
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": < a string containing the temporary user code >
}
```

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
