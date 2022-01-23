# Principles

This page describes the protocol which is used to manage favorites and
too get a list of available favorites. The meaning of a favorite is that
the user want a shortcut to a certain area of the menu structure,
typically it's a shortcut to a radio station, playlist or something
similar.

To find the access point for the Favorites Protocol, see:

  - For cloud version: The
    [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink")
  - For local network versions: The
    [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    method in the [Service Protocol](Service_Protocol "wikilink")

The recommendation is to use the cloud version if it exists in the user
account and else use a local server if it exists and if none of them
exists don't offer any favorites functionality to the user

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

  - [saveFavorite](Favorites_Protocol/saveFavorite "wikilink") - Add
    or update a favorite for current user
  - [removeFavorite](Favorites_Protocol/removeFavorite "wikilink") -
    Remove a favorite for current user
  - [findFavorites](Favorites_Protocol/findFavorites "wikilink") -
    Get favorites for current user

[Category:API](Introduction "wikilink")
