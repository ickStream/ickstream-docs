## Overview

![ickStream_for_Streaming_Apps.png](IckStream_for_Streaming_Apps.png
"ickStream_for_Streaming_Apps.png")

After we have integrated your content in ickStream, it's also possible
for you to integrate support for ickStream in your own app and by doing
this allow end users to play your content on ickStream players and
control them from your own app. The advantage of this is that end users
can continue to use your app with the branding and user interface of
your choice and are still able to play content on ickStream player
devices remotely controlled from your app which also will continue to
play music in the background if your app is closed by the user.

In this scenario your app will browse/search content in your service
using your own API and it will pass the tracks it wants to be played to
ickStream compatible player devices using the [Player
Protocol](API/Player_Protocol "wikilink"). The ickStream player device
will handle the current playback queue and expose it over [Player
Protocol](API/Player_Protocol "wikilink") so your app have the ability
to show what's playing to the user. Your app doesn’t have to store the
current playback queue itself, it just have to communicate with the
ickStream player to retrieve it when it should be modified or shown to
the user.

Your app will pass tracks over [Player
Protocol](API/Player_Protocol "wikilink") as unique track identifiers
together with relevant metadata and cover image urls. When a track is
about to be played, the ickStream playback device will autonomously
retrieve a HTTP streaming url from the streaming service via
\[\[API/Content Access Protocol|Content Access Protocol\]. ickStream
cloud isn't involved in the actual streaming, the content will be
streamed directly between your service and the ickStream player.

The [ickP2p Protocol](API/ickP2p "wikilink") is used by your app to
discover available ickStream players on the local network. This is
handled through the open source ickP2p C-module which should be linked
into your app. There are also communication wrappers available for Java
and Objective-C, making it easy to integrated ickP2p both on Android and
iOS.

ickStream player devices must be [registered in ickStream cloud
server](API/Device_registration_process "wikilink"), this can either be
done by the end user through the official ickStream app or your app have
the option to [perform the
registration](API/Device_registration_process "wikilink") directly. The
exact solution for this is best selected based on the circumstances. If
your app just intends to play music on ickStream player devices that
already exists on the local network it’s not that important to integrate
the registration flow into your app. However, if your app is sold in a
bundled solution with ickStream player, it’s suitable to also integrate
the registration flow to make it easy for the user to setup everything
in a single app.

## Integration steps for iOS app

The following are the most significant integration steps involved in
integrating ickStream in your own iOS app

1.  Build and integrate ickComm library in your app:
    <https://github.com/ickStream/ickstream-ios-comm>
      - See readme files in ickComm/Comm and ickComm/Devices for more
        information
      - ickComm bundles the ickP2p module and automatically discovers
        all ickStream devices on the network and notifies your app when
        devices are connected/disconnected
2.  Send commands over ickComm using [Player
    Protocol](API/Player_Protocol "wikilink"), the most significant are:
      - Issuing setTracks, addTracks, moveTracks, removeTracks commands
        to manage current playback queue managed by the player device,
        see also track meta data section below for more information.
      - Issuing getPlayerStatus, getPlaybackQueue commands to get
        information about current playback queue and what's currently
        playing
      - Issuing setTrack command to manually switch currently playing
        track
      - Issuing getVolume, setVolume commands to change volume on the
        player device
      - Issuing play command to start/stop playback
3.  Listen for notifications over ickComm, the most significant are:
      - Listening to the playerStatusChanged, playbackQueueChanged
        notifications to ensure playback queue and information about
        currently playing track on the selected device is updated in
        your app

## Integration steps for Android app

The following are the most significant integration steps involved in
integrating ickStream in your own Android app

1.  Build and integrate the Java wrapper for ickP2p in your app:
    <https://github.com/ickStream/ickstream-java-p2p>
      - The Java wrapper for ickP2p bundles the ickP2p module and
        automatically discovers all ickStream devices on the network and
        notifies your app when devices are connected/disconnected
2.  Optionally integrate ickprotocol and ickcontroller modules to
    simplify communication over Java wrapper for ickP2p
      - Both ickprotocol and ickcontroller modules are found in the
        ickstream-java-common repository:
        <https://github.com/ickStream/ickstream-java-common>
3.  Send commands over ickprotocol or Java wrapper for ickP2p using
    [Player Protocol](API/Player_Protocol "wikilink"), the most
    significant are:
      - Issuing setTracks, addTracks, moveTracks, removeTracks commands
        to manage current playback queue managed by the player device,
        see also track meta data section below for more information.
      - Issuing getPlayerStatus, getPlaybackQueue commands to get
        information about current playback queue and what's currently
        playing
      - Issuing setTrack command to manually switch currently playing
        track
      - Issuing getVolume, setVolume commands to change volume on the
        player device
      - Issuing play command to start/stop playback
4.  Listen for notifications over ickprotocol or Java wrapper for
    ickP2p, the most significant are:
      - Listening to the playerStatusChanged, playbackQueueChanged
        notifications to ensure playback queue and information about
        currently playing track on the selected device is updated in
        your app

## Integration steps for device registration

If your app also wants to support registration of unregistered ickStream
player devices, the following steps are involved:

\#\* Apply for an API-key

\#\* [Authenticate/register the
user](API/User_authentication_and_registration "wikilink") in ickStream
cloud server

