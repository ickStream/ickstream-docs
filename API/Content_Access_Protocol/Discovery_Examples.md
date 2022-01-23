# SoundCloud

This SoundCloud content service basically supports:

  - Getting all favorite artists
  - Getting all artists available in SoundCloud
  - Getting all tracks for a specific artist
  - Getting all hot tracks

The result from the getProtocolDescription method to indicate this would
look like this:

``` javascript
[
   {
      "contextId" : "myMusic",
      "name" : "My Music",
      "supportedRequests" :
         [
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            },
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            }
         ]
   },
   {
      "contextId" : "allMusic",
      "name" : "All Music",
      "supportedRequests" :
         [
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type"],
                     ["contextId","type","search"]
                  ]
            },
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type","search"],
                     ["contextId","type","artistId"]
                  ]
            }
         ]
   },
   {
      "contextId" : "hotMusic",
      "name" : "Hot Music",
      "supportedRequests" :
         [
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            }
         ]
   }
]
```

Some things to note:

  - There is no request for **\["contextId","type","artistId"\]** in the
    "myMusic" context
      - This is an indication to the Controller that it's not possible
        to request a track list for a specific which is limited to the
        "myMusic" context
      - Typically a controller will in these cases instead use the
        **\["contextId", "type", "artistId"\]** request in "allMusic"
        context
  - In the context "allMusic" and type=track, the only existing
    \*parameters\* entry contains \["type","artistId"\], this means
    that:
      - It's not possible to ask for all tracks in the "allMusic"
        context, a Controller who wanted a list of all tracks would have
        to:
        1.  First request all artists in the "allMusic" context
        2.  Then for each artist request all tracks for that artist

# Rdio

This Rdio content service basically supports:

  - Getting all favorite artists
  - Getting all favorite albums
  - Getting all favorite albums for a specific artist
  - Getting all favorite tracks
  - Getting all favorite tracks for a specific album
  - Getting all favorite tracks for a specific artist

The result from the getProtocolDescription method to indicate this would
look like this:

``` javascript
[
   {
      "contextId" : "myMusic",
      "name" : "My Music",
      "supportedRequests" :
         [
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            },
            {
               "type" : "album",
               "parameters" :
                  [
                     ["contextId","type"],
                     ["contextId","type","artistId"]
                  ]
            },
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type","albumId"]
                  ]
            }
         ]
   }
]
```

Some things to note:

  - The only supported context is "myMusic"

# Deezer

This Deezer content service basically supports:

  - Getting all favorite artists
  - Getting all favorite albums
  - Getting all albums for a specific artist
  - Getting all favorite tracks
  - Getting all tracks for a specific album
  - Getting all categories where charts exists
  - Getting all artists available in the chart for a specific category
  - Getting all albums available in the chart for a specific category
  - Getting all tracks available in the chart for a specific category

The result from the getProtocolDescription method to indicate this would
look like this:

``` javascript
[
   {
      "contextId" : "myMusic",
      "name" : "My Music",
      "supportedRequests" :
         [
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            },
            {
               "type" : "album",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            },
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            }
         ]
   },
   {
      "contextId" : "allMusic",
      "name" : "All Music",
      "supportedRequests" :
         [
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type","search"]
                  ]
            },
            {
               "type" : "album",
               "parameters" :
                  [
                     ["contextId","type","search"],
                     ["contextId","type","artistId"]
                  ]
            },
            {
               "type" : "track",
               "parameters" :
                  [
                     ["contextId","type","search"],
                     ["contextId","type","albumId"]
                  ]
            }
         ]
   },
   {
      "contextId" : "allCharts",
      "name" : "Charts",
      "supportedRequests" :
         [
            {
               "type" : "category",
               "parameters" :
                  [
                     ["contextId","type"]
                  ]
            },
            {
               "type" : "artist",
               "parameters" :
                  [
                     ["contextId","type","categoryId"]
                  ]
            },
            {
               "type" : "album",
               "parameters" :
                  [
                     ["contextId","type","categoryId"]
                  ]
            }
         ]
   }
]
```

Some things to note:

  - It supports a custom context **allCharts**
      - Typically the Controller might want to either use the default
        structure provided by the findItems method for this context
      - Or use the **name** attribute value "Charts" when rendering this
        context in the user interface
  - In neither **myMusic** nor **allCharts** contexts there is a
    \*parameters\* entry for type=album that looks like:
    **\["contextId","type","artistId"\]**
      - The result is that the controller need to use the **allMusic**
        context to get a list of albums for a specific artist.
  - The definition for type=track only contains \*parameters\* entries
    for **\["contextId","type","search"\]** and
    **\["contextId","type","albumId"\]** and none of them contains
    **artistId**
      - This means that a controller that want all tracks for an artist,
        will have to first ask for all albums for the artist and then
        for each album ask for all tracks of the album.

# RadioTime

This RadioTime content service basically supports:

  - Getting all continuous streams below a specific menu
  - Getting all sub menus below a specific menu
  - Getting all continuous streams or sub menus below a specific menu

The result from the getProtocolDescription method to indicate this would
look like this:

``` javascript
[
   {
      "contextId" : "allRadio",
      "name" : "Internet Radio",
      "supportedRequests" :
         [
            {
               "type" : "stream",
               "parameters" :
                  [
                     ["contextId","type","menuId"]
                  ]
            },
            {
               "type" : "menu",
               "parameters" :
                  [
                     ["contextId","type"],
                     ["contextId","type","menuId"]
                  ]
            },
            {
               "parameters" :
                  [
                     ["contextId","menuId"]
                  ]
            }
         ]
   }
]
```

Some things to note:

  - There is only one \*parameters\* entry which doesn't contain
    **menuId** parameter
      - A controller would have to first issue a call to the
        **\["contextId","type"\]** parameter combination to get the top
        level menuId value to start browsing with
  - The **myMusic** context available for all the other content services
    is not available for this service

One of the **supportedRequests** entries doesn't specify a **type**
attribute, this means that either type=stream or type=menu objects will
be returned if issuing a request to findItems like:

``` javascript
{
    "jsonrpc": 2.0,
    "id": 1,
    "method": "findItems",
    "params": {
         "menuId": "radiotime:menu:id=r100772"
    }
}
```

In this case, the controller have to be aware that it needs to look at
the "type" attribute in the items returned to detect which kind of
objects it is.

For other content services issuing a request with type=track means that
all matching tracks will be returned even if the request was issued
several levels above. For "type=menu" this isn't the case, type=menu
will always just return the direct child items.

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")