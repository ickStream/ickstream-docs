Convert an ickStream P2P context status code to human readable string.

**Signature:**

``` c
#include <ickP2p.h>

const char *ickLibState2Str( ickP2pLibState_t state );
```

**Arguments:**

  - **state** - an ickStream library state

**Return Value:**

  - String with human readable explanation for the state or "Invalid
    state", if **state** is unknown.
  - Example: "running"

**See also:**

  - [ickP2pLibState_t](../ickP2p/ickP2pLibState_t "wikilink") -
    Library states
  - [ickP2pGetState](../ickP2p/ickP2pGetState "wikilink") - Get context
    state

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
