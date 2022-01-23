# Examples of usage with artist/album/tracks based service

## Retrieving top level menus

### To retrieve top level item lists available in the service

<font color="red">NOTE\! This part is under construction and shouldn't
be trusted at the moment</font>

``` javascript
{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": {
        "contextId": "myMusic",
        "type": "menu"
    }
}
```

Which could result in a repsonse like:

``` javascript
{
    count = 2
    items = [
        {
            "id": "soundcloud:myMusic",
            "text": "My Library",
            "type": "menu",
        },
        {
            "id": "soundcloud:myMusic/artists",
            "text": "Artists",
            "type": "menu"
        },
        {
            "id": "soundcloud:myMusic/albums",
            "text": "Albums",
            "type": "menu"
        },
        {
            "id": "soundcloud:myMusic/tracks",
            "text": "Tracks",
            "type": "menu"
        },
        {
            "id": "soundcloud:top100",
            "text": "Top 100",
            "type": "menu",
        },
        {
            "id": "soundcloud:top100/artists",
            "text": "Top 100 Artists",
            "type": "menu",
            "parentNode": "soundcloud:top100"
        },
        {
            "id": "soundcloud:top100/tracks",
            "text": "Top 100 Tracks",
            "type": "menu",
            "parentNode": "soundcloud:top100"
        }
    ]
}
```

Which typically would result in a menu like

`* My Music`
`  * Artists`
`  * Albums`
`  * Tracks`
`* Top 100`
`  * Top 100 Artists`
`  * Top 100 Tracks`

But it's up to the controller if it likes to use the server provided
menu or not.

# Retrieving sub menus/content

## Examples of usage of findItems

### To retrieve a list of all artists available in my favorite artists, albums, tracks in the service

``` javascript
{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": {
        "contextId": "myMusic",
        "type": "artist"
    }
}
```

Which could result in a response like:

``` javascript
{
    count = 2
    },
    items = [
        {
            "id": "soundcloud:artist:contributor1",
            "text": "First Artist",
            "type": "artist",
            "tracks" {
                "contextId": "myMusic",
                "artistId": "soundcloud:artist:contributor1",
                "type": "track"
            }
        },
        {
            "id": "soundcloud:artist:contributor2",
            "text": "Second Artist",
            "type": "artist",
            "tracks" {
                "contextId": "myMusic",
                "artistId": "soundcloud:artist:contributor2",
                "type": "track"
            }
        }
    ]
}
```

### To retrieve a list of all tracks available in my favorite artists, albums, tracks in the service where "First Artist" is involved:

``` javascript
{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": {
        "contextId": "soundcloud:myMusic",
        "artistId": "soundcloud:artist:contributor1",
        "type":"track"
    }
}
```

Which could result in a response like:

``` javascript
{
    count = 2,
    items = [
        {
            "id": ""soundcloud:track:track1",
            "text": "1. A nice track",
            "type": "track",
            "streamingRefs": [
                < streaming references, explained separately >,
            ],
            "itemAttributes": {
                < model data, explained separately >
            }
        },
        {
            "id": ""soundcloud:track:track2",
            "text": "2. Another nice track",
            "type": "track",
            "streamingRefs": [
                < streaming references, explained separately >,
            ],
            "itemAttributes": {
                < model data, explained separately >
            },
        }
    ]
}
```

# Examples of usage of findItems for internet radio station service

## Retrieve top level menus

### Retrieve top level items for RadioTime

