# Principles

This page describes the protocol which needs to be implemented by all
devices offering a Playback service. It's not the protocol for the
actual playback, it's the protocol which is used to specify what should
be played and reporting status changes related to what's currently
playing.

The protocol consist of four different areas:

  - **Player configuration/information** - Methods to change player
    configuration and view other information about the player
  - **Playback control** - Methods to change start/stop playback and
    navigate in the current playback queue
  - **Playback queue management** - Methods to manage current playback
    queue
  - **Notifications** - Notifications which are generated when different
    type of states changes

The player should maintain a state sequence number that's incremented
every time the state is changed, for example:

  - When a command modifies current playback queue or move current
    played position in the current playback queue
  - When a command changes between play/pause or adjusts volume
  - When the player by itself either changes between play/pause or move
    position in current playback queue to next track

On longer terms this sequence number might be exposed in the protocol,
but it won't be exposed before we know that a typical controller
actually have a need for it.

The player needs to keep track of two different playback queues

  - The current playback queue - Contains the tracks in the order they
    are about to be played
  - The originally ordered playback queue - Contains the tracks in the
    order that should be used when shuffle is not active

Both these playback queues contains exactly the same tracks with related
data, it's only the order of the tracks that differs. All modifications
are done to the current playback queue and it's the player
responsibility to update the originally ordered playback queue. Both can
be retrieved with the
[getPlaybackQueue](Player_Protocol/getPlaybackQueue "wikilink")
method. The main purpose of the originally ordered playback queue is to
make it possible to restore the originally order when the user switches
from a shuffled to a non shuffled mode with the
[setPlaybackQueueMode](Player_Protocol/setPlaybackQueueMode "wikilink")
method. It's documented in more detail in each separate method how they
affect the originally ordered playback queue.

The player keeps a reference to the playlist or dynamic playlist which
have been used to fill the current playback queue, the purpose of this
reference is only a service to controllers to make it possible for a
controller to show which playlist that's currently playing and do
modifications in the current playback queue and later save these in the
playlist.

For an introduction about what's needed to implement a player, see the
separate [Creating a player](Creating_a_player "wikilink") page.

# Methods

## Player configuration/information

The player configuration section provides the functions:

  - [getProtocolVersions](Player_Protocol/getProtocolVersions "wikilink")
    - Sets player configuration like name and access token
  - [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    - Sets player configuration like name and access token
  - [getPlayerConfiguration](Player_Protocol/getPlayerConfiguration "wikilink")
    - Retrieves player configuration like the name of the player

## Playback control

The playback control section provides the functions:

  - [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink") -
    Get current player status, like what's currently playing
  - [play](Player_Protocol/play "wikilink") - Start or stop playback
  - [getSeekPosition](Player_Protocol/getSeekPosition "wikilink") -
    Get current position within the track
  - [setSeekPosition](Player_Protocol/setSeekPosition "wikilink") -
    Set current position within the track
  - [getTrack](Player_Protocol/getTrack "wikilink") - Get
    information about a specific track
  - [setTrack](Player_Protocol/setTrack "wikilink") - Set currently
    playing track
  - [setTrackMetadata](Player_Protocol/setTrackMetadata "wikilink")
    - Update meta data for a track in playback queue
  - [getVolume](Player_Protocol/getVolume "wikilink") - Get current
    volume
  - [setVolume](Player_Protocol/setVolume "wikilink") - Set current
    volume
  - [setPlaybackQueueMode](Player_Protocol/setPlaybackQueueMode "wikilink")
    - Set current playback queue mode of player

## Playback queue management

The playback queue management section provides the following functions:

  - [setDynamicPlaybackQueueParameters](Player_Protocol/setDynamicPlaybackQueueParameters "wikilink")
    - Set the parameters to be used when filling playback queue when the
    dynamic playback queue mode is active
  - [getPlaybackQueue](Player_Protocol/getPlaybackQueue "wikilink")
    - Get list of tracks currently in the playback queue
  - [setPlaylistName](Player_Protocol/setPlaylistName "wikilink") -
    Set reference to playlist which have been used to fill the playback
    queue
  - [addTracks](Player_Protocol/addTracks "wikilink") - Add one or
    several tracks to the playback queue
  - [removeTracks](Player_Protocol/removeTracks "wikilink") - Remove
    one or several tracks from the playback queue
  - [moveTracks](Player_Protocol/moveTracks "wikilink") - Move one
    or several tracks in the playback queue
  - [setTracks](Player_Protocol/setTracks "wikilink") - Replace
    playback queue with a new set of tracks
  - [shuffleTracks](Player_Protocol/shuffleTracks "wikilink") -
    Shuffle tracks in current playback queue

# Notifications

The following notifications are generated from a player

  - [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
    - When player state has changed
  - [playbackQueueChanged](Player_Protocol/playbackQueueChanged "wikilink")
    - When current playback queue has changed

[Category:API](Introduction "wikilink")