\#\* [Register and tie the ickStream player device to the current
user](API/Device_registration_process "wikilink")

## Track metadata

It's important that the track identities used in the [Player
Protocol](API/Player_Protocol "wikilink") matches how they are
represented in ickStream. The track metadata including identity needs to
follow the same syntax as retrieved via the [Content Access
Protocol](API/Content_Access_Protocol "wikilink"). However, your app of
course doesn't have to retrieve the tracks this way, you retrieve the
tracks by communicating with your service using a suitable protocol of
your choice and then just create the same structure as exposed by
[Content Access Protocol](API/Content_Access_Protocol "wikilink") when
you pass the tracks to the player using the [Player
Protocol](API/Player_Protocol "wikilink").

As an example, an addTracks command for a WiMP track can look like this:

``` javascript
{
  "jsonrpc" : "2.0",
  "id" : 23,
  "method" : "addTracks",
  "params" : {
    "items" : [ {
      "id" : "wimp:track:35297765",
      "text" : "3. Good Is Good",
      "type" : "track",
      "image" : "http://images.osl.wimpmusic.com/im/im?w=512&h=512&albumid=35297762",
      "itemAttributes" : {
        "id" : "wimp:track:35297765",
        "name" : "Good Is Good",
        "image" : "http://images.osl.wimpmusic.com/im/im?w=512&h=512&albumid=35297762",
        "trackNumber" : 3,
        "duration" : 259,
        "album" : {
          "id" : "wimp:album:35297762",
          "name" : "Wildflower (Deluxe Edition)",
          "image" : "http://images.osl.wimpmusic.com/im/im?w=512&h=512&albumid=35297762",
          "mainArtists" : [ ],
          "year" : 2014
        },
        "mainArtists" : [ {
          "id" : "wimp:artist:9529",
          "name" : "Sheryl Crow",
          "image" : "http://images.osl.wimpmusic.com/im/im?w=512&h=512&artistid=9529"
        } ],
        "composers" : [ ],
        "conductors" : [ ],
        "performers" : [ ],
        "categories" : [ ]
      }
    } ]
  }
}
```

A similar addTracks command for a Deezer track would look like this:

``` javascript
{
  "jsonrpc" : "2.0",
  "id" : 24,
  "method" : "addTracks",
  "params" : {
    "items" : [ {
      "id" : "deezer:track:85758578",
      "text" : "3. Good Is Good",
      "type" : "track",
      "image" : "http://api.deezer.com/2.0/album/8649642/image?size=big",
      "itemAttributes" : {
        "id" : "deezer:track:85758578",
        "name" : "Good Is Good",
        "image" : "http://api.deezer.com/2.0/album/8649642/image?size=big",
        "trackNumber" : 3,
        "duration" : 259,
        "disc" : "1",
        "album" : {
          "id" : "deezer:album:8649642",
          "name" : "Wildflower",
          "image" : "http://api.deezer.com/2.0/album/8649642/image?size=big",
          "mainArtists" : [ ],
          "year" : 2014
        },
        "mainArtists" : [ {
          "id" : "deezer:artist:1439",
          "name" : "Sheryl Crow"
        } ],
        "composers" : [ ],
        "conductors" : [ ],
        "performers" : [ ],
        "categories" : [ ]
      }
    } ]
  }
}
```

The most important part is the syntax of the "id" since this is later
used by the ickStream player device to retrieve a streaming url.