This sample is based on [1](http://opml.radiotime.com)

``` javascript
{
    "jsonrpc": 2.0,
    "id": 1,
    "method": "findItems",
    "params": {
        "contextId": "allRadio",
        "type": "menu"
    }
}
```

Which gives the response:

``` javascript
{
  "jsonrpc" : "2.0",
  "id" : "1",
  "result" : {
    "count" : 8,
    "countAll" : 8,
    "offset" : 0,
    "items" : [ {
      "id" : "radiotime:allRadio",
      "text" : "Internet Radio"
    }, {
      "id" : "radiotime:menu:c=local",
      "parentNode" : "radiotime:allRadio",
      "text" : "Local Radio",
      "type" : "menu",
    },

     ...

     {
      "id" : "radiotime:menu:id=r0",
      "parentNode" : "radiotime:allRadio",
      "text" : "By Location",
      "type" : "menu",
    } ]
  }
}
```

### Browsing into "Internet Radio/By Location":

``` javascript
{
    "jsonrpc": 2.0,
    "id": 1,
    "method": "findItems",
    "params": {
         "menuId": "radiotime:menu:id=r0"
    }
}
```

Which gives the response:

``` javascript
{
  "jsonrpc" : "2.0",
  "id" : "1",
  "result" : {
    "count" : 8,
    "countAll" : 8,
    "offset" : 0,
    "items" : [ {
      "id" : "radiotime:menu:id=r101215",
      "text" : "Africa",
      "type" : "menu",
    },

      ...

      {
      "id" : "radiotime:menu:id=r101217",
      "text" : "Europe",
      "type" : "menu",
    } ]
  }
}
```

### Navigating into "Internet Radio/Europe/Germany/Berlin"

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

Some things to note:

  - The "radiotime:menu:id=r100772" value is found in the id attribute
    of the parent menu for "Germany", it's just not displayed in this
    sample to save some space.

The response would be something like:

``` javascript
{
  "jsonrpc" : "2.0",
  "id" : "1",
  "result" : {
    "count" : 68,
    "countAll" : 68,
    "offset" : 0,
    "items" : [ {
      "id" : "radiotime:menu:id=r100772.stations",
      "text" : "All Stations",
      "type" : "menu"
    }, {
      "id" : "radiotime:radio:id=s126577",
      "parentNode" : "radiotime:menu:id=r100772.stations",
      "text" : "88vier 88.4 (Public)",
      "type" : "stream",
      "image" : "http://radiotime-logos.s3.amazonaws.com/s126577q.png",
      "streamingRefs" : [ {
        "format" : "mp3",
        "url" : "http://opml.radiotime.com/Tune.ashx?id=s126577"
      } ]
    },

      ...

     {
      "id" : "radiotime:menu:id=r100772.pivotGenre",
      "text" : "By Genre",
      "type" : "menu"
    },

      ...

       {
      "id" : "radiotime:menu:id=r100772&filter=g22",
      "parentNode" : "radiotime:menu:id=r100772.pivotGenre",
      "text" : "World",
      "type" : "menu",
    }, {
      "id" : "radiotime:menu:id=r100772&filter=g40",
      "parentNode" : "radiotime:menu:id=r100772.pivotGenre",
      "text" : "World Talk",
      "type" : "menu",
    } ]
  }
}
```

Some things to note:

  - The menu combines:
      - hierarchical items like "All Stations" and "By Genre"
      - with "By Genre/World" and "By Genre/World Talk"
      - with internet radio stream items like "All Stations/88vier 88.4
        (Public)"
  - type=stream is used instead of type=track for the streamable items
    since this is a continuous stream and not a single track

# Retrieving a single item

## Examples of usage of getItem

### To retrieve a single deezer album

``` javascript
{
     "jsonrpc": "2.0",
     "id": 12354,
     "method": "getItem",
     "params": {
          "contextId": "allMusic",
          "itemId":"deezer:album:75046"
     }
}
```

Which results in a response like:

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : 12354,
   "result" :
      {
         "id" : "deezer:album:75046",
         "text" : "Super Colossal",
         "type" : "album",
         "image" : "http://api.deezer.com/2.0/album/75046/image",
         "itemAttributes" :
            {
               "id" : "deezer:album:75046",
               "name" : "Super Colossal",
               "image" : "http://api.deezer.com/2.0/album/75046/image",
               "mainArtists" :
                  [
                     {
                        "id" : "deezer:artist:496",
                        "name" : "Joe Satriani"
                     }
                  ]
            }
      }
}
```

### To retrieve a single deezer album which doesn't exist in the myMusic context

``` javascript
{
     "jsonrpc": "2.0",
     "id": 12354,
     "method": "getItem",
     "params": {
          "contextId": "myMusic",
          "itemId":"deezer:album:75046"
     }
}
```

Which if the album doesn't exist results in a reply like:

``` javascript
{
    "jsonrpc": "2.0",
    "id":12354
}
```

# Model attribute examples

Below follows some sample of the \*itemAttributes\* structure for
various item types. (The values are just dummy data at the moment
because we haven't found a suitable reference sample yet)

## Model attributes for an artist

``` javascript
"itemAttributes": {
    "id": "spotify:artist:composer1",
    "name": "Wolfgang Amadeus Mozart"
}
```

## Model attributes for an album

``` javascript
"itemAttributes": {
    "id": "spotify:album:yyy",
    "name:" "Some album",
    "image": "http://somehost/somealbum.jpg",
}
```

## Model attributes for a track

``` javascript
"itemAttributes": {
    "id": "spotify:track:xxx",
    "name": "Some track",
    "image": "http://somehost/sometrack.jpg",
    "trackNumber": 1,
    "disc": "A",
    "album": {
        "id": "spotify:album:yyy",
        "name:" "Some album",
        "image": "http://somehost/somealbum.jpg",
        "year": "2010",
        "mainArtists": [
            {
                "id": "spotify:artist:artist1",
                "name": "Some famous classical soloist"
            }
        ],
    },
    "mainArtists": [
        {
            "id": "spotify:artist:artist1",
            "name": "Some famous classical soloist"
        }
    ],
    "composer": [
        {
            "id": "spotify:artist:composer1",
            "name": "Wolfgang Amadeus Mozart"
        }
    ],
    "conductor": [
        {
            "id": "spotify:artist:conductor1",
            "name": "Some conductor"
        }
    ],
    "performers": [
        {
            "id": "spotify:artist:orchestra1",
            "name": "Some orchestra"
        }
    ],
    "year": "2010",
    "categories": [
        {
            "id": "spotify:category.genre:classical",
            "categoryType": "genre",
            "name": "Classical"
        },
        {
            "id": "spotify:category.genre:chamber",
            "categoryType": "genre",
            "name": "Chamber Music"
        }
    ]
}
```

## Model attributes for a genre

``` javascript
{
    "id": "spotify:category.genre:classical",
    "categoryType": "genre",
    "name": "Classical"
}
```

## Model attributes for a playlist

``` javascript
{
    "id": "spotify:playlist:playlist1",
    "playlistType": "static",
    "name": "My Classical Playlist"
},
```

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")