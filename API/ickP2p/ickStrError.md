Convert an ickstream p2p error code to human readable string.

**Signature:**

``` c
#include <ickP2p.h>

const char *ickStrError( ickErrcode_t code );
```

**Arguments:**

  - **code** - an error code delivered by a library call

**Return Value:**

  - String with human readable explanation for the error code or
    "<Undefined error code>", if **code** is unknown.
  - Example: "Invalid parameter"

**See also:**

  - [ickErrcode_t](../ickP2p/ickErrcode_t "wikilink") - Library error
    codes

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")
