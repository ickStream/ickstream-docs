### Overview

Introduced in API version: 2.0

The getProtocolDescritpion2 method use used to retrieve information
about available requests which could be passed to the
[findItems](../Content_Access_Protocol/findItems "wikilink") method.
The purpose of having a discovery protocol like this is to make it
possible for a controller to check which queries that are available in a
specific content service and apply it's own user interface layout
principles based on what's available.

**Request**:

``` javascript
{
    "jsonrpc": 2.0,
    "id": < A unique number used to correlate requests with responses, see JSON-RPC specification for more information >,
    "method": "getProtocolDescription2",
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
            { < First context supported >
                "contextId": < The id of this browsing context >
                "name": < The user friendly name for this browsing context >
                "images": [ <Optional, array of images/logos available that should be used to represent this context>
                    {
                        "url": <URL to the image>
                        "type": <Type of image, for example "small", "banner" or "custom">
                        "width": <Width of image>
                        "height": <Height of image>
                    },
                    ...
                ],
                "supportedRequests": {
                    "<type>" => { < Type of object which this request returns >
                        "<request identity>" => { <Unique identity of the request, should be "contextId:uniqueIdentityWithinContext" >
                            "values" => { <Optional, list of parameter values that should be added to the request
                                "< some parameter id >": < Optional, hard coded value supported
                                                                                                            for this parameter in this request >
                                ...
                            },
                            "parameters": [ < parameters that should be included in the request > ]
                        },
                        ...
                    },
                    ...
                }
            },
            ...
        ]
    ]
}
```

A bit more detail:

  - **contextId**
      - Can be one of the standard values which all content services
        should offer if applicable
          - **myMusic** Music which has been marked as favorites in the
            backend content service by current user
          - **allMusic** All music available in the content service
          - **allRadio** All continuous internet radio streams available
            in the content service
          - **hotMusic** "Charts" items
  - **type**
      - Can be "**none**" or a specific item type, the meaning of
        "**none**" is that the request can return any kind of items.
      - See documentation of [item
        attributes](../Content_Access_Protocol/Item_attributes "wikilink")
        for more information of available types.
      - It's important that a controller as a minimum can support
        browsing also for new item types which wasn't known when the
        controller was designed.
  - **name**
      - User friendly name for the context, this typically only used for
        custom contexts which are specific for a specific content
        service, for the standard contexts the controller typically
        render its own text.
  - **supportedRequests**
      - Hash with all available type of requests, there is one entry in
        the ash for each supported object type
  - **type**
      - There should be one entry in **supportedRequests** for each type
        value supported by the content service
      - The "type" can be set to "none" and in this case it means that
        any of the type of objects might be returned from the request.
        Note: this is only a return parameter, "none" shall not be
        specified as a type-parameter for an actual "findItems" request
        but the type-parameter shall be omitted instead\!
  - **values**
      - List of hard coded parameter values that should be included in
        the request. Note that value of **contextId** and **type**
        parameters do not have to be listed here, they should be taken
        from the **contextId** attribute in the top of the structure and
        the key in the **supportedRequests** attribute.
  - **parameters**
      - Lists required parameters
      - There are some special parameters which a controller can take
        advantage of, if they are included as supported parameters for a
        request:
          - **contextId** - See description above of available contexts
          - **type** - See documentation of [item
            attributes](../Content_Access_Protocol/Item_attributes "wikilink")
            for more information of available types.
          - **roleId** - Can be one of:
              - albumartist - Artist which is considered to be the main
                artist of an album
              - composer - Composer of the music
              - conductor - Conductor of the music

### Parameter interpretation

\["contextId","type"\]: Means that it's possible to issue a query to
findItems like:

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "type":"album"
    ]
}
```

  - Note that the values of "contextId" parameter is restricted to the
    values the "contextId" parameter at the top
  - Note that the values of "type" parameter is restricted to the value
    specified in the key of the **supportedRequests** hash

\["type","artistId"\]: Means that it's possible to issue a query to
findItems like:

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "artistId": "someservice:artist:412",
        "type":"album"
    ]
}
```

or

``` javascript

{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "artistId": "someservice:artist:412",
        "type":"album"
    ]
}
```

  - Even though "contextId" isn't specified as a parameter, the content
    service will always allow a "contextId" parameter and just ignore it
    if it isn't required.
  - Both "contextId" and "type" is restricted to values defined higher
    up in the hierarchy as described above
  - The "artistId" can use any value which is applicable to this service

The parameter "search" is a special parameter that's used for searching,
so if supportedParameters contains an entry like this:

``` javascript

"supportedRequests": {
    "albums" => {
        "parameters":
            [ "contextId","type" ]
    },
    "searchForAlbums" => {
        "parameters":
            [ "contextId","type","search" ],
    }
}
```

it means that it's possible to get a list of all albums but it's also
possible to search for albums with a title that contains a certain text,
like:

``` javascript
{
    "jsonrpc": 2.0,
    "id": 123,
    "method": "findItems",
    "params": [
        "contextId": "myMusic",
        "type":"album",
        "search": "Love"
    ]
}
```

For more example of how the getProtocolDescription2 result looks like,
take a look at [the sample
page](../Content_Access_Protocol/Discovery_Examples_2 "wikilink")

### Request identities

It's up to each service to decide which request identities to provide
but as a general rule the following standard request identities should
be used if these requests are supported by the service. For other
request it's up to the content service to select a suitable name. The
purpose of the standardised request identity is to make it possible for
a controller to use look if a certain request identity exists instead of
doing in-depth analysis of the parameters array returned for each
request from the getProtocolDescription2 method.

The request identities will always be defined using the syntax:
<contextId>:<requestIdWithinContext>

