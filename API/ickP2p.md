# Overview

The ickP2P protocol is used for automatic announcement and discovery on
the local network all device to device communication is handled through
the ickP2P protocol. It's a C component with a C API, provided as static
library **libickp2p.a** and the include file **ickP2p.h**. The general
principle when used on third party platform is that you build it for the
architecture you want to use and bundle it with your application.

# Basic usage

Each ickstream device is defined by its UUID and logically represented
by an ickstream context of type **ickP2pContext_t**. I.e. an
application can host several logical devices by managing multiple
contexts.

The life-cycle of a context is

  - **Creation** - Define context with basic features
  - **Setup** - Add additional features and define callbacks
  - **Activation** - Activate the context, i.e. announce the device and
    react on messages
  - **Deletion** - Remove the device and clean up

# Data Types

The ickP2P library makes use of the following data types:

  - ickP2pContext_t - ickStream context descriptor
  - [ickErrcode_t](ickP2p/ickErrcode_t "wikilink") - Library error
    code
  - [ickP2pLibState_t](ickP2p/ickP2pLibState_t "wikilink") -
    Library state
  - [ickP2pDeviceState_t](ickP2p/ickP2pDeviceState_t "wikilink") -
    Device state
  - [ickP2pServicetype_t](ickP2p/ickP2pServicetype_t "wikilink") -
    ickStream service type
  - [ickP2pMessageFlag_t](ickP2p/ickP2pMessageFlag_t "wikilink") -
    Flags for message callback
  - [ickUpnpNames_t](ickP2p/ickUpnpNames_t "wikilink") -
    Definitions of UPnP announcement strings

# List of API functions

## Global functions (not related to a context)

The following API functions are global, i.e. do affect all contexts.

### Versions

  - [ickP2pGetVersion](ickP2p/ickP2pGetVersion "wikilink") - Get
    version of library
  - [ickP2pGitVersion](ickP2p/ickP2pGitVersion "wikilink") - Get
    full git version of library

### String conversion

  - [ickStrError](ickP2p/ickStrError "wikilink") - Convert library
    error code to human readable string
  - [ickLibState2Str](ickP2p/ickLibState2Str "wikilink") - Convert
    context status code to human readable string
  - [ickLibDeviceState2Str](ickP2p/ickLibDeviceState2Str "wikilink")
    - Convert device status code to human readable string

### Logging

  - [ickP2pSetLogging](ickP2p/ickP2pSetLogging "wikilink") - Setup
    log level and default logging facility
  - [ickP2pGetLogContent](ickP2p/ickP2pGetLogContent "wikilink") -
    get content of in-memory logging of default logging facility
  - [ickP2pSetLogFacility](ickP2p/ickP2pSetLogFacility "wikilink") -
    Set a user logging facility

## Context life-cycle

  - [ickP2pCreate](ickP2p/ickP2pCreate "wikilink") - Create an
    ickstream device context
  - [ickP2pResume](ickP2p/ickP2pResume "wikilink") - Activate an
    ickstream device context
  - [ickP2pSuspend](ickP2p/ickP2pSuspend "wikilink") - Suspend an
    ickstream device context
  - [ickP2pEnd](ickP2p/ickP2pEnd "wikilink") - Delete an ickstream
    device context

## Context configuration

### Interfaces and connections

  - [ickP2pAddInterface](ickP2p/ickP2pAddInterface "wikilink") - Add
    a network interface
  - [ickP2pUpnpLoopback](ickP2p/ickP2pUpnpLoopback "wikilink") -
    Enable context loopback
  - [ickP2pSetConnectMatrix](ickP2p/ickP2pSetConnectMatrix "wikilink")
    - Set a user defined connection matrix

### Callbacks for device discovery and message handling

  - [ickP2pRegisterDiscoveryCallback](ickP2p/ickP2pRegisterDiscoveryCallback "wikilink")
    - Register a device discovery callback
  - [ickP2pRemoveDiscoveryCallback](ickP2p/ickP2pRemoveDiscoveryCallback "wikilink")
    - Remove a device discovery callback
  - [ickP2pRegisterMessageCallback](ickP2p/ickP2pRegisterMessageCallback "wikilink")
    - Register a message receiver callback
  - [ickP2pRemoveMessageCallback](ickP2p/ickP2pRemoveMessageCallback "wikilink")
    - Remove a message receiver callback

