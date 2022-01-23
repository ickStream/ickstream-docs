**Definition:**

``` c
#include <ickP2p.h>

typedef enum ickP2pLibState_t;
```

**Defined values and associated strings:**

| Value               | String        |
| ------------------- | ------------- |
| ICKLIB_CREATED     | "created"     |
| ICKLIB_RUNNING     | "running"     |
| ICKLIB_SUSPENDED   | "suspended"   |
| ICKLIB_TERMINATING | "terminating" |

**See also:**

  - [ickLibState2Str](../ickP2p/ickLibState2Str "wikilink") - Convert
    context status code to human readable string
  - [ickP2pGetState](../ickP2p/ickP2pGetState "wikilink") - Get context
    state

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
