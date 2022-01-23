# Overview

There are multiple different API's available in the ickStream system
which you can access or integrate with.

Generally the implementation of an API is either provided by either by a
ickStream device on the local network or our central cloud service.

There are three main type of ickStream devices

  - **controller**
      - Provides ability to control player devices owned by the user
      - Offer ability to browse/search local and cloud based content
        services providing music content
      - Management of user data in the cloud, such as device
        information, playlists, favorites, loved/banned tracks and
        ratings
  - **player**
      - Stream and playback music in a current playlist which is managed
        by the player itself
      - Scrobble played tracks to the cloud
  - **content service**
      - Provides music content, which can be either locally stored music
        files or music from an online streaming service

All these device types can be used individually or combined in a single
device.

# Protocols used in a controller

  - Protocols typically accessed
      - [ickP2P Protocol](ickP2P_Protocol "wikilink") for discovery
        of ickStream devices on the local network and all device to
        device communication.
      - [Cloud Core Protocol](Cloud_Core_Protocol "wikilink") for
        management of ickStream devices and discovery of cloud services
      - [Service Protocol](Service_Protocol "wikilink") to resolve
        artwork urls represented as a <service://> url to an real
        <http://> url.
      - [Content Access
        Protocol](Content_Access_Protocol "wikilink") to browse and
        search in a content service, either on the local network or in
        the cloud, and manage favorite content which should be exposed
        through myMusic context when browsing the content service.
      - [Favorites Protocol](Favorites_Protocol "wikilink") to
        browse and manage favorites
      - [Library Management
        Protocol](Library_Management_Protocol "wikilink") to manage
        cloud library such as loved/banned tracks and ratings.
      - [Player Protocol](Player_Protocol "wikilink") to control an
        individual player device and manage or view its current playlist
      - [Playlist Management
        Protocol](Playlist_Management_Protocol "wikilink") to offer
        management of playlists to the user.

Except for the above, the controller is also responsible for:

  - Authentication of the user and initially register itself and
    discovered unregistered devices on the local network: [User
    authentication and
    registration](User_authentication_and_registration "wikilink")
  - Register discovered devices on the local network in the cloud:
    [Device registration
    process](Device_registration_process "wikilink")
  - Optionally allow a user to add a new authentication identity to the
    user account: [Add identity to user
    account](Add_identity_to_user_account "wikilink")
  - Optionally allow a user to add a new cloud based content service to
    the user account: [Add service to user
    account](Add_service_to_user_account "wikilink")

For more details about how to create a controller see the [creating a
controller](Creating_a_controller "wikilink") page

# Protocols used in a player

  - Protocols typically accessed
      - [ickP2P Protocol](ickP2P_Protocol "wikilink") for announcing
        itself on the network, discover local content services and all
        device to device communication.
      - [Service Protocol](Service_Protocol "wikilink") to resolve
        streaming urls represented as a <service://> url to an real
        <http://> url.
      - [Cloud Core Protocol](Cloud_Core_Protocol "wikilink") to
        update the device address at startup
      - [Scrobble Protocol](Scrobble_Protocol "wikilink") to
        scrobble played tracks to the library service in ickStream
        cloud.
  - Protocols typically implemented
      - [Player Protocol](Player_Protocol "wikilink") to be able to
        answer requests from controllers

For more details about how to create a player, see the [creating a
player](Creating_a_player "wikilink") page

# Protocols used in a content service

  - Protocols typically accessed
      - [ickP2P Protocol](ickP2P_Protocol "wikilink") for announcing
        itself on the network and all device to device communication.
  - Protocols typically implemented
      - [Service Protocol](Service_Protocol "wikilink") to make it
        possible for controllers and players to request information to
        be able to resolve streaming url's and artwork url's exposed as
        a <service://> url.
      - [Content Access
        Protocol](Content_Access_Protocol "wikilink") to let a
        controller know which queries and search ability that exists in
        the content service and to answer browse and search requests
        from a controller.
      - [Playlist Management
        Protocol](Playlist_Management_Protocol "wikilink") if it
        like to offer an ickStream controller the possibility to manage
        the playlists it maintains.

# Differences compared to Squeezebox

Since some third party developers developing add-ons for ickStream might
have done previous development on Squeezebox, here are some significant
differences regarding responsibilities between the two platforms from a
development perspective.

