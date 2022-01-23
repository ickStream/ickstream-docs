The general authentication principles in ickStream is that each user
account is tied to an OAuth authentication provider. The authentication
provider could be Google, Facebook or if a user want to use a
username/password it's also possible to use the OAuth authentication
provider provided on ickStream cloud.

The process is that when the user starts an ickStream controller
application the first time authentication will be performed, this is
done according to the the following section: [User authentication and
registration](User_authentication_and_registration "wikilink").
After the registration process is completed the ickStream controller
will get a device access token and in all future communication with the
ickStream cloud this token will be used instead of the credentials the
user used to register the device.

For a player it works a bit different. The process when registering a
player is that a controller will discover the player on the local
network and perform the registration of it through the [Cloud Core
Protocol](Cloud_Core_Protocol "wikilink") and after registration
call the setPlayerConfiguration method in the [Player
Protocol](Player_Protocol "wikilink") to pass the device access
token to the player. The controller shall never store this token itself,
it just passes it on to the player and after this it's the
responsibility of the player to keep track of it and use it in all
ickStream cloud accesses.

<font color=red>Note that we are currently investigating the player
registration process and it will probably be adapted a bit to make it
more secure, but the general principle that the player gets a device
access token at the end will be the same</font>

When accessing ickStream cloud the device access token should be be
passed in the HTTP **Authorization** header as a Bearer token, for
example:

`Bearer A4059543-1D54-484F-AE70-0398950BFB78`

If the authorization fails in ickStream cloud this will result in error
codes as described in the [Error Codes](Error_Codes "wikilink")
section.

[Category:API](Introduction "wikilink")
