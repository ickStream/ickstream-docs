Get version information for the ickstream p2p library

**Signature:**

``` c
#include <ickP2p.h>

const char *ickP2pGetVersion( int *major, int *minor );
```

**Arguments:**

  - **major** - pointer for storing the major version (might be NULL)
  - **minor** - pointer for storing the minor version (might be NULL)

**Return Value:**

  - String with the major and minor version information, including the
    git commit prefix.
  - Example: "1.0 755f73a"

[Category:ickP2P Protocol](Category:ickP2P_Protocol "wikilink")