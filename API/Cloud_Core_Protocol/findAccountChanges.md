Finds all changes done in the current user account

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findAccountChanges",
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
        "countAll": <Total number of account changes available for current user>,
        "items": [
            {
                "id": < Unique identity for this change >,
                "occurrenceTimestamp": < Timestamp which indicates when the change occurred, as unix epoch >
                "type": < Indication of which type of change this represents >
                "text": < Textual representation of this item >,
                "itemAttributes" < Optional, structure with detailed attributes related to this change >

            },
            ...
        ]
    }
}
```

Some important things to note:

  - The result is sorted with the latest account changes first
  - **type**: The available type values are
      - USER_CREATED - The user have been initially created
      - DEVICE_ADDED - A device have been added
      - DEVICE_RECONFIGURED - A device have been activated after a
        factory reset or re-installation of the app
      - DEVICE_REMOVED - A device have been removed
      - SERVICE_ADDED - A service have been added
      - SERVICE_RECONFIGURED - A service have been reconfigured, for
        example changed credentials to streaming service
      - SERVICE_REMOVED - A service have been removed
      - IDENTITY_ADDED - A user identity have been added, for example
        linked the user to a Facebook account
      - IDENTITY_REMOVED - A user identity have been removed

The **itemAttributes** structure looks different depending of the
**type** attribute value.

  - DEVICE_ADDED or DEVICE_REMOVED

<!-- end list -->

``` javascript
{
    "deviceId": < Identity of the device >
    "deviceModel": < Model of the device >
    "deviceName": < Name of the device >
}
```

  - DEVICE_RECONFIGURED

<!-- end list -->

``` javascript
{
    "previousDeviceId": < Previous identity of the device >
    "previousDeviceName": < Previous name of the device >
    "deviceId": < Identity of the device >
    "deviceModel": < Model of the device >
    "deviceName": < Name of the device >
}
```

  - SERVICE_ADDED, SERVICE_RECONFIGURED or SERVICE_REMOVED

<!-- end list -->

``` javascript
{
    "serviceId": < Identity of the service >
    "serviceName": < Name of the service >
}
```

  - IDENTITY_ADDED or IDENTITY_REMOVED

<!-- end list -->

``` javascript
{
    "type": < Type of identity >
    "identity": < The unique identity, for example an e-mail address if type=email >
}
```

  - USER_CREATED

No itemAttributes structure

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")