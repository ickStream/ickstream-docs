### getAccountInformation

Get information about the account the user is using for the service.

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getAccountInformation",
    "params": {
    }
}
```

**Response:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < The request identity >,
    "result": {
        "id": <The unique identity the user is using in the service>,
        "name": <The user friendly name of the user account used in the service>,
        "image": <Optional, an image representing the user in the service>,
        "url": <Optional, a web page link which the user is using in the service>
    }
}
```

**Specific information:**

  - The whole **result** attribute is optional and can return null if
    the service doesn't have personalization options so it's tied to a
    user account. A content service which doesn't implement account
    management should just return null in the result attribute.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")