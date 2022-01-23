Introduced in API version: 2.0

The getPreferredMenus method is used to retrieve information about
preferred menus with references to request parameters which could be
passed to the
[findItems](../Content_Access_Protocol/findItems "wikilink") method.
The purpose of having a discovery protocol like this is to make it
possible for a controller to check which menus that are preferred to
show in a specific content service and apply it's own user interface
layout principles based on what's preferred by the content service.

The menu structure returned by this method is not necessarily complete
but only describes a top-level hierarchy. Where this hierarchy ends (has
no more children) a controller shall use requests as specified by the
preferredChildRequest field in the item returned by the lowest-level
request. Where no such field is present (e.g. for playable items;
streams or tracks) the controller has to rely on it's own logic to
create new lower-level menus.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getPreferredMenus",
    "params": {
        "offset": <Optional, numeric index of first item to retrieve >
        "count": <Optional, number of items to retrieve >
        "language": <Optional, language which is preferred, if not specified the default language will be used>
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
        "countAll": <total number of items available, ignoring any limitation specified with
                             offset and count request parameters>,
        "items": [
            { < First preferred menu >
                "type": < The type of menu, can contain values [search|browse] >
                "menuType": < The type of content which this menu contains >
                "text": < The user friendly name for this browse menu >
                "images": [ <Optional, array of images/logos available that should be used to represent this context>
                    {
                        "url": <URL to the image>
                        "type": <Type of image>
                        "width": <Width of image>
                        "height": <Height of image>
                    },
                    ...
                ],
                "childItems": [ < Optional, cannot be specified if childRequest is specified, contains static child items >
                    {
                        < First child item, same attribute as above >
                    }
                "childRequest": { <Optional, cannot be specified if childItems is specified, contains information about the request to issue to retrieve child items >
                    {
                        "request": < The unique identity of the request from the getProtocolDescription2 method, for example "myMusic:artists"
                        "childItems": [ < Optional, cannot be specified if childRequest is specified, contains static child items >
                        "childRequest": { <Optional, cannot be specified if childItems is specified, contains information about the request to issue to retrieve child items >
                    }
            },
            ...
        ]
    ]
}
```

A bit more detail:

  - **type**
      - The intention of the **type** attribute is to make it possible
        for a controller to skip search menus and instead have a central
        search field which does search in all content services.
  - **menuType**
      - The **menuType** attribute contains information of the type of
        objects that this menu contains, the value is a dot separated
        string, for example "artists.recommendations.mostPlayed". The
        intention is that a controller should be able to provide its own
        icons for different type of menu and look at this attribute to
        render a suitable icon. The principle with the dot separated
        value is that a controller can select the right icon according
        to the hierarchy, so:
          - If the controller knows about the
            "artists.recommendations.mostPlayed" value it uses this icon
          - If not, but the controller knows about the
            "artists.recommendations" value it uses this icon
          - if not, but the controller knows about the "artists" value
            it uses this icon
          - if not the controller render a generic menu icon
      - The following list is a guideline which values a controller
        might want to implement icons for (remember to allow dot
        extensions after these values)
          - artists - A menu that browse music by artists
          - artists.composers - A menu that browse music by composers
          - artists.conductors - A menu that browse music by conductors
          - albums - A menu that browse music by albums
          - playlists - A menu that browse music by playlists
          - tracks - A menu that browse music by tracks
          - times - A menu that browse music by time
          - times.decades - A menu that browse music by decades
          - times.years - A menu that browse music by years
          - folders - A menu that browse music by folders
          - categories - A menu that browse music by genres
          - categories.genres - A menu that browse music by genres
          - categories.moods - A menu that browse music by moods
          - friends - A menu that browse music by friends
          - charts - A menu that browse music by different kind of
            charts
          - recommendations - A menu that browse different kind of
            recommendations from various sources
          - radios - A menu that browse internet radio stations
          - search - A menu that offers the user to search for
            information
          - artists.search - A menu that offers the user to search for
            artists
          - albums.search - A menu that offers the user to search for
            albums
          - playlists.search - A menu that offers the user to search for
            playlists
          - tracks.search - A menu that offers the user to search for
            tracks
          - friends.search - A menu that offers the user to search for
            friends
          - radios.search - A menu that offers the user to search for
            radio stations
  - **images** - If images are specified in this attribute there has to
    be at least one image of the following types. If an **images**
    attribute is specified it means that the service want that a
    specific image should be used so in this case the menu should be
    represented with this image instead of providing an image locally in
    the controller.
      - icon_bw - A monochrome square icon with alpha channel as
        typically used by iOS and Android
      - icon_rgb - An square icon that uses full color display and may
        not have a transparent background
  - **childItems**
      - This is a recursive structure
      - In some cases a controller might want to skip static items, for
        example if there is only one top level menu of type 'browse'
  - **childRequest**
      - This is a recursive structure
      - The **request** attribute points to a request identity available
        in the
        [getProtocolDescription2](../Content_Access_Protocol/getProtocolDescription2 "wikilink")
        method

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
