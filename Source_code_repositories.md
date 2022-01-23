## Overview

To make it a bit easier for third party developers to keep their work
updated with the most recent version of the open parts of the source
code of ickStream we have decided to expose the source code through a
git repository. The git repositories are hosted on github
<https://github.com/ickStream>

All applications using the ickStream Music Platform requires a specific
API-key, contact us for more information if you want to develop and
ickStream based app and we will arrange so you get an API-key.

## Repositories

### ickStream P2P module

  - Contains the P2P module which is the core of the communication and
    discovery on the local network
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-p2p.git>
`cd ickstream-p2p`

Pre-requisities:

  - libz library either must be installed on the machine or stored in
    the "lib" directory
  - libwebsockets library either must be installed on the machine or
    stored in the "lib" directory

To build it as a static module, just run:

`make all`

This will produce a static module:

    lib/libickp2p.a

which you can link with your application.

If you don't have libwebsockets header files installed on the machine,
you can build it as follows and point to a specific location of
libwebsockets header files

`make all INCLUDES=-I../libwebsockets/lib`

And to clean the build completely run:

`make cleanall`

### ickStream iOS ickComm

  - Contains access code for iOS which simplifies communication with our
    cloud service and other devices on the local network
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-ios-comm.git>
`cd ickstream-ios-comm`
`git submodule update --init --recursive`

To build it you use the committed Xcode project and compile as normal
through Xcode

### ickStream Java wrapper for P2P module

  - Contains some Java JNI wrappers around P2P module to make it easier
    to use from Java
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-java-p2p.git>
`cd ickstream-java-p2p`
`git submodule update --init --recursive`

To build it you need to install maven and after that you can run:

`mvn install`

And to clean the build completely run:

`mvn clean`

### ickStream Java Common module

  - Contains various Java code which makes it easy to build ickStream
    applications in Java
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-java-common.git>
`cd ickstream-java-common`

There are a number of different modules

  - jsonrpc: Java JSON-RPC helper classes that makes it easy to parse
    JSON-RPC or call JSON-RPC services
  - ickprotocol: Java representation of all ickStream Music Platform
    protocols to make it easy to communicate with ickStream
    devices/services
  - ickplayer: Generic Java implementation of the [Player
    Protocol](API/Player_Protocol "wikilink") to make it easier to
    implement a Java based ickStream player device
  - ickcontroller: Typical Java utility classes which is needed when you
    are developing an ickStream controller in Java

To build it you need to install maven and after that you can in each
module directory run:

`mvn install`

And to clean the build completely run:

`mvn clean`

### ickStream Java Samples module

  - Contains a few different Java based sample applications
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-java-samples.git>
`cd ickstream-java-samples`

There are a number of different modules

  - sample-java-controller: A simple Java based controller that
    discovers available devices/services on local network and in cloud
    server account
  - sample-java-player: A simple Java based player that act as a player
    but doesn't output any sound

To build it you need to install maven and after that you can in each
module directory run:

`mvn install -DBUILD_ENVIRONMENT=linux`

The possible values for the BUILD_ENVIRONMENT variable are:

  - osx - Intel 32 and 64 bit OSX
  - linux - Intel 32 and 64 bit Linux

And to clean the build completely run:

`mvn clean`

### ickStream LMS plugin

  - Contains the LMS plugin which expose the local LMS music to your
    ickStream devices
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-service-lms.git>
`cd ickstream-service-lms`
`git submodule update --init --recursive`

Pre-requisitites:

  - You must have
    [cmake](http://www.cmake.org/cmake/resources/software.html)
    installed

To build it you need to install maven and build the ickStream Java
wrapper for P2P module as described above and then run:

`mvn install -DBUILD_ENVIRONMENT=linux`

The possible values for the BUILD_ENVIRONMENT variable are:

  - osx - Intel 32 and 64 bit OSX
  - linux - Intel 32 and 64 bit Linux
  - linuxarm - ARM soft float Linux (For example SheevaPlug)
  - linuxarmhf - ARM hard float Linux (For example Raspberry Pi or
    Wandboard)

And to clean the build completely run:

`mvn clean`

### ickStream Squeezebox player

  - Contains the very experimental Squeezebox player which contains an
    implementation of the Player Protocol which could be useful to look
    at if someone wants to build their own player. We would love to get
    a C-based Linux player which could be executed on Squeezebox
    hardware instead of the current one which is implemented as an
    applet and is kind of a hack since it relies on both LMS or
    mysqueezebox.com to be able to play anything.
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-squeezebox-player.git>
`cd ickstream-squeezebox-player`
`git submodule update --init --recursive`

Pre-requisities:

  - You must have
    [cmake](http://www.cmake.org/cmake/resources/software.html)
    installed
  - You must have Squeezebox cross compile environment installed. The
    Squeezebox.cmake file presumes it is installed with:
      - Root file system at:
        /usr/src/poky/build/tmp-jive/staging/armv5te-none-linux-gnueabi
      - Compiler at:
        /usr/src/poky/build/tmp-jive/cross/armv5te/bin/arm-none-linux-gnueabi-gcc

To build it you need to setup a Squeezebox cross compile environment and
after that you can run something like:

`make dist`

And to clean the build completely, you run:

`make cleanall`

### ickStream Linux daemon player

  - Contains an experimental Linux daemon player which is based on ALSA
    with the intention that it can be used as a base for a player on
    embedded Linux devices.
  - **License**: BSD

Checkout code as follows:

`git clone `<https://github.com/ickStream/ickstream-daemon-player.git>
`cd ickstream-daemon-player`

To build it you do as follows:

`# Step 1: Build libwebsockets as described in its section on this page`
`# Step 2: Build ickstream-p2p as described in its section on this page`
`# Step 3:`
``ICK_ROOT=`pwd`/../ickstream-p2p ./configure debug``
`make`

And to clean the build completely run:

`make cleanall`

### libwebsockets

  - Contains a customized version of the libwebsockets library
  - Only used indirectly as a submodule so you don't have to check it
    out specifically.
  - **License**: LGPL

Checkout code as follows:

`git clone `<https://github.com/ickStream/libwebsockets.git>

Pre-requisitites:

  - You must have
    [cmake](http://www.cmake.org/cmake/resources/software.html)
    installed

To build it you do as follows

`mkdir libwebsockets/target`
`cd libwebsockets/target`
`cmake .. -DWITH_SSL=0`
`make`
`cd ../..`

And to clean the build completely, you run:

`rm -rf libwebsockets/target`