## Inquiring context features

  - [ickP2pGetState](ickP2p/ickP2pGetState "wikilink") - Get context
    state
  - [ickP2pGetName](ickP2p/ickP2pGetName "wikilink") - Get friendly
    device name of context
  - [ickP2pGetDeviceUuid](ickP2p/ickP2pGetDeviceUuid "wikilink") -
    Get device UUID of context
  - [ickP2pGetServices](ickP2p/ickP2pGetServices "wikilink") - Get
    ickstream services as announced by context
  - [ickP2pGetUpnpLoopback](ickP2p/ickP2pGetUpnpLoopback "wikilink")
    - Get loopback status of context
  - [ickP2pGetUpnpFolder](ickP2p/ickP2pGetUpnpFolder "wikilink") -
    Get path to folder served by context
  - [ickP2pGetLifetime](ickP2p/ickP2pGetLifetime "wikilink") - Get
    SSDP life time as announced by context
  - [ickP2pGetLwsPort](ickP2p/ickP2pGetLwsPort "wikilink") - Get
    libwebsockets server port of context
  - [ickP2pGetUpnpPort](ickP2p/ickP2pGetUpnpPort "wikilink") - Get
    SSDP port of context
  - [ickP2pGetBootId](ickP2p/ickP2pGetBootId "wikilink") - Get SSDP
    boot id as announced by context
  - [ickP2pGetConfigId](ickP2p/ickP2pGetConfigId "wikilink") - Get
    SSDP config id as announced by context
  - [ickP2pGetOsName](ickP2p/ickP2pGetOsName "wikilink") - Get
    operating system string as announced by context

## Get features of discovered devices

  - [ickP2pGetDeviceName](ickP2p/ickP2pGetDeviceName "wikilink") -
    Get friendly name
  - [ickP2pGetDeviceServices](ickP2p/ickP2pGetDeviceServices "wikilink")
    - Get announced services
  - [ickP2pGetDeviceLocation](ickP2p/ickP2pGetDeviceLocation "wikilink")
    - Get UPnP location string
  - [ickP2pGetDeviceLifetime](ickP2p/ickP2pGetDeviceLifetime "wikilink")
    - Get announced lifetime
  - [ickP2pGetDeviceUpnpVersion](ickP2p/ickP2pGetDeviceUpnpVersion "wikilink")
    - Get announced UPnP service version
  - [ickP2pGetDeviceConnect](ickP2p/ickP2pGetDeviceConnect "wikilink")
    - Get device connection state
  - [ickP2pGetDeviceMessagesPending](ickP2p/ickP2pGetDeviceMessagesPending "wikilink")
    - Get number of unsent messages for device
  - [ickP2pGetDeviceMessagesSent](ickP2p/ickP2pGetDeviceMessagesSent "wikilink")
    - Get number of messages sent to device
  - [ickP2pGetDeviceMessagesReceived](ickP2p/ickP2pGetDeviceMessagesReceived "wikilink")
    - Get number of messages received from device
  - [ickP2pGetDeviceTimeCreated](ickP2p/ickP2pGetDeviceTimeCreated "wikilink")
    - Get time stamp of device discovery
  - [ickP2pGetDeviceTimeConnected](ickP2p/ickP2pGetDeviceTimeConnected "wikilink")
    - Get time stamp of device connection

## Messaging

  - [ickP2pSendMsg](ickP2p/ickP2pSendMsg "wikilink") - Send a
    message

## Debugging

  - [ickP2pSetHttpDebugging](ickP2p/ickP2pSetHttpDebugging "wikilink")
    - Enable or disable HTTP debugging interface
  - [ickP2pGetDebugInfo](ickP2p/ickP2pGetDebugInfo "wikilink") - Get
    debugging info as string
  - [ickP2pGetDebugPath](ickP2p/ickP2pGetDebugPath "wikilink") - Get
    debugging URI for a discovered device

[Category:API](Introduction "wikilink")
