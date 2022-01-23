**Definition:**

``` c
#include <ickP2p.h>

typedef enum ickErrcode_t;
```

**Defined values and associated strings:**

| Value                  | String                    |
| ---------------------- | ------------------------- |
| ICKERR_SUCCESS        | "No error"                |
| ICKERR_GENERIC        | "Generic error"           |
| ICKERR_NOTIMPLEMENTED | "Not implemented"         |
| ICKERR_INVALID        | "Invalid parameter"       |
| ICKERR_UNINITIALIZED  | "Not initialized"         |
| ICKERR_INITIALIZED    | "Already initialized"     |
| ICKERR_NOMEMBER       | "Not a member"            |
| ICKERR_WRONGSTATE     | "Library in wrong state"  |
| ICKERR_NOMEM          | "Out of memory"           |
| ICKERR_NOINTERFACE    | "Bad interface"           |
| ICKERR_NOTHREAD       | "Could not create thread" |
| ICKERR_NOSOCKET       | "Could not create socket" |
| ICKERR_NODEVICE       | "Unknown device"          |
| ICKERR_BADURI         | "Bad URI"                 |
| ICKERR_NOTCONNECTED   | "Not connected"           |
| ICKERR_ISCONNECTED    | "Already connected"       |
| ICKERR_LWSERR         | "Libwebsockets error"     |

**See also:**

  - [ickStrError](../ickP2p/ickStrError "wikilink") - Convert library
    error code to human readable string

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
