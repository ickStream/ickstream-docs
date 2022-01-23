Find available authentication providers which can be used to
login/register a new user account

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findAuthenticationProviders",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "offset": <numeric index of the first item which is part of this response,
                           this is set based on the "offset" parameter provided in the request >
        "count": < number of items in this response, if "count" has been specified in request
                           it is the maximum value of this item >,
        "countAll": < Total number of authentication providers available >,
        "items": [
            {
                "id": < Unique identity for this authentication provider >,
                "name": < User friendly name of this authentication provider >,
                "icon": < Icon representing this authentication provider >,
                "url": < Endpoint to use to login/register using this authentication provider >,
                "addIdentityUrl": < Endpoint to use when adding an additional identity to an existing user
                        using this authentication provider >
            },
            ...
        ]
    }
}
```

Some important things to note:

  - The **url** value will be without the protocol version number at the
    end, it's the client which decide which version of the protocol to
    use when accessing the service.
  - The **addIdentityUrl** attribute can be used if a controller want to
    have a native UI to add identities to a user account, see [separate
    page for adding identities to a user account for more
    information](../Add_identity_to_user_account "wikilink")

[Category:Cloud Core Authentication
Protocol](Category:Cloud_Core_Authentication_Protocol "wikilink")
