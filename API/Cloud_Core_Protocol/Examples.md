# getUser sample response

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : "1",
   "result" : {
     "id" : "12347865-B5E1-4914-9954-BD330B46CE2E",
     "name" : "Some User",
     "identities" : [ {
       "type" : "soundcloud",
       "identity" : "someuseratsoundcloud"
     }, {
       "type" : "email",
       "identity" : "someuser@example.org"
     } ]
   }
}
```

# getDevice, addDeviceWithHardwareId, setDeviceAddress, setDeviceName sample response

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : "1",
   "result" : {
     "id" : "1234BFD9-912F-4AB5-B5B9-93D4861FEC93",
     "accessToken" : "A7798635-008A-49B9-814E-F7DCA8463DA0",
     "text" : "Kitchen Player",
     "model" : "player",
     "address" : "172.16.0.31"
   }
}
```

# removeDevice sample response

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : "1",
   "result" : true
}
```

# findDevices sample response

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : "1",
   "result" : {
     "count" : 3,
     "countAll" : 3,
     "offset" : 0,
     "items" : [ {
       "id" : "12342D83-2137-431F-9477-528E41B50648",
       "text" : "Overall Controller",
       "model" : "controller",
       "address" : "172.16.0.35"
     }, {
       "id" : "1234BFD9-912F-4AB5-B5B9-93D4861FEC93",
       "text" : "Kitchen Player",
       "model" : "player",
       "address" : "172.16.0.31"
     }, {
       "id" : "1234FD5A-F326-4944-B053-74AF5C52D306",
       "text" : "Bedroom Player",
       "model" : "player",
       "address" : "172.16.0.32"
     } ]
   }
}
```

# findServices sample response

``` javascript
{
   "jsonrpc" : "2.0",
   "id" : "1",
   "result" : {
     "count" : 2,
     "countAll" : 2,
     "offset" : 0,
     "items" : [ {
       "id" : "deezer",
       "text" : "deezer",
       "type" : "content",
       "url" : "https://api.ickstream.com/ickstream-cloud-deezer/jsonrpc"
     }, {
       "id" : "soundcloud",
       "text" : "soundcloud",
       "type" : "content",
       "url" : "https://api.ickstream.com/ickstream-cloud-soundcloud/jsonrpc"
     } ]
   }
}
```

[Category:Cloud Core Protocol](Category:Cloud_Core_Protocol "wikilink")