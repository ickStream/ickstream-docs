### getServiceInformation

Get information about the service

**Request:**

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getServiceInformation",
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
        "id": <The globally unique identity of the service>,
        "name": <The user friendly name of the service>,
        "type": <The type of service>,
        "mainCategory": <Optional, the main category which this service belongs to >
        "url": <Optional, the request url, only defined for services that use HTTP,
                        if not defined the assumption is that P2P protocol is used to communicate with service>
        "serviceUrl": <Base url which should be used for resolving service: URIs,
                               this is only defined if the service support service: URIs>
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
    }
}
```

**Specific information:**

  - **type**: The following service types exists
      - **content**: Content services implementing [Content Access
        Protocol](../Content_Access_Protocol "wikilink")
      - **librarymanagement**: Library management services implementing
        the [Library Management
        Protocol](../Library_Management_Protocol "wikilink")
      - **playlistmanagement**: Playlist management services
        implementing the [Playlist Management
        Protocol](../Playlist_Management_Protocol "wikilink")
      - **scrobble**: Scrobble services implementing the [Scrobble
        Protocol](../Scrobble_Protocol "wikilink")
      - **favorites**: Favorites services implementing the [Favorites
        Protocol](../Favorites_Protocol "wikilink")
  - **mainCategory** The following categories exists
      - **radio**: Internet radio online streaming service
      - **musicondemand**: Music on demand online streaming service
      - **localmusic**: Local music available on the local network or
        device
  - **serviceUrl**: Contains the value which "<service://>" should be
    converted to when the service expose "<service://>" URI's when
    returning references to content, can be references to images and/or
    streamingRefs urls
  - **images**
      - It's up to each service to decide which one to provide, but it's
        recommended that one image of each type is provided to make it
        possible for controllers to achieve a consistent presentation
        and layout.
      - **type** The following image types are available
          - icon_bw - A monochrome square icon with alpha channel as
            typically used by iOS and Android
          - icon_rgb - An square icon that uses full color display and
            may not have a transparent background
          - ribbon - A ribbon icon that can be used as an overlay on a
            content item from the service to indicate which service it
            comes from
          - image - A large image, can have any rectangular shape and
            orientation

[Category:Service Protocol](Category:Service_Protocol "wikilink")
