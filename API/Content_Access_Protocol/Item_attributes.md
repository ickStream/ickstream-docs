# Principles

All items returned from
[findItems](../Content_Access_Protocol/findItems "wikilink") and
[getItem](../Content_Access_Protocol/getItem "wikilink") methods have a
**type**, depending on the type different structures are provided in the
**itemAttributes** structure.

To be able to handle protocol changes in the future a controller must be
able to handle new **type** values but it can for unknown type values
either ignore the **itemAttributes** structure or just use the generic
parameters which are available on all type of items.

Below follows the currently defined type values.

# Item attributes

## Mandatory attributes (for all type of items)

  - **id**:
    <unique identity for this item within the context of this service >
  - **name**: \< textual representation of this item within the context
    of this service \>

Generally, the only mandatory model attribute to provide is "id" and
"name" the other attributes might not exist so a controller need to
handle the situation when they don't exist.

## artist

Represents an artist or band who is performing, composing or conducting
the music.

For an item of type=artist the following attributes may exist in
**itemAttributes** structure

  - **id**: \< unique identity for this artist within the ickStream
    concept \>
  - **name**: \< artist name \>
  - **image**: \< Image url to artist \>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
 "musicbrainz": "29762c82-bb92-4acd-b1fb-09cc4da250d2",
     "spotify": "spotify:artist:2yzxX2DI9LFK8VFTyW2zZ8"
```

## album

For an item of type=album the following attributes may exist in
**itemAttributes** structure

  - **id**:
    <unique identity for this album within the context of this service >
  - **name**: \< album name \>
  - **image**: <Image url to album cover >
  - **mainArtists**: \<An array of artist objects which represents main
    artist of album, see below for itemtype=artist for exact content of
    each artist object \>
  - **year**: \< The release year of the album \>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
 "musicbrainz": "7896d321-3b81-3b8b-8e6c-2b3ee8474a3d",
     "spotify": "spotify:album:6aDSlt7I0dFRtJ8gxR9Gh7"
```

## playlist

For an item of type=playlist the following attributes may exist in
**itemAttributes** structure

  - **id**:
    <unique identity for this playlist within the ickStream concept >
  - **playlistType**: \<type of playlist this represent, "static" or
    "dynamic", "dynamic" means that the playlist might have different
    contents each time it is requested \>
  - **name**: \< playlist name \>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
"spotify": "spotify:someuser:playlist:1JxzRVqYhAF9UsM2q04UC8"
```

## track

Represents a track with a fixed duration.

For an item of type=track the following attributes may exist in
**itemAttributes** structure

  - **id**:
    <unique identity for this track within the ickStream concept >
  - **name**: \< Unformatted track title, should not contain track and
    disc numbers \>
  - **image**: \< Image url to the track \>
  - **trackNumber**: \< Track number \>
  - **duration**: \<Optional, track duration in number of seconds\>
  - **disc**: \< Disc identity, only provided for multi disc releases \>
  - **album**: \< Used if tracks belongs to an album, see description
    for type=album for exact content \>
  - **mainArtists**: \< An array of artist objects which represents main
    artist of track, see description for type=artist for exact content
    of each artist object \>
  - **composers**: \< An array of artist objects which represent
    composers, see description for type=artist for exact content of each
    artist object \>
  - **conductors**: \< An array of artist objects which represent
    conductors, see description for type=artist for exact content of
    each artist object \>
  - **performers**: \< An array of artist objects which represent
    additional performers on the track, see description for type=artist
    for exact content of each artist object \>
  - **year**: \< The release year of the track \>
  - **categories**: \<An array of genre objects which represent
    categories this track is related to, see below for itemtype=category
    for exact content of each category object\>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
 "musicbrainz": "8142b073-82ed-402a-9e75-8ad8ee04ffe6",
     "spotify": "spotify:track:1JCWSZIHhXyw24LV5YpZ54"
```

## stream

Represents a stream of music which will continuously play, typically a
radio station.

For an item of type=stream the following attributes may exist in
**itemAttributes** structure

  - **id**:
    <unique identity for this track within the ickStream concept >
  - **name**: \< Unformatted track title, should not contain track and
    disc numbers \>
  - **image**: \< Image url to the track \>
  - **trackNumber**: \< Track number \>
  - **duration**: \<Optional, track duration in number of seconds, if
    this exists for a stream it just indicates when the currently
    playing track will end the stream will automatically continue
    streaming the next track in the radio station \>
  - **disc**: \< Disc identity, only provided for multi disc releases \>
  - **album**: \< Used if tracks belongs to an album, see description
    for type=album for exact content \>
  - **mainArtists**: \< An array of artist objects which represents main
    artist of track, see description for type=artist for exact content
    of each artist object \>
  - **composers**: \< An array of artist objects which represent
    composers, see description for type=artist for exact content of each
    artist object \>
  - **conductors**: \< An array of artist objects which represent
    conductors, see description for type=artist for exact content of
    each artist object \>
  - **performers**: \< An array of artist objects which represent
    additional performers on the track, see description for type=artist
    for exact content of each artist object \>
  - **year**: \< The release year of the track \>
  - **categories**: \<An array of genre objects which represent
    categories this track is related to, see below for itemtype=category
    for exact content of each category object\>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
 "musicbrainz": "8142b073-82ed-402a-9e75-8ad8ee04ffe6",
     "spotify": "spotify:track:1JCWSZIHhXyw24LV5YpZ54"
```

## category

For an item of type=category the following attributes may exist in
**itemAttributes** structure

  - **id**:
    <unique identity for this category within the ickStream concept >
  - **categoryType**: \<type of category this represent, for example
    "genre", "style", "mood", "tag"\>
  - **name**: \< category name \>
  - **identifiers**: \<Optional, a hash with identifiers for other
    services, the type of identifiers supported will be different for
    different services\>, for example

<!-- end list -->

``` javascript
 "discogs_genre": "Rock",
     "lastfm_tag": "rock"
```

## friend

For an item of type=friend the following attributes may exist in
**itemAttributes** structure

  - **id**: \< unique identity for this artist within the ickStream
    concept \>
  - **name**: \< friend name \>
  - **image**: \< avatar or image url representing friend \>

## menu

Represents a generic textual item, typically used for menus defined by
the content service.

For an item of type=menu the following attributes may exist in
**itemAttributes** structure

  - **id**: \< unique identity for this menu item within the ickStream
    concept \>
  - **name**: \< textual representation \>