| -------------------- | ------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                      | **ickStream platform**                                                                                  | **Squeezebox platform**                                                                                                                                  |
| **Player discovery** | Each controller is responsible to keep track of available players on the network through the P2P module | LMS or mysqueezebox.com keeps track of the players which results in that in some apps you only see players which are connected to the current server. |
| **Current playlist** | Each player is responsible for its current playlist | The current playlist is completely managed by the server (LMS or mysqueezebox.com) |+
| **Player control** | Each controller controls the player directly through the P2P module without any server being involved | The players are always controlled through the server (LMS or mysqueezebox.com) |
| **Local content services** | Each controller discovers available local content services through the P2P module which means that ickStream can stream content from multiple content services (local and online) without the need to switch server and the player discovery logic is implemented in the P2P module so third party controllers doesn't have to implement it themselves. | The controller is connected to one specific server at the time and discovery logic to find available servers and available players have to be implemented in each controller. To browse a different server, the controller have to switch it's connection to the server it wants to browse content in. |
| **Saved playlists** | All saved ickStream playlists will be stored on the cloud server which means that saved playlists containing online content will be available without any local server | Saved playlists are only available from the local LMS the controller currently is connected to. |
| **Favorites** | In ickStream all favorites will be stored on the cloud server | Favorites are stored both in LMS and mysqueezebox.com which have to synchronize their content |
| **Play counts, ratings and statistics** | Information about play counts, ratings and loved/banned tracks will be stored on the cloud server making these available both for local and online content | Play counts and ratings are only stored in LMS for local content and loved/banned tracks are only available for the online streaming service which have native support for it. Ratings and possibility to view information about previous playback history is only available through third party add-ons |
| **Browse protocols** | The content services implements a content oriented protocol with a description protocol that describes the content which makes it possible for each controller application to decide exactly which browse menu structures to use or discover what is available from the content services and offer all possibilities | Many content services only provides a presentation oriented protocol which makes it hard for a controller to provide anything else than what the content service supports. |
| **Browse content format** | The contents from online streaming services is transformed to the ickStream content format to make it easy for the controller to understand if a certain item is an artist, album or track | Most online services just provides presentation data making it hard for a controller do do anything smart with it. |
| **My music collection** | The online content services provides a collection concept tied to the specific protocols on each supported premium streaming service which makes it possible to manage your favorite artists, albums, tracks in online services the same way independent how the premium streaming service implements it. The favorite artists, albums, tracks will also be represented if you use the apps provided by the premium streaming services since ickStream manage the data in the actual premium streaming service. | Many integrated online services doesn't have a concept on a favorite collection and barely none have the ability to manage it directly from the Squeezebox controller apps. |
| **Third party hardware** | The ickStream platform will be available to hardware partners who like to build players or controllers, for small volume and non commercial partners we will consider to offer it for free on case by case basis, contact us if this is something you are interested in. We want the ickStream platform to be available on hardware from many different manufacturers. | Hardware partners are restricted to playing local music and Logitech haven't shown any willingness to let them stream music from online streaming services |


We are already looking at the following feature but we haven't exposed
the API for these yet:

  - Synchronized playback
  - Dynamic/smart playlists
  - Integration with various social networks (Google, Twitter, Facebook,
    LastFM, ...)

We are continuously looking at possibilities for third parties to
enhance the ickStream platform, both through software and hardware, so
if you have ideas of something you would like to do, please let us know.

# JSON-RPC

With the exception for the [ickP2P
Protocol](ickP2P_Protocol "wikilink") which is a native C API all
other API's are using [JSON-RPC](http://www.jsonrpc.org/specification)
for the communication. Device to device protocol send requests and
responses using [JSON-RPC](http://www.jsonrpc.org/specification) using
the [ickP2P Protocol](ickP2P_Protocol "wikilink") while cloud
services sends [JSON-RPC](http://www.jsonrpc.org/specification) over
HTTP.

# Authentication and Authorization

The device to device communication through [ickP2P
Protocol](ickP2P_Protocol "wikilink") is currently open and doesn't
contain any security mechanisms, this might change in future revisions
of the protocols.

The security mechanism used in all accesses towards cloud services uses
the [OAuth 2.0](http://oauth.net/2/) standard and is based on the
principles described in the [Authentication and
Authorization](Authentication_and_Authorization "wikilink") section.
