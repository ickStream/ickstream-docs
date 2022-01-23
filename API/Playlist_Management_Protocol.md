# Principles

This page describes the protocol which is implemented either by a local
or cloud service which offers the ability to maintain a list of
playlists for a specific user.

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions

# Service methods

  - [findPlaylists](Playlist_Management_Protocol/findPlaylists "wikilink")
    - Used to get a list of playlsits available for the current user
  - [getPlaylist](Playlist_Management_Protocol/getPlaylist "wikilink")
    - Used to retrieve an existing playlist
  - [removePlaylist](Playlist_Management_Protocol/removePlaylist "wikilink")
    - Used to delete an existing playlist
  - [setTracks](Playlist_Management_Protocol/setTracks "wikilink") -
    Used to create a new playlist or modify an existing one
  - [setPlaylistName](Playlist_Management_Protocol/setPlaylistName "wikilink")
    - Used to modify name of an existing playlist
  - [setTrackMetadata](Playlist_Management_Protocol/setTrackMetadata "wikilink")
    - Used to refresh metadata of an existing playlist
  - [addTracks](Playlist_Management_Protocol/addTracks "wikilink") -
    Used to add individual tracks to a playlist
  - [moveTracks](Playlist_Management_Protocol/moveTracks "wikilink")
    - Used to move individual tracks in a playlist
  - [removeTracks](Playlist_Management_Protocol/removeTracks "wikilink")
    - Used to remove individual tracks from a playlist

[Category:API](Introduction "wikilink")