#### Standard requests within the myMusic and allMusic contexts

  - **myMusic:searchForArtists** or **allMusic:searchForArtists**
      - Search all artists within the context matching the search
        criteria specified with **search** parameter
  - **myMusic:artists**
      - List all artists within the context
  - **myMusic:artistsInCategory** or **allMusic:artistsInCategory**
      - List all artists within the context within a genre defined with
        a **categoryId** parameter

<!-- end list -->

  - **myMusic:searchForAlbums** or **allMusic:searchForAlbums**
      - Search all albums within the context matching the search
        criteria specified with **search** parameter
  - **myMusic:albums**
      - List all albums within the context
  - **myMusic:albumsInCategory** or **allMusic:albumsInCategory**
      - List all albums within the context within a genre defined with a
        **categoryId** parameter
  - **myMusic:albumsByArtist** or **allMusic:albumsByArtist**
      - List all albums within the context by an artist defined with an
        **artistId** parameter
  - **myMusic:albumsByArtistInCategory** or
    **allMusic:albumsByArtistInCategory**
      - List all albums within the context by an artist defined with an
        **artistId** parameter and within a genre defined with a
        **categoryId** parameter

<!-- end list -->

  - **myMusic:searchForPlaylists** or **allMusic:searchForPlaylists**
      - Search all playlists within the context matching the search
        criteria specified with **search** parameter
  - **myMusic:playlists**
      - List all playlists within the context
  - **myMusic:playlistsInCategory** or **allMusic:playlistsInCategory**
      - List all playlists within the context within a genre defined
        with a **categoryId** parameter

<!-- end list -->

  - **myMusic:searchForTracks** or **allMusic:searchForTracks**
      - Search all tracks within the context matching the search
        criteria specified with **search** parameter
  - **myMusic::tracks**
      - List all tracks within the context
  - **myMusic:tracksInCategory** or **allMusic:tracksInCategory**
      - List all tracks within the context within a genre defined with a
        **categoryId** parameter

<!-- end list -->

  - **myMusic:searchForCategories** or **allMusic:searchForCategories**
      - Search all categories within the context matching the search
        criteria specified with **search** parameter
  - **myMusic:categories** or **allMusic:categories**
      - List all genres within the context
  - **myMusic:tracksByArtist** or **allMusic:tracksByArtist**
      - List all tracks within the context by an artist defined with an
        **artistId** parameter
  - **myMusic:tracksByArtistInCategory** or
    **allMusic:tracksByArtistInCategory**
      - List all tracks within the context by an artist defined with an
        **artistId** parameter and within a genre defined with a
        **categoryId** parameter
  - **myMusic:tracksOnAlbum** or **allMusic:tracksOnAlbum**
      - List all tracks within the context on an album defined with an
        **albumId** parameter
  - **myMusic:tracksInPlaylist** or **allMusic:tracksInPlaylist**
      - List all tracks within the context in a playlist defined with a
        **playlistId** parameter

<!-- end list -->

  - **allMusic:searchForPersons**
      - Search all users within the context matching the search criteria
        specified with **search** parameter

#### Standard requests wtihin the friendsMusic contexts

  - **friendsMusic:friends**
      - List all friends to the current user

<!-- end list -->

  - **friendsMusic:artistsForFriend**
      - List all artists for a friend defined with a **friendId**
        parameter
  - **friendsMusic:artistsForFriendInCategory**
      - List all artists for a friend defined with a **friendId**
        parameter within a genre defined with a **categoryId** parameter

<!-- end list -->

  - **friendsMusic:albums**
      - List all albums from any friend
  - **friendsMusic:albumsForFriend**
      - List all albums for a friend defined with a **friendId**
        parameter
  - **friendsMusic:albumsForFriendInCategory**
      - List all albums for a friend defined with a **friendId**
        parameter within a genre defined with a **categoryId** parameter
  - **friendsMusic:albumsForFriendByArtist**
      - List all albums for a friend defined with a **friendId**
        parameter by an artist defined with an **artistId** parameter
  - **friendsMusic:albumsForFriendByArtistInCategory**
      - List all albums for a friend defined with a **friendId**
        parameter by an artist defined with an **artistId** parameter
        and within a genre defined with a **categoryId** parameter

<!-- end list -->

  - **friendsMusic:playlistsForFriend**
      - List all playlists for a friend defined with a **friendId**
        parameter

<!-- end list -->

  - **friendsMusic:tracks**
      - List all tracks from any friend
  - **friendsMusic:tracksForFriend**
      - List all tracks for a friend defined with a **friendId**
        parameter
  - **friendsMusic:tracksForFriendInCategory**
      - List all tracks for a friend defined with a **friendId**
        parameter within a genre defined with a **categoryId** parameter
  - **friendsMusic:tracksForFriendByArtistInCategory**
      - List all tracks for a friend defined with a **friendId**
        parameter by an artist defined with an **artistId** parameter
        and within a genre defined with a **categoryId** parameter
  - **friendsMusic::tracksForFriendByArtist**
      - List all tracks for a friend defined with a **friendId**
        parameter by an artist defined with an **artistId** parameter
  - **friendsMusic::tracksForFriendOnAlbum**
      - List all tracks for a friend defined with a **friendId**
        parameter on an album defined with an **albumId** parameter
  - **friendsMusic::tracksForFriendInPlaylist**
      - List all tracks for a friend defined with a **friendId**
        parameter in a playlist defined with a **playlistId** parameter

<!-- end list -->

  - **friendsMusic:categoriesForFriend**
      - List all genres for a friend defined with a **friendId**
        parameter

[Category:Content Access
Protocol](Category:Content_Access_Protocol "wikilink")
