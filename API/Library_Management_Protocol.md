# Principles

This page describes the protocol which is used to mark tracks as
loved/banned or add a tag or rating to a track, basically functions to
manage the ickStream Cloud Library.

Also note that the browsing functionality for the library management
service will be provided as a normal implementation of a [Content Access
Protocol](Content_Access_Protocol "wikilink") and due to this it's
not described on this page.

To find the access point for the Library Management Protocol, see:

  - For cloud version: The
    [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink")
  - For local network versions: The
    [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    method in the [Service Protocol](Service_Protocol "wikilink")

The recommendation is to use the cloud version if it exists in the user
account and else use a local server if it exists and if none of them
exists don't offer any library management functionality to the user

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

  - [saveTrack](Library_Management_Protocol/saveTrack "wikilink") -
    Set rating, loved, ban information on a track or just add it to
    library
  - [removeTrack](Library_Management_Protocol/removeTrack "wikilink")
    - Remove track from library including rating, loved, ban information
  - [getTrack](Library_Management_Protocol/getTrack "wikilink") -
    Get information about a specific track in the library

[Category:API](Introduction "wikilink")
