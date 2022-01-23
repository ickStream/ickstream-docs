## Overview

The purpose of this page is to give a brief instruction and guide how to
make an ickStream controller application.

I'll refer to git repositories below, to get access to these see the
separate page for [Source code
repositories](Source_code_repositories "wikilink")

In the main [API documentation page](Introduction "wikilink") there is
a section for "Protocols used in a controller", this is what a typical
controller would implement.

However, a bit more details follows below that describes how it works.

## Registration on local network

The controller need to register itself in the P2P module as described in
[ickP2p](ickP2p "wikilink") API

  - The interface is in /ickp2p/ickP2p.h in the
    [ickstream-p2p](Source_code_repositories#ickStream_P2P_module "wikilink")
    repository
  - The controller uses the P2P module and its
    [ickP2p](ickP2p "wikilink") API to register itself on the
    network and also for all communication on the local network, this is
    done by calling:
    1.  [ickP2pCreate](ickP2p/ickP2pCreate "wikilink") to setup a
        discovery context
    2.  [ickP2pRegisterMessageCallback](ickP2p/ickP2pRegisterMessageCallback "wikilink")
        to setup a callback which will be called when responses from
        players and content services on local network is received
    3.  [ickP2pRegisterDiscoveryCallback](ickP2p/ickP2pRegisterDiscoveryCallback "wikilink")
        to setup a callback which will be called when new players or
        content services is discovered on the local network. You will
        need this to be able to call the getServiceInformation method to
        get the IP address of the content service to resolve
        <service://>`<UUID/` image urls to a <http://><IP>`/` url which
        you can use to retrieve an image for items from a content
        service on local network. This method is also the way you will
        discover players available on local network so you can show them
        in a list in the controller.
    4.  [ickP2pAddInterface](ickP2p/ickP2pAddInterface "wikilink")
        to add a network card/device which should be used for discovery
    5.  [ickP2pResume](ickP2p/ickP2pResume "wikilink") to start
        discovery and announce itself on the network
  - If you like some sample code, you can take a look at the
    /daemon/ickSocketDaemon.c file in the
    [ickstream-squeezebox-player](Source_code_repositories#ickStream_Squeezebox_player "wikilink")
    repository and its handleInitialization function which does the
    initialization.

For reference a player will register itself on the network in similar
fashion and all responses and notifications from content services or
players will be received in the callback registered with the
ickP2pRegisterMessageCallback function.

## Authentication and Authorization

All requests to ickStream Cloud need an HTTP Authorization header with
the access token, for example:

`Authorization: Bearer B5BE805F-87A7-4C44-B2AB-D0B91B7EAA99`

Where "B5BE805F-87A7-4C44-B2AB-D0B91B7EAA99" is the value it gets during
the [Device registration
process](Device_registration_process "wikilink").

There are currently no authentication in communication on local network
through the P2P module.

## Initial setup first time a controller starts

A typical scenario for a new controller which never has been used before
is:

1.  It registers itself on the network through the P2P module as
    described above.
2.  It realize it doesn't have a valid access token and follows the
    [User authentication and
    registration](User_authentication_and_registration "wikilink")
    process to authenticate or register the user and then the [Device
    registration process](Device_registration_process "wikilink") to
    register itself in the cloud server.
      - The accessToken returned from the device registration process is
        something the controller should store persistently because it
        needs it later to be able to issue requests to the ickStream
        Cloud. The address to ickStream Cloud API's are available in the
        [Cloud Core Protocol](Cloud_Core_Protocol "wikilink")
        documentation.
3.  The controller calls the
    [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink") to retrieve available
    services in the user account.
4.  Either call
    [findDevices](Cloud_Core_Protocol/findDevices "wikilink") once
    or call [getDevice](Cloud_Core_Protocol/getDevice "wikilink")
    for each player device discovered on the local network. The process
    when a player is discovered is typically calling
    [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink") to
    detect if the player if already registered by looking at the
    **cloudCoreStatus** attribute and if registered compare it with the
    result from \[Cloud Core Protocol/findDevices|findDevices\]\]
    once or call
    [getDevice](Cloud_Core_Protocol/getDevice "wikilink") to detect
    if it's registered to the current user.
5.  The startup sequence is finished and the controller can start render
    a menu of available services as described in next section.

## Startup of an already registered controller

A typical scenario for a previously registered controller is:

1.  It registers itself on the network through the P2P module as
    described above.
2.  It realize it has an accessToken already from a previous
    registration and calls the
    [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink") to retrieve available
    services in the user account.
3.  Either call
    [findDevices](Cloud_Core_Protocol/findDevices "wikilink") once
    or call [getDevice](Cloud_Core_Protocol/getDevice "wikilink")
    for each player device discovered on the local network. The process
    when a player is discovered is typically calling
    [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink") to
    detect if the player if already registered by looking at the
    **cloudCoreStatus** attribute and if registered compare it with the
    result from \[Cloud Core Protocol/findDevices|findDevices\]\]
    once or call
    [getDevice](Cloud_Core_Protocol/getDevice "wikilink") to detect
    if it's registered to the current user.
4.  The startup sequence is finished and the controller can start render
    a menu of available services as described in next section.

## Render browse menu in a controller

The controller should render a browse menu based on content services on
local network discovered via the P2P module and online content services
dissevered by calling
[findServices](Cloud_Core_Protocol/findServices "wikilink") method
in the [Cloud Core Protocol](Cloud_Core_Protocol "wikilink").

Independent if a service is discovered on local network or via ickStream
Cloud the process is the same to get more information about the service:

  - Call
    [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    method to get image urls and other information needed to represent
    the service itself.
  - Call
    [getProtocolDescription2](Content_Access_Protocol/getProtocolDescription2 "wikilink")
    method to get information of available browse and search requests in
    the content service
  - Call
    [getPreferredMenus](Content_Access_Protocol/getPreferredMenus "wikilink")
    method to get information of preferred browse/search menus to
    provide for the content service

Browsing for content is always done using the
[findItems](Content_Access_Protocol/findItems "wikilink") method.

Generally it's recommended that a controller enables browsing in the
following ways:

  - An easy way to browse the music library of the user (independent
    which service the music belongs to)
      - See more information about My Music section below
  - An easy way to browse music for friends (independent which service
    the music belongs to)
      - See more information about Friends Music section below
  - An easy way to browse music in an individual service using the
    browse structure preferred by the specific service.
      - The structure should be based on the
        [getPreferredMenus](Content_Access_Protocol/getPreferredMenus "wikilink")
        method result.

### Providing a My Music section

It's recommeded that a controller provides a way to browse favorite
music independent which service it comes from. The **My Music** section
should not separate content into different services, instead content
from all services should be merged into a combined menu. Generally it's
safe to merge artist items with the same textual name from different
service into a single item in the browse menu. Other type of items
should be listed separately if the same item exists in multiple
services.

It's recommended that the structure to provide in a My Music section is
created as follows: (more information about the different requests can
be found in the description of
[getProtocolDescription2](Content_Access_Protocol/getProtocolDescription2 "wikilink")
method)

  - **My Artists**
      - Lists favorite artists based on the request: myMusic:artists
          - **My Artists/<artist>**
              - Lists albums for the artist based on the first of the
                following requests that exists: myMusic:albumsByArtist,
                allMusic:albumsByArtist
              - **My Artists/<artist>/Tracks**
                  - Lists tracks for the artist based on the first of
                    the following requests that exists:
                    myMusic:tracksByArtist, allMusic:tracksByArtist
              - **My Artists/<artist>/<album>**
                  - Lists tracks for the album based on the first of the
                    following requests that exists:
                    myMusic:tracksOnAlbum, allMusic:tracksOnAlbum

<!-- end list -->

  - **My Albums**
      -   - Lists favorite albums based on the request: myMusic:albums

      - **My Albums/<album>**

          - Lists tracks for the album based on the first of the
            following requests that exists: myMusic:tracksOnAlbum,
            allMusic:tracksOnAlbum

<!-- end list -->

  - **My Playlists**
      -   - Lists favorite playlists based on the request:
            myMusic:playlists

      - **My Playlists/<playlist>**

          - Lists tracks in the playlist based on the first of the
            following requests that exists: myMusic:tracksInPlaylist,
            allMusic:tracksInPlaylist

<!-- end list -->

  - **My Tracks**
      - Lists favorite tracks based on the request: myMusic:tracks

### Providing a Friends Music section

It's recommended that a controller provide a way to browse music by
friends independent which service it comes from. The Friends Music
section should not separate content into different services, instead
content from all services should be merged into a combined menu.
Generally it's safe to merge artist items with the same textual name
from different service into a single item in the browse menu. Other type
of items should be listed separately if the same item exists in multiple
services.

It's recommended that the structure to provide in a Friends Music
section is created as follows: (more information about the different
requests can be found in the description of
[getProtocolDescription2](Content_Access_Protocol/getProtocolDescription2 "wikilink")
method)

  - **My Friends**
      - Lists friends based on the request: friendsMusic:friends
      - **My Friends/<friend>/Artists**
          - List artists for the friend based on the request:
            friendsMusic:artistsForFriend
          - **My Friends/<friend>/Artists/<artist>**
              - Lists albums for the artist based on the first of the
                following requests that exists:
                friendsMusic:albumsForFriendByArtist,
                allMusic:albumsByArtist
              - **My Friends/<friend>/Artists/<artist>/Tracks**
                  - Lists tracks for the artist based on the first of
                    the following requests that exists:
                    friendsMusic:tracksForFriendByArtist,
                    allMusic:tracksByArtist
              - **My Friends/<friend>/Artists/<artist>/<album>**
                  - Lists tracks for the album based on the first of the
                    following requests that exists:
                    friendsMusic:tracksForFriendOnAlbum,
                    allMusic:tracksOnAlbum
      - **My Friends/<friend>/Albums**
          - List albums for the friend based on the request:
            friendsMusic:albumsForFriend
          - **My Friends/<friend>/Albums/<album>**
              - Lists tracks for the album based on the first of the
                following requests that exists:
                friendsMusic:tracksForFriendOnAlbum,
                allMusic:tracksOnAlbum
      - **My Friends/<friend>/Playlists**
          - List playlists for the friend based on the request:
            friendsMusic:playlistsForFriend
          - **My Friends/<friend>/Playlists/<playlist>**
              - Lists tracks in the playlist based on the first of the
                following requests that exists:
                friendsMusic:tracksForFriendInPlaylist,
                allMusic:tracksInPlaylist
      - **My Friends/<friend>/Tracks**
          - List tracks for the friend based on the request:
            friendsMusic:tracksForFriend

<!-- end list -->

  - **Albums from friends**
      - Lists albums from my friends based on the request:
        friendsMusic:albums
      - **Albums from friends/<album>**
          - Lists tracks for the album based on the request:
            allMusic:tracksOnAlbum

<!-- end list -->

  - **Tracks from friends**
      - Lists tracks from my friends based on the request:
        friendsMusic:tracks

## Render search menu in a controller

Searching can be offered in multiple ways in a controller and it's all
up to the controller developer to decide which search options that are
suitable, but some examples are:

  - Making it possible to search in an individual service based on the
    search requests referenced from the
    [getPreferredMenus](Content_Access_Protocol/getPreferredMenus "wikilink")
    method
  - Making it possible to for objects in other services based on the
    name of a selected artist, album, track
  - Making it possible to search globally in all services configured in
    the user account

It's important to remember that the reason to search can both be to play
something, to find related music or to find new music to add to your
list of favorite music. Generally when you perform a search you should
always limit the result to a suitable amount of items, it's not
recommended to retrieve as many items as possible in a search result
because this will just make it harder for a user to find what they are
looking for, usually requesting 10 items is enough and instead make it
possible for the user to list more matches if desired. Also, make sure
to show the result in the order it's returned from the content service
because the search requests generally return items ordered by relevance
with the most relevant matches first.

If a controller wants to provide a global search functionality, this can
be done by making a global search field that issues requests to all
services available in the user account. The requests to use can be
decided as follows:

  - **Artists**
      - Use the **allMusic:searchForArtists** request if it exists
  - **Albums**
      - Use the **allMusic:searchForAlbums** request if it exists
  - **Playlists**
      - Use the **allMusic:searchForPlaylists** request if it exists
  - **Tracks**
      - Use the **allMusic:searchForTracks** request if it exists

<!-- end list -->

  - **Radio stations**
      - Use the **allRadio:searchForStations** request if it exists

<!-- end list -->

  - **Users**
      - Use the **allMusic:searchForPersons** request if it exists

## Manage favorites and friends

It's recommended that controller that provides a **My Music** or
**Friends Music** section also provides a way to manage the content in
these sections.

Management is done using the
[addItem](Content_Access_Protocol/addItem "wikilink") and
[Content_Access_Protocol/removeItem](Content_Access_Protocol/removeItem "wikilink")
methods but you also have to verify that management is supported by the
content service by using the
[getManagementProtocolDescription](Content_Access_Protocol/getManagementProtocolDescription "wikilink")
method.

Typical management functions are:

  - Making it possible to search for an artist, album, track, playlist
    and add it to the **My Music** section
  - Remove an existing artist, album, track, playlist from the **My
    Music** section
  - Making it possible to search for a user and add it to **Friends
    Music** section
  - Remove an existing friend from **Friends Music** section

## Manage playlists

Playlists in ickStream are stored using the [Playlist Management
Protocol](Playlist_Management_Protocol "wikilink"). There can be
multiple implementations of this protocol in a user account and if more
than one is available it's important to ask the user which service a
playlist should be stored in. The available implementations can be found
using the
[findServices](Cloud_Core_Protocol/findServices "wikilink") method
by specifying type=**playlistmanagement**

<span style="color: red;">More information is probably needed here, ask
in the forum if you have some question about something</span>

## Sending commands to players and receive notifications

It's recommended that a controller implement all commands in the [Player
Protocol](Player_Protocol "wikilink"), but the most important
commands to implement is:

  - [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    - Issued by controller to provide a device registration token to the
    player during [device registration
    process](Device_registration_process "wikilink")
  - [getPlayerConfiguration](Player_Protocol/getPlayerConfiguration "wikilink")
    - Called to get name of unregistered players and information which
    cloud server a registered player is registered to
  - [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink") -
    Called to get information about currently playing track
  - [play](Player_Protocol/play "wikilink") - Called to start/stop
    playback
  - [getPlaybackQueue](Player_Protocol/getPlaybackQueue "wikilink")
    - Called to get information about current playback queue
  - [setTracks](Player_Protocol/setTracks "wikilink") - Called to
    set tracks in current playback queue
  - [addTracks](Player_Protocol/addTracks "wikilink") - Called to
    add tracks in current playback queue

A controller should also listen to at least the following notifications:

  - [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
    - Issued by a player for example when the currently playing track
    changes, playback stops/starts or when a player is
    registered/unregistered from the cloud server
  - [playbackQueueChanged](Player_Protocol/playbackQueueChanged "wikilink")
    - Issued by a player when the current playback queue changes,
    typically a controller should look at the **lastChanged** attribute
    and call
    [getPlaybackQueue](Player_Protocol/getPlaybackQueue "wikilink")
    in case it has changed if the controller wants its representation of
    current playback queue to be accurate.

However, as specified above, the controller should really implement all
methods in the [Player Protocol](Player_Protocol "wikilink"), the
above is just the bare minimum needed to get something to work in an
early phase when creating a new controller.

## Viewing requests between a controller and player

If you want to view the interaction between a controller and a player,
the easiest way is to install the [Test
Player](:Category:Installation#Test_Player "wikilink") on a Linux or OSX
computer and interact with it using the iOS controller app we have. This
is a dummy player without any playback support which just logs and
answers all requests sent from a controller.

## Resolving URLs for local content services

For local content services (like LMS plugin), image urls can point to a
<service://><serviceId>`/` url, the player need to resolve these to a
<http://><ip-address>`/` url.

This is typically done by listening to the callback registered in the
P2P module with the **ickDeviceRegisterDeviceCallback** function call
and when the callback is called for a new discovered device of type
**ICKDEVICE_SERVER_GENERIC** (4) issue a call to its
[getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
method according to [Service Protocol](Service_Protocol "wikilink")
to get it's **serviceUrl** attribute and later use that information to
resolve <service://><serviceId>`/` urls.

The reason for this resolving mechanism is that ickStream stores
playlists, favorites, ratings and similar things in the cloud server, so
it's important that references to local tracks is valid even if the
IP-address of the local content service has changed since last time it
was used.

[Category:API](Introduction "wikilink")
