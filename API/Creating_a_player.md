## Overview

The purpose of this page is to give a brief instruction and guide how to
make an ickStream player application.

I'll refer to git repositories below, to get access to these see the
separate page for [Source code
repositories](Source_code_repositories "wikilink")

In the main [API documentation page](Introduction "wikilink") there is
a section for "Protocols used in a player", this is what a typical
player would implement.

However, a bit more details follows below that describes how it works.

## Registration on local network

The player need to register itself in the P2P module as described in
[ickP2p](ickP2p "wikilink") API

  - The interface is in /ickp2p/ickP2p.h in the
    [ickstream-p2p](Source_code_repositories#ickStream_P2P_module "wikilink")
    repository
  - The player uses the P2P module and its
    [ickP2p](ickP2p "wikilink") API to register itself on the
    network and also for all communication on the local network, this is
    done by calling:
    1.  [ickP2pCreate](ickP2p/ickP2pCreate "wikilink") to setup a
        discovery context
    2.  [ickP2pRegisterMessageCallback](ickP2p/ickP2pRegisterMessageCallback "wikilink")
        to setup a callback which will be called when messages from the
        controller is received
    3.  [ickP2pRegisterDiscoveryCallback](ickP2p/ickP2pRegisterDiscoveryCallback "wikilink")
        to setup a callback which will be called when new content
        services is discovered on the local network, you will need this
        to be able to call the getServiceInformation method to get the
        IP address of the content service to resolve
        <service://>`<UUID/` streaming urls to a <http://><IP>`/` url
        which you can stream when the controller sends you tracks to
        play.
    4.  [ickP2pAddInterface](ickP2p/ickP2pAddInterface "wikilink")
        to add a network card/device which should be used for discovery
    5.  [ickP2pResume](ickP2p/ickP2pResume "wikilink") to start
        discovery and announce itself on the network
  - If you like some sample code, you can take a look at the
    /daemon/ickSocketDaemon.c file in the
    [ickstream-squeezebox-player](Source_code_repositories#ickStream_Squeezebox_player "wikilink")
    repository and its handleInitialization function which does the
    initialization.

For reference a controller and local content service (like LMS plugin)
will register itself on the network in similar fashion and all messages
from the controller will be received in the callback registered with the
ickP2pRegisterMessageCallback function.

## Authentication and Authorization

All requests to ickStream Cloud need an HTTP Authorization header with
the access token the player has got from the
[addDevice](Cloud_Core_Protocol/addDevice "wikilink") method in the
[Cloud Core Protocol](Cloud_Core_Protocol "wikilink"), for example:

`Authorization: Bearer B5BE805F-87A7-4C44-B2AB-D0B91B7EAA99`

Where "B5BE805F-87A7-4C44-B2AB-D0B91B7EAA99" is the value it gets in the
**accessToken** parameter returned from the
[addDevice](Cloud_Core_Protocol/addDevice "wikilink") call. For more
details regarding the device registration process, see the page: [Device
registration process](Device_registration_process "wikilink").

There are currently no authentication in communication on local network
through the P2P module.

## Initial setup first time a player starts

A typical scenario for a new player which never has been used before is:

1.  It registers itself on the network through the P2P module as
    described above.
2.  After the user of a controller has selected to register the player,
    the controller sends the player the
    [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    method in the [Player Protocol](Player_Protocol "wikilink")
      - In this call a deviceRegistrationToken is provided and this is
        used by the player when it register itself and retrieves an
        accessToken from the [Cloud Core
        Protocol](Cloud_Core_Protocol "wikilink"), see the [device
        registration
        process](Device_registration_process "wikilink") for more
        information about this.
      - The accessToken is something the player should store
        persistently because it needs it later to be able to issue
        requests to the ickStream Cloud. The address to ickStream Cloud
        API's are available in the [Cloud Core
        Protocol](Cloud_Core_Protocol "wikilink") documentation.
      - The player also typically calls the
        [setDeviceAddress](Cloud_Core_Protocol/setDeviceAddress "wikilink")
        method in the [Cloud Core
        Protocol](Cloud_Core_Protocol "wikilink") after it has got
        the accessToken. Calling
        [setDeviceAddress](Cloud_Core_Protocol/setDeviceAddress "wikilink")
        is not mandatory for the player to work but the player should do
        it to make it possible for controllers on an outside network to
        reach it in case the user has opened up the firewall to the
        local network.
3.  The startup sequence is finished and the player waits for the
    controller to start sending it some other commands in the [Player
    Protocol](Player_Protocol "wikilink")

## Startup of an already registered player

A typical scenario for a previously registered player is:

1.  It registers itself on the network through the P2P module as
    described above.
2.  It realize it has an accessToken already from a previous
    registration and calls the
    [setDeviceAddress](Cloud_Core_Protocol/setDeviceAddress "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink"). Calling
    [setDeviceAddress](Cloud_Core_Protocol/setDeviceAddress "wikilink")
    not mandatory for the player to work but the player should do it to
    make it possible for controllers on an outside network to reach it
    in case the user has opened up the firewall to the local network.
3.  The startup sequence is finished and the player waits for the
    controller to start sending it some other commands in the [Player
    Protocol](Player_Protocol "wikilink")

## Acting on commands from a controller

It's mandatory for a player to implement all commands in the [Player
Protocol](Player_Protocol "wikilink"). The experimental ickStream
Squeezebox Pplayer contains an implementation of this protocol in lua,
this can be found in the
[ickstream-squeezebox-player](Source_code_repositories#ickStream_Squeezebox_player "wikilink")
repository in the file /appletlocalplaylist/IckStreamApplet.lua. It's
the _handleJSONRPCRequest method that implements the [Player
Protocol](Player_Protocol "wikilink"). This code is licensed under
BSD to make everyone feel it's safe to copy, convert or get inspired by
the lua code when implementing their own player in C or some other
language of your choice.

The most important methods to implement to get something is:

  - [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    - Which the controller calls when it has initially registered the
    player and want to send it the access token for ickStream cloud.
  - [getPlayerConfiguration](Player_Protocol/getPlayerConfiguration "wikilink")
    - Which the controller initially calls to get name and similar
    information about the player
  - [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink") -
    Which the controller calls to get information about currently
    playing track
  - [play](Player_Protocol/play "wikilink") - Which the controller
    calls to start/stop playback
  - [getPlaylist](Player_Protocol/getPlaylist "wikilink") - Which
    the controller calls to get the current playlist
  - [setTracks](Player_Protocol/setTracks "wikilink") - Which the
    controller calls to replace tracks in the player current playlist
  - [addTracks](Player_Protocol/addTracks "wikilink") - Which the
    controller calls to send new tracks to the player current playlist

However, as specified above, the player must implement all methods in
the [Player Protocol](Player_Protocol "wikilink"), the above is just
the bare minimum needed to get something to work in an early phase when
creating a new player.

## Viewing requests between a controller and player

If you want to view the interaction between a controller and a player,
the easiest way is to install the [Test
Player](:Category:Installation#Test_Player "wikilink") on a Linux or OSX
computer and interact with it using the iOS controller app we have. This
is a dummy player without any playback support which just logs and
answers all requests sent from a controller.

## Resolving URLs for local content services

For local content services (like LMS plugin), streamingRef entries can
point to a <service://><serviceId>`/` url, the player need to resolve
these to a <http://><ip-address>`/` url.

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

## Resolving URLs for online content services

For premium online content service (like Deezer, Qobuz and WiMP) the
track items typically doesn't contain any streamingRefs entry. In this
case the player need to do the following when it's about to play track.

  - Call [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in [Cloud Core Protocol](Cloud_Core_Protocol "wikilink")
    to get the url for the service the tracks comes from. The service
    identity is the part before the first ":" character in the **id**
    attribute of the track. The result from findServices call should be
    cached by the player so it only makes a new call if a track belongs
    to a service for which it hasn't already retrieved the url.
  - Use the service url to call
    [getItemStreamingRefs](Content_Access_Protocol/getItemStreamingRef "wikilink")
    method in the [Content Access
    Protocol](Content_Access_Protocol "wikilink") of the content
    service providing the track.

## Scrobbling

A fully featured player should also support scrobbling when tracks are
played, to do this it will after registration have to:

1.  Call [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in [Cloud Core Protocol](Cloud_Core_Protocol "wikilink")
    to discover if there is a service of type **scrobble** registred in
    the current user account. A user have the option to disable and
    enable scrobbling globally for all his devices and this is done by
    adding and removing the **scrobble** services from his/hers account.
2.  If a **scrobble** service is registered, the player will take the
    url to it from the above request and then after each track has been
    played issue a call to the
    [playedTrack](Scrobble_Protocol/playedTrack "wikilink") method
    in the [Scrobble Protocol](Scrobble_Protocol "wikilink")

[Category:API](Introduction "wikilink")
