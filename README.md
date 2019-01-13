# sbnetio

I do not own any code of this here, but since the original download on the google code page got deleted, this is a new approach to host this still useful plugin for LogitechMediaServer(LMS)https://www.mysqueezebox.com/index/Home

Server and Plugin are seperated.
You may try to add this repo to LMS: https://raw.githubusercontent.com/TheAppService/sbnetio/master/LMS_Plugin/repo.xml




Automatically exported from code.google.com/p/sbnetio


## Original Readme Below ##
## Table of Contents ##


# What SBNetIO actually is #
SBNetIO is a LMS (_Logitech Media Server_, formerly known as _Squeezebox Server_) plugin.

The plugin forwards Squeezebox events to a listening server by means of network communication. On server side these messages can be translated to arbitrary actions.

Thus, in combination with a server the plugin offers a mechanism to run remote commands. So, to use the plugin you will need to setup another server - on its own, the plugin is pretty  useless.

Based on my personal requirement for a mechanism to switch amps, SBNetIO focusses on the support of pairs of _On-_ and _Off-_Commands.

These commands can be launched
  * manually by user interaction; for this purpose SBNetIO extends the Squeeze Box controller's _Extras_ menu, or
  * automatically based on player state changes. Upon _Play_ and _(Un)Pause_ the "Turn-On" cmds are sent; upon _Stop_, _Pause_, _Power Off_ and when the end of the current Playlist is reached "Turn-Off" is triggered.

A single player may feed multiple amps - either directly by cabling or indirectly through  synchronised SB players - and one may wish to switch those amps individually. To address this, SBNetIO introduces the concept of _zones_; a _zone_ is a listening area which has its own amp.

For each player SBNetIO lets you define up to 3 _zones_. A zone
  * has a name,
  * a pair of _On-_ and _Off-_Commands,
  * and can be set to be switched automatically.

**Remark & Example:** Besides individual players in the kids rooms and home office I have two synced players which feed all other rooms - those players are located on first and second floor and have individual amps. In SBNetIO this translates into having 2 zones for those players: "Wohnzimmer" (first floor) and "Galerie" (second floor). I use 433MHz plugs to switch the amps (only works with old fashioned amps with real switches) which nicely work through walls and over different stories.

