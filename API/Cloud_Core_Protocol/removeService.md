Remove a service from this user. To add services to a user see [Add
service to user account](../Add_service_to_user_account "wikilink")

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "removeService",
    "params": {
        "serviceId": <Identity of the service to remove>,
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

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
