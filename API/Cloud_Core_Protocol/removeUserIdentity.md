Remove a user identity from the current user account. To add an identity
to a user see [Add identity to user
account](../Add_identity_to_user_account "wikilink")

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeUserIdentity",
    "params": {
        "type": <Type of identity, for example "email">,
        "identity": <The actual identity, for example "user1@example.com">
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": < true if successful, else false >
}
```

Some important things to note:

  - It's not possible to remove the last identity bound to the user, the
    reason for this is that it would make it impossible to login as this
    user. In this case this method will return false.

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