The name and concept of this plugin is inspired from [NetIO](http://netio.davideickhoff.de), a generic remote control smartphone app. SBNetIO is fully compatible to NetIO.

Please visit the [thread on the Logitech forum](http://forums.slimdevices.com/showthread.php?100514-Yet-another-Remote-Control-Plugin-SBNetIO) for comments and questions.

# Example Setup #

The following image shows a schematic view of a typical usage scenario of SBNetIO.

The plugin 'talks' to a server running on a Raspberry Pi which translates the messages into 433MHz commands which then are received and executed by 433MHz plugs.

![http://www.specifica.de/public/sbnetio/images/Setup.png](http://www.specifica.de/public/sbnetio/images/Setup.png)

To keep the above image simple, it shows a setup with only one single zone.

The choice of a Raspberry Pi as a server and the usage of the 433MHz equipment reflects my personal setup - the plugin does not make any assumptions on the the downstream setup. Another approach would be to use the Pi's GPIO pins to drive a relay or just forward the messages to a network-enabled amp or one of various home automation servers.

For Denon Amps there is a dedicated and much more powerful Plugin available: [Denon AVP Control Plugin](http://code.google.com/p/denonavpcontrol/).



---

# SBNetIO UI #

Screenshots of Squeeze Control app showing SBNetIO menus:

| ![http://www.specifica.de/public/sbnetio/images/Extras.png](http://www.specifica.de/public/sbnetio/images/Extras.png) | ![http://www.specifica.de/public/sbnetio/images/Extras_SBNetIO.png](http://www.specifica.de/public/sbnetio/images/Extras_SBNetIO.png) | ![http://www.specifica.de/public/sbnetio/images/SBNetIO_Zone.png](http://www.specifica.de/public/sbnetio/images/SBNetIO_Zone.png) |
|:----------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------|
| SBNetIO Integration in Extras menu                                                                                    | SBNetIO Home Screen                                                                                                                   | Zone Screen                                                                                                                       |

The SBNetIO home screen shows:
  * the current (internal) state of power as derived from the player state and delays: Can be _Power On_ or _Power Off_ - but may also indicate that the power is still on but will be turned off soon. So this power state is not identical to the player state and also not necessarily identical to the actual zone (amp) power state, since the zone power could have been overruled by means of another remote control. In a sense, this power state presents the _expected_ state of a synced zone - currntly there is no way to retrieve the actual state of the plugs (one way communication). Anyway - during normal operation, this power state will be identical to the zone (amp) power state. Use the zone power buttons to sync the actual zone power with the internal power state in case of a mismatch.
  * the individual zones. This example shows two zones: 'Wohnzimmer' and 'Galerie'. An orange pin signals automatic switching mode. Tapping a zone opens a submenu, to switch it individually.
  * a button to turn on all zones
  * a button to turn off all zones



---

# SBNetIO Installation #

To install the plugin go to the LMS settings page, open the plugin tab page and copy this URL
```
  http://www.specifica.de/public/sbnetio/download/BSF/repo.xml
```

to the _Additional repositories_ input section at the bottom of the page.

The above link points to latest version - for older versions refer to the download section below.


![http://www.specifica.de/public/sbnetio/images/RepoURL.png](http://www.specifica.de/public/sbnetio/images/RepoURL.png)

Hit the _Apply_-Button and select SBNetIO from the list of discovered plugins.


Restart the Logitech Media Server server for the plugin to be recognized.

## Deactivation ##

To remove the plugin from the server uncheck the SBNetIO plugin from the list of Plugins in the Logitech Media Server web control.



---

# SBNetIO Configuration #

In LMS Settings UI click on Player-Tab, select a player, and click on SBNetIO.

![http://www.specifica.de/public/sbnetio/images/admin1.png](http://www.specifica.de/public/sbnetio/images/admin1.png)

The SBNetIO settings page is divided into a general section and 3 zone sections:

![http://www.specifica.de/public/sbnetio/images/admin2.png](http://www.specifica.de/public/sbnetio/images/admin2.png)

The information ("i") buttons on the web form provide all necessary information about meaning and expected format of the input data.

The cmd input fields can contain lists of individual commands (see next paragraph for cmd-syntax) and/or timers, separated by semi-colon, e.g.

```
   TurnOnCmd = Cmd1;Cmd2;n1;Cmd3;n2;Cmd4;...
```

Numbers are treated as timers; i.e. in the above example Cmd1 and Cmd2 are executed immediately after turn on, wheras Cmd3 is executed after n1 seconds and Cmd4 after (n1+n2) seconds.


## Socket-Communication ##

By default a socket connection to the specified IP and port is established.

A port must be given; so the connection strings need to have the format:

```
   Connection = [<protocol>:]<IP>:<Port>
```

Protocol may be 'tcp' or 'udp' - if omitted, 'tcp' is used.

A carriage return is appended to the command before it is sent.


## Http-Communication ##

If the connection string starts with 'http://', a http-GET request is issued; the URL is created by concatination of the connection string and command:
```
   URL = <Connection><Cmd>                   
```

In case of basic access authentication the connection string may also contain a username and password; so the general format of a http connection strings is
```
   Connection = http://[<user>:<password>@]<IP>[:<port>]
```

The command could look like
```
   Cmd = /protected/HomeControl.asp?KitchenLamp=On&BathroomLamp=Off
```

Since the strings are simply concatinated, it may be handy to move common parts of commands to the connection.






---

# A simple Automation Server #
Any computer can be used for the server, but small single board computers like RaspBerry Pi, Arduino, AVR-Net-IO, etc. are surely the most reasonable platform because they come with easy to use GPIO interfaces. I use a RaspBerry Pi as platform.

A server written in Python is included in the package; please refer to the Download section below.

The server expects messages of the form
```
   Cmd [Args]
```

where _Cmd_ is an executable located at
```
  /home/pi/netio
```

Right now the path is hard coded but can be modified easily.

The cmd _RCSend_ which is mentioned at several places, is a shell script and will be discussed in the following section.

I keep server, a test client and command scripts in one location:

![http://www.specifica.de/public/sbnetio/images/WinSCP_netio.png](http://www.specifica.de/public/sbnetio/images/WinSCP_netio.png)

There are a bunch of simple python client/server programs available for Raspberry Pi based Home Automation. However, most tend to include much code concerning the actual scenario (e.g. names of rooms, ...) whereas I was looking for a more generic approach. Also I noticed that some simple server codes are not stable - especially, when they are contacted from mutiple clients.

The provided server code is based on a contribution in the [NetIO forum](http://netio.davideickhoff.de/forum/topic/87/). I simplified it and fixed a bug which led to crashes now and then. Now the current version of the server runs stable for weeks on my Pi.

The server is started with:
```
  sudo python /home/pi/netio/netio_server.py
```

Server responsiveness can be tested by means of the included client script; e.g.:

```
  sudo python /home/pi/netio/netio_client.py RCSend 11111 4 1
```

The port (54321) which is used is also hardcoded in the python programs.


To start the server every time the machine is started, _crontab_ can be used. Enter
```
  crontab -e
```
and add the line to start the server at the end of the crontab file
```
@reboot python /home/pi/netio/netio_server.py &
```

![http://www.specifica.de/public/sbnetio/images/crontab.png](http://www.specifica.de/public/sbnetio/images/crontab.png)


---

# 433MHz Transmitter #

There are lots of cheap 433MHz transmitters available - google for it or look at Amazon, EBay, ... The home page of [PowerPi](http://raspberrypiguide.de/howtos/powerpi-raspberry-pi-haussteuerung/) gives a nice overwiew of 433MHz modules and plug bundles.

Wiring is pretty simple - if the module has no antenna, attach a wire of 17cm length (length and orientation of the antenna is crucial, try for best results).

![http://www.specifica.de/public/sbnetio/images/RaspPi.jpg](http://www.specifica.de/public/sbnetio/images/RaspPi.jpg)

I drilled a hole into the Pi case for the cabling and hot glued the module on the top.

![http://www.specifica.de/public/sbnetio/images/Foto.jpg](http://www.specifica.de/public/sbnetio/images/Foto.jpg)


As already mentioned the cmd _RCSend_ is a shell script and reads
```
#!/bin/bash

/home/pi/raspberry-remote/./send $*
```

and just calls _send_ from the 433MHz library (Links with information about the setup of _send_ are given below).

_send_ - and in turn _RCSend_ - expects 3 arguments:
```
   send HomeCode PlugID State
```
where HomeCode is the code set at the respective plug (and remote control) by means of dip switches (a sequence of five 1s or 0s), PlugID is 1, 2, 3 or 4, and the State is either 1 (On) or 0 (Off).

![http://www.specifica.de/public/sbnetio/images/Elro.jpg](http://www.specifica.de/public/sbnetio/images/Elro.jpg)

Elro plugs come in bundles together with a remote control, which has 4 pairs of buttons to switch four plugs. So, the command line of _send_ pretty much reflects the capabilities of this remote control.

The shell script is a good place to map arguments if desired; so instead of calling
```
  RCSend 11111 4 1
```

one could support more readable cmds like
```
  RCSend KitchenLight On
```
or similar.


---

# NetIO Example Solution #

A NetIO example configuration is included in the package. It can be used for trouble shooting (e.g. to test server availability and responsiveness) or just as an alternative control. The UI is pretty self explanatory.

![http://www.specifica.de/public/sbnetio/images/NetIO.png](http://www.specifica.de/public/sbnetio/images/NetIO.png)

Screenshot of NetIO controller app; SBNetIO configuraion loaded

The configuration file can be found in the Download section below. To use it, just register at [NetIO](http://netio.davideickhoff.de), create a new project, open the json-Editor and paste the content of the configuration file. Then click on _Save Online_.

On your smartphone you can update and choose the configuration to use through the app's context menu. You have to enter the credentials chosen upon registration.

You can also use this configuration as one page of a larger NetIO project, which may consist of a couple of pages and also includes control of lights, heating, etc.


---

# Credits & Links #

## Ressources ##
| NetIO | http://netio.davideickhoff.de/ |
|:------|:-------------------------------|
| Simple Java Server to run on PC to monitor SBNetIO | http://netio.davideickhoff.de/tutorials#pc |
| Wiring Pi | http://wiringpi.com/           |
| Raspberry Pi Remote | https://github.com/xkonni/raspberry-remote |
| Tutorial to use 433MHz plugs with Raspberyy Pi (sorry german) | http://www.forum-raspberrypi.de/Thread-tutorial-funksteckdosen-433mhz-mit-ios-andoid-app |
| PowerPi (sorry only german as well)| http://raspberrypiguide.de/howtos/powerpi-raspberry-pi-haussteuerung/ |



---

# Release Notes #


## 0.1 ##
  * Initial Release

## 0.2 ##
  * Enhancement: http-protocol support
  * Enhancement: a time to pause playback after zones turn on commands are issued may be specified (i.e. wait for amps to be ready ...)
  * Bugfix: fixed a typo which broke the plugin on linux
  * Bugfix: Depending on used controller not all playback start/stop events were detected and hence the commands have not been sent reliably

## 0.2.5 ##
  * Enhancement: The cmd-parameters can contain several (semi-colon separated) individual commands


## 0.3 ##
  * Bug Fixes


## 0.4 ##
  * udp-protocol Support


---

# Download #

Since [Google deprecated the download service](http://google-opensource.blogspot.de/2013/05/a-change-to-google-code-download-service.html) on code.google.com recently, the project files are hosted on my personal webspace at www.specifica.de.

## Latest Version (BSF = best so far) ##

To install the latest version of the plugin go to the LMS settings page, open the plugin tab page and copy this URL
```
  http://www.specifica.de/public/sbnetio/download/BSF/repo.xml
```


## Specific Versions ##


### Version 0.4 (Current BSF) ###

```
  http://www.specifica.de/public/sbnetio/download/V04/repo.xml
```

| Repo xml                        | [repo.xml](http://www.specifica.de/public/sbnetio/download/V04/repo.xml) |
|:--------------------------------|:-------------------------------------------------------------------------|
| LMS Plugin                      | [SBNetIO\_04.zip](http://www.specifica.de/public/sbnetio/download/V04/SBNetIO_04.zip) |



### Version 0.3 ###

```
  http://www.specifica.de/public/sbnetio/download/V03/repo.xml
```

| Repo xml                        | [repo.xml](http://www.specifica.de/public/sbnetio/download/V03/repo.xml) |
|:--------------------------------|:-------------------------------------------------------------------------|
| LMS Plugin                      | [SBNetIO\_03.zip](http://www.specifica.de/public/sbnetio/download/V03/SBNetIO_03.zip) |



### Version 0.2 ###

```
  http://www.specifica.de/public/sbnetio/download/V02/repo.xml
```

| Repo xml                        | [repo.xml](http://www.specifica.de/public/sbnetio/download/V02/repo.xml) |
|:--------------------------------|:-------------------------------------------------------------------------|
| LMS Plugin                      | [SBNetIO\_02.zip](http://www.specifica.de/public/sbnetio/download/V02/SBNetIO_02.zip) |



### Version 0.1 ###

```
  http://www.specifica.de/public/sbnetio/download/V01/repo.xml
```


| Repo xml                        | [repo.xml](http://www.specifica.de/public/sbnetio/download/V01/repo.xml) |
|:--------------------------------|:-------------------------------------------------------------------------|
| LMS Plugin                      | [SBNetIO\_01.zip](http://www.specifica.de/public/sbnetio/download/V01/SBNetIO_01.zip) |
| Python Server & Client          | [netio\_server\_client.zip](http://www.specifica.de/public/sbnetio/download/V01/netio_server_client.zip) |
| SBNetIO configuration for NetIO | [SBNetIO.json](http://www.specifica.de/public/sbnetio/download/V01/SBNetIO.json) |
| Complete package                | [Complete\_01.zip](http://www.specifica.de/public/sbnetio/download/V01/Complete_01.zip) |




