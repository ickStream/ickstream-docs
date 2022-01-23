Convert am ickStream device status code to human readable string.

**Signature:**

``` c
#include <ickP2p.h>

const char *ickLibDeviceState2Str( ickP2pDeviceState_t state );
```

**Arguments:**

  - **state** - an ickStream device state as provided by the discovery
    callback

**Return Value:**

  - String with human readable explanation for the state or "Invalid
    device state", if **state** is unknown.
  - Example: "disconnected"

**See also:**

  - [ickP2pDeviceState_t](../ickP2p/ickP2pDeviceState_t "wikilink") -
    Device state

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
