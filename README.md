
# Unsupported version of chutney

This version of chutney contains some modifications that lets you run the system on the `loopback` interface using different aliases for this one.   

The `aliases` shell script creates different alias on the interface.
The IP block used is 128.0.0.0/24 due to problems in setting the `LocalOutbounBindAddress` parameter on the torrc file.
The scripts needs to be run with sudo permissions using the following command:

`./aliases up/down #nodes`

This will setup or tear down the aliases depending on the choice (_up_ or _down_).  
## Original chutney README


This is chutney.  It doesn't do much so far.  It isn't ready for prime-time.

If it breaks, you get to keep all the pieces.

It is supposed to be a good tool for:
  - Configuring a testing tor network
  - Launching and monitoring a testing tor network
  - Running tests on a testing tor network

Right now it only sorta does these things.

You will need, at the moment:
  - Tor installed somewhere in your path or the location of the 'tor' and
    'tor-gencert' binaries specified through the environment variables
    CHUTNEY_TOR and CHUTNEY_TOR_GENCERT, respectively.
  - Python 2.7 or later

Stuff to try:

**Standard Actions:**  
```
./chutney configure networks/basic  
./chutney start networks/basic  
./chutney status networks/basic  
./chutney verify networks/
./chutney hup networks/basic  
./chutney stop networks/basic  
```
**Bandwidth Tests:**  
```
./chutney configure networks/basic-min  
./chutney start networks/basic-min  
./chutney status networks/basic-min  
CHUTNEY_DATA_BYTES=104857600
./chutney verify networks/basic-min  
# Send 100MB of data per client connection  
# verify produces performance figures for:  
# Single Stream Bandwidth: the speed of the slowest stream, end-to-end  
# Overall tor Bandwidth: the sum of the bandwidth across each tor instance  
# This approximates the CPU-bound tor performance on the current machine,  
# assuming everything is multithreaded and network performance is infinite.  
./chutney stop networks/basic-min
```
**Connection Tests:**  
```
./chutney configure networks/basic-025
./chutney start networks/basic-025
./chutney status networks/basic-025
CHUTNEY_CONNECTIONS=5
./chutney verify networks/basic-025
# Make 5 connections from each client through a random exit  
./chutney stop networks/basic-025
```
Note: If you create 7 or more connections to a hidden service from a single
client, you'll likely get a verification failure due to
https://trac.torproject.org/projects/tor/ticket/15937

**HS Connection Tests:**  
```
./chutney configure networks/hs-025  
./chutney start networks/hs-025  
./chutney status networks/hs-025  
CHUTNEY_HS_MULTI_CLIENT=1 ./chutney verify networks/hs-025  
# Make a connection from each client to each hs  
# Default behavior is one client connects to each HS  
./chutney stop networks/hs-025
  ```

**The configuration files:**  
networks/basic holds the configuration for the network you're configuring above.
It refers to some torrc template files in torrc_templates/.

**The working files:**  
chutney sticks its working files, including all data directories, log
files, etc, in ./net/.  Each tor instance gets a subdirectory of net/nodes.
