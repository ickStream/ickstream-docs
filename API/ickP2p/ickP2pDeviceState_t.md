**Definition:**

``` c
#include <ickP2p.h>

typedef enum ickP2pDeviceState_t;
```

**Defined values and associated strings:**

| Value                | String         |
| -------------------- | -------------- |
| ICKP2P_INITIALIZED  | "initialized"  |
| ICKP2P_CONNECTED    | "connected"    |
| ICKP2P_DISCONNECTED | "disconnected" |
| ICKP2P_DISCOVERED   | "discovered"   |
| ICKP2P_BYEBYE       | "byebye"       |
| ICKP2P_EXPIRED      | "expired"      |
| ICKP2P_TERMINATE    | "terminating"  |
| ICKP2P_INVENTORY    | "inventory"    |
| ICKP2P_ERROR        | "error"        |

**See also:**

  - [ickLibDeviceState2Str](../ickP2p/ickLibDeviceState2Str "wikilink")
    - Convert device status code to human readable string

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
