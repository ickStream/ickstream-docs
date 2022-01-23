# Overview

This page describes the device registration process step by step from
the perspective of each component

# Controller registering itself and optionally bundled player

1.  The controller retrieves a user access token as described on: [User
    authentication and
    registration](User_authentication_and_registration "wikilink")
2.  The controller calls
    [createDeviceRegistrationToken](Cloud_Core_Protocol/createDeviceRegistrationToken "wikilink")
    in Cloud Core service to retrieve a **deviceRegistrationToken**
3.  The controller register itself with the
    [addDevice](Cloud_Core_Protocol/addDevice "wikilink") method in
    Cloud Core service
      - The **deviceRegistrationToken** must be used for authentication
        in this call instead of the user access token
      - The controller will get back an **accessToken** which it must
        store persistently
4.  If the controller app also contains a player, the **accessToken**
    should be passed to the player module
      - Note, since the **accessToken** allows anyone to access the user
        account and all services available to the user, the
        **accessToken** should never be passed to other devices or other
        apps.
      - The player sends a
        [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
        notification with **cloudCoreStatus** attribute indicating that
        it's now registered

# Controller registering a remote player

1.  The controller discovers an unregistered device on the local network
      - By initializing [ickP2p](ickP2P_Protocol "wikilink")
      - The fact that a device is unregistered is indicated through
          - **cloudCoreStatus** attribute in
            [getPlayerStatus](Player_Protocol/getPlayerStatus "wikilink")
            method result or
            [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
            notification
          - A controller can optionally also check the **cloudCoreUrl**
            attribute from
            [getPlayerConfiguration](Player_Protocol/getPlayerConfiguration "wikilink")
            method result, but this is optional as different
            cloudCoreUrl values are only used during beta testing when
            there might be separate Cloud Core services for production
            and beta testing.
2.  The controller calls
    [createDeviceRegistrationToken](Cloud_Core_Protocol/createDeviceRegistrationToken "wikilink")
    in Cloud Core service to retrieve a **deviceRegistrationToken**
3.  The controller calls
    [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    method in Player Protocol and passes the **deviceRegistrationToken**
    as parameter
4.  The controller receives a
    [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
    notification from player with **cloudCoreStatus** attribute
    indicating that the player has been successfully registered

# Player being registered by a controller

1.  The player announce itself on the local network
      - By initializing [ickP2p](ickP2P_Protocol "wikilink")
2.  The player gets a
    [setPlayerConfiguration](Player_Protocol/setPlayerConfiguration "wikilink")
    method call with a **deviceRegistrationToken** which the controller
    has requested from the cloud.
3.  The player register itself with the uses the
    [addDevice](Cloud_Core_Protocol/addDevice "wikilink") method in
    Cloud Core service
      - The **deviceRegistrationToken** must be used for authentication
        in this call
      - The player will get back an **accessToken** which it must store
        persistently
4.  The player sends a
    [playerStatusChanged](Player_Protocol/playerStatusChanged "wikilink")
    notification with **cloudCoreStatus** attribute indicating that it's
    now registered
