# Principles

This page describes the protocol which is used to report played tracks
and keep track of playback history.

Also note that the browsing functionality for the statistics service
will be provided as a normal implementation of a [Content Access
Protocol](Content_Access_Protocol "wikilink") and due to this it's
not described on this page.

To find the access point for the Scrobble Protocol, see:

  - For cloud version: The
    [findServices](Cloud_Core_Protocol/findServices "wikilink")
    method in the [Cloud Core
    Protocol](Cloud_Core_Protocol "wikilink")
  - For local network versions: The
    [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    method in the [Service Protocol](Service_Protocol "wikilink")

The recommendation is to scrobble to all available Scrobble Protocol
implementations available both in the cloud and on the local network.

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

  - [playedTrack](Scrobble_Protocol/playedTrack "wikilink") - Mark a
    track as played

[Category:API](Introduction "wikilink")
