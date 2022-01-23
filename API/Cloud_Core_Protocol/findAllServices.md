Find all available content services provided by ickStream Cloud
independent if the current user have access to them or not.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "findAllServices",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
        "type": <Optional, type of service to retrieve >
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
        "countAll": < Total number of services available for current user >,
        "items": [
            {
                "id": < Unique identity for this service >,
                "type": < Type of service >,
                "mainCategory": <Optional, the main category which this service belongs to >
                "name": < User friendly name of this service >,
                "url": < Endpoint for the content access protocol implementation provided by this service >,
                "addServiceUrl": < Endpoint to use when adding a service to a user account >
                "images": [ <Optional, array of images/logos available that should be used to represent this service>
                    {
                        "url": <URL to the image>
                        "serviceUrl": <Main url/prefix to use when accessing "service://" URIs provided by this service>
                        "type": <Type of image>
                        "width": <Width of image>
                        "height": <Height of image>
                    },
                    ...
                ]
            },
            ...
        ]
    }
}
```

Some important things to note:

  - **type**: If not specified the result will not be restricted to a
    specific type, the available value of type are:
      - **content**: Content services implementing [Content Access
        Protocol](../Content_Access_Protocol "wikilink")
      - **playlistmanagement**: Playlist management services
        implementing the [Playlist Management
        Protocol](../Playlist_Management_Protocol "wikilink")
      - **librarymanagement**: Library management services implementing
        the [Library Management
        Protocol](../Library_Management_Protocol "wikilink")
      - **scrobble**: Scrobble services implementing the [Scrobble
        Protocol](../Scrobble_Protocol "wikilink")
      - **favorites**: Favorites services implementing the [Favorites
        Protocol](../Favorites_Protocol "wikilink")
  - **images** : See documentation of
    [getServiceInformation](../Service_Protocol/getServiceInformation "wikilink")
    method for more information regarding possible values.
  - **mainCategory** : See documentation of
    [getServiceInformation](../Service_Protocol/getServiceInformation "wikilink")
    method for more information regarding possible values.
  - The **url** value will be without the protocol version number at the
    end, it's the client which decide which version of the protocol to
    use when accessing the service.
  - The **addServiceUrl** attribute can be used if a controller want to
    have a native UI to add services to a user account, see [separate
    page for adding services for more
    information](../Add_service_to_user_account "wikilink")

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")
