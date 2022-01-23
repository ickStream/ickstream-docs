# Principles

Each ickStream content service, local or ickStream Cloud, needs to offer
a number of JSON-RPC commands for querying its data. Optionally they can
also offer management function making it possible to add/remove items
from a certain content in the content service.

# Generic methods

  - [getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
    - Get information about service
  - [getProtocolVersions](Service_Protocol/getProtocolVersions "wikilink")
    - Get information about supported protocol versions
  - [getAccountInformation](Content_Access_Protocol/getAccountInformation "wikilink")
    - Get information about the account the user is using in the service

# Discovery methods

The purpose of these methods is to describe which query options a
certain content service supports, this shall be used by a Controller to
know what findItems, addItem and removeItem calls it can use.

  - ~~[getProtocolDescription](Content_Access_Protocol/getProtocolDescription "wikilink")~~
    - Get information about available queries that can be used in
    [findItems](Content_Access_Protocol/findItems "wikilink") method
  - [getProtocolDescription2](Content_Access_Protocol/getProtocolDescription2 "wikilink")
    - Get information about available queries that can be used in
    [findItems](Content_Access_Protocol/findItems "wikilink") method
  - [getPreferredMenus](Content_Access_Protocol/getPreferredMenus "wikilink")
    - Get information about preferred menus which is appropriate to
    render for this service
  - [getManagementProtocolDescription](Content_Access_Protocol/getManagementProtocolDescription "wikilink")
    - Get information about available possibilities that are available
    for [addItem](Content_Access_Protocol/addItem "wikilink") and
    [removeItem](Content_Access_Protocol/removeItem "wikilink")
    methods.

# Dynamic playlist methods

  - [getNextDynamicPlaylistTracks](Content_Access_Protocol/getNextDynamicPlaylistTracks "wikilink")
    - Retrieve next tracks for a dynamic playlist

# Query methods

  - [findItems](Content_Access_Protocol/findItems "wikilink") -
    Search or browse for content items
  - [getItem](Content_Access_Protocol/getItem "wikilink") - Get a
    specific content item

# Streaming reference methods

  - [getItemStreamingRef](Content_Access_Protocol/getItemStreamingRef "wikilink")
    - Get streaming reference for a specific track/stream

# myMusic context management methods

  - [addItem](Content_Access_Protocol/addItem "wikilink") - Add a
    content item to the myMusic context
  - [removeItem](Content_Access_Protocol/removeItem "wikilink") -
    Remove a content item from the myMusic context

# Resolving URI's

In the results from the Content Access Protocol there is a number of
places where a URI is returned, the most common places are the \*image\*
attribute in the main item and the \*url\* attribute inside the
\*streamingRefs\* attribute.

The supported URL's are:

  - http/https urls
  - service url's with the syntax:
    <service://><serviceId>/<path>\[?<queryParameters>\]\[#<fragment>\]
      - <serviceId> is the identity returned in \[Cloud Core Protocol\]
        methods findServices and findAllServices or the \*id\* returned
        from the \*getServiceInformation\* method in \[Service
        Protocol\] which all services have to implement.
      - <path>, <queryParameters> and <fragment> is the same as in any
        standard HTTP/HTTPS url

The service: URI's are mostly exposed from local content services but
any ickStream device must ensure it can handle them both for local and
cloud based content services.

**The general principle is**: If a content service returns a URI like:

    service://myUniqueServiceID/album1/track1.flac

The ickStream device have to get the **serviceURL** attribute from the
[getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
method. Let's say
[getServiceInformation](Service_Protocol/getServiceInformation "wikilink")
returns something like:

``` javascript
{
    "jsonrpc": "2.0",
    "id": "123",
    "result": {
        "id": "myUniqueServiceID",
        "name": "My Unique Service",
        "serviceURL": "http://192.168.0.33/myuniqueapplication"
    }
}
```

To resolve the URL the ickStream device now have to replace the part

    service://myUniqueServiceID

with the **serviceURL** attribute value so it ends up with a URL like:

    http://192.168.0.33/myuniqueapplication/album1/track1.flac

# Usage from a controller

See the [creating a controller](Creating_a_controller "wikilink")
page for more details about how the method in this service it typically
used by a controller.

The navigation of menu objects is a bit special:

  - Controller can navigate server side menus if type=menu
      - findItems type:menu
          - Gets the top level menu items where each item have
              - type=menu
              - id=<some id>
      - findItems menuId:<some id>
          - Will get the immediate child items below the menu item with
            id <some id>
      - findItems menuId:<some id> type:menu
          - Will get the immediate child items with type:menu below the
            menu item with id <some id>

There is no way to get a complete sub menu tree in a single request, the
controller have to get one menu level at the time.

# Examples of usage with artist/album/tracks based service

See [the separate examples
page](Content_Access_Protocol_Examples "wikilink")

[Category:API](Introduction "wikilink")
