## Debugging ickP2P

There are a number of ways to debug the state of an ickP2P instance, the
ickP2P module have support for a number of debug API's as described
below. However, the easiest way to use them is to use the P2P debug
client which is available as a ready to use binary, you will find it on
the [installation
page](:Category:Installation#P2P_debug_client "wikilink")

### Using Client Code on the Instance Itself

There are two functions for ickP2P

**ickDeviceGetLocalDebugInfoForDevice**

`char * ickDeviceGetLocalDebugInfoForDevice(char * UUID);`

This will return a JSON encoded debug state for the device identified by
<UUID>.

The result is a dictionary describing the state of the ickP2P connection
to the device <UUID> as seen from the current client. With other words,
the state in which the connection to device <UUID> on the client on
which the function is called, currently is.

**ickDeviceGetLocalDebugInfo**

`char * ickDeviceGetLocalDebugInfo();`

This returns an array of debug dictionaries for the connections to all
connected devices.

### Remote State Retrieved Through ickP2P

There are two functions for this in ickP2P

**ickDeviceGetRemoteDebugInfoForDeviceQueryDevice**

`char * ickDeviceGetRemoteDebugInfoForDeviceQueryDevice(char * UUID, char * debugUUID);`

This will query client <UUID> for the status of it's connection to
device <debugUUID>

<UUID> will have to be discovered.

**ickDeviceGetRemoteDebugInfoForDevice**

`char * ickDeviceGetRemoteDebugInfoForDevice(char * UUID);`

This queries client <UUID> for the state of all of it's connections.

<UUID> will have to be discovered.

### Remote State Retrieved Over http

The remote state can also be retrieved over http if the IP address and
the port used by the server on the remote device are known (e.g. through
the discovery).

<http://><ip>`:`<port>`/debug`

This URL will return the state of all connections on the client/instance

<http://><ip>`:`<port>`/debug/`<UUID>

This URL will return the state of the client's connection to device
<UUID>

### Information format

``` java
{
        "type": <connection type>,
        "UUID": <UUID-String>,
        "URL": <IP-String>,
        "port": <port-number>,
        "name": <device name string>,
        "wsi": <websocket pointer>,
        "messagesWaiting": <number of outbound messages waiting to be sent>,
        "_upnp_device": {
            "validity": <time until when the discovered device is valid>,
            "header1": <device type header>,
            "header2": <device id header>,
            "header3": <xml resource URL>
        },
        "xmlData": <xml data retrieved from remote device>,
        "lastIn": <last time an inbound message was retrieved or random value>,
        "lastOut": <last time an outbound message was successfully sent>,
        "t_connected": <time when the connection was established>,
        "t_disconnected": <time when the connection was closed (if so)>,
        "bufLen": <size of unprocessed, inbound buffer in case ickP2P is waiting for a part of a bigger message>,
        "isServer": <indicates whether the device holds the server or client side of the libwebsocket connection>
    }
```

### Examples

See [the example page for a sample of the
output](../ickP2P_Protocol/Debugging/Examples "wikilink")

## Debug ickP2P Test Mirroring

ickP2P allows an End-To-End test of the ickP2P functionality by sending
test messages to a connected instance that will be processed at the end
of the ickP2P process (like a callback into the actual application) so
that the ickP2P instance can be tested for availability.

To make this work, the ickP2P instance to be tested needs to be in
"debug" mode. This mode is enabled by calling

`void enableDebugCallback();`

Once the debug mode is enable, an ickP2P client will respond to the
following messages:

### debugMirror

`{"method":"debugMirror",<optional, additional payload>}`

The first part

`{"method":"debugMirror"`

Needs to be sent literally and without additional padding etc. After
this, any additional set of valid JSON parameters can be added.

The message shall be valid JSON

Note: the message will also be sent to the receiving application, so any
application enabling debug mode in ickP2P needs to expect to receive
debug messages.

ickP2P will reply to the sender with a response of

`{"method":"debugReply",<optional, additional payload>}`

The optional payload of the original message will be preserved

### debugNotification

`{"method":"debugNotification",<optional, additional payload>}`

The first part

`{"method":"debugNotification"`

Needs to be sent literally and without additional padding etc. After
this, any additional set of valid JSON parameters can be added.

The message shall be valid JSON

Note: the message will also be sent to the receiving application, so any
application enabling debug mode in ickP2P needs to expect to receive
debug messages.

ickP2P will reply to all connected devices with a response of

`{"method":"debugReply",<optional, additional payload>}`

The optional payload of the original message will be preserve

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
