# E2 Studio on Linux

***Note:*** Done on Ubuntu 22.04

***Note:*** This page was created during work with Blue and Green (CK-RX65N and RX65NCloudKit respectively)boards and IoTC codebase for them, so the paths and names may be different for other boards/projects.

## Installation

Download 2022 04 version of GCCRX instead of the latest

install e2 studio.

install GCCRX toolchain.

Reboot.

## Configuring and building

Import project (include nested)

Manually add toolchain to e2 studio - `Help->Add Renesas Toolchains -> Add -> <path to installed toolchain>`

Under ck-rx65n project add that installed toolchain by right clicking on top directory for every subdirectory (basic-sample, filex, etc...) in `C/C++ Project settings -> Toolchain`

Open `basic-sample/basic-sample.scfg` in E2 Studio 

Down in the opened window there should be "Components" tab. Open it and download any grey (prompted) packages.

Generate code

Build

## Flashing

Install dependencies:

`sudo apt-get install libusb-1.0-0-dev`

`sudo apt-get install libusb-1.0-0`

`sudo apt-get install dotnet-sdk-6.0`

[Additional Information and ways to flash](#additional-links)

### GDB

Right-click on project `basic-sample -> Debug As -> Debug Configurations`.

Choose `GDB Hardware Debugging -> basic-sample HardwareDebugging -> Debugger` -> unclick Use Remote Target if it's clicked -> `Apply -> Debug`

Right-click on project `basic-sample -> Debug As -> Debug Configurations`.

In Renessas GDB Hardware Debugging -> basic-sample Hardware Debugging -> Debugger:

***NOTE:*** This setup is valid just for CK-RX65N (Blueboard) and RX65NCloudKit (Greenboard)
In GDB Settings Tab:
 - Debug Hardware - E2Lite (RX)
 - Target Device - RSF565NE
 - Autostart local GDB Server

In Connection Settings Tab:
 - Connection type - fine

Apply

Debug

When asked if you want to restart server - click yes.

***Note:*** Another way to start debugger is `Right Click on your project -> Renessas GDB Hardware Debugging`. This requires previous setup steps to be completed.
 
## Additional links

https://www.renesas.com/eu/en/software-tool/renesas-flash-programmer-programming-gui#overview

https://www.renesas.com/eu/en/document/mat/renesas-flash-programmer-v311-flash-memory-programming-software-users-manual?r=488871

once you download and unzip RFP head to `docs` directory and read `rfp-cli.md` file 