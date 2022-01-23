Finds devices own by current user

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findDevices",
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
    "id": < The request identity >
    "result": {
        "offset": <numeric index of the first item which is part of this response,
                           this is set based on the "offset" parameter provided in the request >
        "count": < number of items in this response, if "count" has been specified in request it is the maximum value of this item >,
        "countAll": <Total number of devices available for current user>,
        "items": [
            {
                "id": < Unique identity for this device >,
                "text": < Textual representation of this item >,
                "model": < Model identification for this device >,
                "address": < Current local IP-address of this device >,
                "publicAddress": < Current public IP-address of this device (may not be accessible due to firewalls) >,
            },
            ...
        ]
    }
}
```

Some important things to note:

  - **id**: The device id is not the same as the access token which is
    used in the **Authorization** header, the access token can only be
    retrieved by registering the device with the
    [addDeviceWithHardwareId](../Cloud_Core_Protocol/addDeviceWithHardwareId "wikilink")
    method.
  - The **model** attribute follows the syntax <manufacturer>/<model>,
    you will be assigned which model identifier to use when applying for
    an API key

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
