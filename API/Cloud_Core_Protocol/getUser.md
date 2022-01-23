Get information about current user

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getUser",
    "params": {
    }
}
```

**Response**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": < Unique identity for this user>,
        "name": < User friendly name for this user >,
        "country": < The country code of this user >,
        "identities": [
            {
                "type": < Type of identity, for example "email" or "soundcloud" >,
                "identity": < Unique identity within the context of the specified "type" >
            },
            ...
        ]
    }
}
```

Some important things to note:

  - **id**: This id should normally never be displayed to the end user,
    it's just an internal identity. This identity is not the same one as
    you use in the **Authorization** header.
  - **identities**: The identities array list all identities this user
    is known as, the user will be able to authenticate using all these
    identities. Note that type=email will be used when possible, so if a
    user is authenticated through Facebook it will be stored as
    type=email as Facebook have an API were ickStream Cloud can get the
    e-mail of current user.

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")