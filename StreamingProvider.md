# Introduction

Below follows some information about what you should think about when
considering letting us integrate your service in the ickStream Music
Platform.

# Benefits with ickStream Music Platform

There are a number of benefits to integrate your service with the
ickStream Music Platform.

Some benefits for you as streaming service provider:

  - You don't have to care about differences between different end user
    devices
      - Your service will be automatically available on all end user
        devices supporting ickStream Music Platform
      - If you don't want your service to be available on all type of
        devices you can specify exactly which ones you want to allow
  - If you would like, you don't have to care about making your own
    smart phone/tablet remote controls, they are all provided by
    ickStream
  - Another platform where you can attract new users who currently
    aren't using your service

Some benefits for your end users:

  - Ability to access online and local music from one place
  - Search and create aggregated favourites and playlists from any music
    source available
  - Rate tracks and recommend music to friends
  - Control and play music to and from any device supporting the
    ickStream Music Platform

# Defining your API

Do make it possible for us to integrate your service into the ickStream
Music Platform you need to offer us an API.

Some general aspects to think about when designing your API

  - Try to use JSON (or XML if JSON isn't possible) as API format
  - Try to use http/https as communication protocol
  - Make it possible to request items in chunks, so we can first request
    item 0-99 and then as a separate request item 100-199 and so on
  - If possible make sure to expose a count of the total number of
    available items even though we only request item 0-99
  - Try to sort the result in a logical order or offer a possibility for
    us to request the items in a certain order.
      - Typically we want to request lists of friends, favourite
        artists, favourite albums, favourite playlists, geographical
        regions and radio stations in alphabetic order.
      - Search result it's usually best to return with the most relevant
        matches first.
      - Tracks on an album is typically best to return in the order they
        appear on the album.
      - Tracks in a playlist is typically best to return in the order
        they appear in the playlist.
  - Regarding information of track objects returned
      - Try to expose track number as a separate field instead of
        prefixing the title with the track number
      - Try to expose disc number to make it possible for us to display
        multi disc albums in a better way
  - Give each object returned from the API a unique identity. This is
    important since the user will be able to reference tracks, albums,
    artists, playlists in ickStream Music Platform and to be able to
    retrieve information about referenced objects it's important that
    each artist, album, track, playlist have a unique identity.
  - Try to make the API as stateless as possible since this will make it
    easier for you to scale up to more users.

## Music on Demand service API

Typically a music on demand service should offer API functions to

  - Authenticate user
      - Our preference is to use OAuth since this means that we don't
        have to store any user credentials on our servers and its
        possible to allow the user to manually invalidate an OAuth
        access token
  - Get information about the authenticated in user
      - If e-mail is exposed we can let the user login to ickStream
        using the credentials of your service (if OAuth is used for
        authentication our servers will never see the user password)
  - Search for artists
  - Search for albums
  - Search for tracks
  - Search for playlists
  - Get information about a specific track
  - Get information about a specific album
  - Get information about a specific artist
  - Get information about a specific playlist
  - Get all albums by a certain artist
  - Get all tracks for a certain album
  - Get all tracks for a certain playlist
  - If a favorite concept is supported in the service
      - Get favourite artists
      - Get favourite albums
      - Get favourite playlists
      - Get favourite albums by a certain artist
      - Optionally, get all favourite tracks
      - Optionally, get favourite tracks for a certain album
      - Mark/Unmark an album as favourite
      - Mark/Unmark an artist as favourite
      - Mark/Unmark a playlist as favourite
      - Optionally, mark/unmark a track as favourite
  - If a friend concept is supported in the service
      - Search for other users (to later be able to add them as friends)
      - Get all friends
      - Get favourite artists for a friend
      - Get favourite albums for a friend
      - Get favourite albums for a friend by a certain artist
      - Get favourite playlists for a friend
      - Mark/Unmark a user as friend
  - Get a streaming url
      - One alternative is to expose this always when sending back
        information about a track
      - Another alternative is to have a separate function which is used
        to request a temporary streaming url only valid for a limited
        time
      - Regarding streaming format, you should consider to:
          - Offer a possibility to request preferred format or preferred
            quality
          - Always provide information about which format the streaming
            url returned is using
          - Try to use MP3 for lossy compressed music, while AAC and WMA
            is supported there are significantly more devices that
            support MP3 streams.
          - Try to use FLAC for lossless compressed music
  - If recommendations/reviews/editor picks are supported
      - Get all recommendation/review/editor categories available
      - Get all artists/albums/tracks for a certain
        recommendation/review/editor category

## Internet radio service API

Typically an internet radio service should offer API functions to

  - If authentication is required
      - Authenticate user
          - Our preference is to use OAuth since this means that we
            don't have to store any user credentials on our servers and
            its possible to allow the user to manually invalidate an
            OAuth access token
      - Get information about the authenticated in user
          - If e-mail is exposed we can let the user login to ickStream
            using the credentials of your service (if OAuth is used for
            authentication our servers will never see the user password)
  - Search for radio stations
  - Get all countries/geographical regions
  - Get all radio stations in a certain country/geographical region
  - Get information about a specific radio station
  - Optionally, get information about regions in the same geographical
    area as the end user device
      - Since the request will go though our cloud server it's important
        that you offer us a way to pass the IP-address of the end user
        device, using X-Forwarded-For HTTP header is preferred.
  - If a favorite concept is supported in the service
      - Get favourite radio stations
      - Mark/Unmark a radio station as favourite
  - If a friend concept is supported in the service
      - Search for other users (to later be able to add them as friends)
      - Get all friends
      - Get favourite radio stations for a friend
      - Mark/Unmark a user as friend
  - Get a streaming url
      - One alternative is to expose this always when sending back
        information about a radio station
      - Another alternative is to have a separate function which is used
        to request a temporary streaming url only valid for a limited
        time
      - Regarding streaming format, you should consider to:
          - Offer a possibility to request preferred format or preferred
            quality
          - Always provide information about which format the streaming
            url returned is using
          - Try to use MP3 for lossy compressed music, while AAC and WMA
            is supported there are significantly more devices that
            support MP3 streams.
          - Try to use FLAC for lossless compressed music
  - If recommendations/reviews/editor picks are supported
      - Get all recommendation/review/editor categories available
      - Get all radio stations for a certain
        recommendation/review/editor category

We can integrate using OPML, but in this case the integration will work
best if you can define the principles of the OPML structure you use in
advance so we can take advantage of this in our integration.

# Integration of your content in ickStream Music Platform

We will integrate your service by wrapping it in an implementation of
the [Content Access Protocol](API/Content_Access_Protocol "wikilink")
and this will be exposed to the end user device.

Streamed content is always streamed directly from your service to the
end user device, our wrapped service will just give the streaming url to
the end user device.

You API will not be exposed to the end user device, all API access will
go through the integration deployed on our cloud servers. User
credentials will also not be exposed to the end user device, they will
be handled either on your service (if you use OAuth) or on our cloud
servers (if you only provide a username/password authentication method).

# Integration with ickStream players in your own app

After we have integrated your content in ickStream, it's also possible
for you to integrate support for ickStream in your own app and by doing
this allow end users to play your content on ickStream players and
control them from your own app. The advantage of this is that end users
can continue to use your app with the branding and user interface of
your choice and are still able to play content on ickStream player
devices remotely controlled from your app which also will continue to
play music in the background if your app is closed by the user.

For more details see the separate page for [streaming provider app
integration](StreamingProviderAppIntegration "wikilink")