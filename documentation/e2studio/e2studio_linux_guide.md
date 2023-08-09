# E2 Studio on Linux

**And some tips on how to use it**

***Note:*** Done on Ubuntu 22.04

***Note:*** This page was created during work with Blue and Green (CK-RX65N and RX65NCloudKit respectively)boards and IoTC codebase for them, so the paths and names may be different for other boards/projects.

## Installation

Download 2022 04 version of GCCRX instead of the latest

Download and install e2 studio. [download](https://www.renesas.com/us/en/software-tool/e2studio-information-rx-family)

***Note:*** The following options are required for Cloudkit development: RX support, and optionally Jlink debugging, terminal, and git support.

Download and install GCCRX toolchain. [download](https://llvm-gcc-renesas.com/rx-download-toolchains/).

***Note:*** Don't use the latest GCCRX (2023/05 particularly. Couldn't get it to build (while working on Blue and Green boards)). Try using 2022/04 instead.

Reboot. (this is important for some reason)

**Windows E2Studio installation example (with screenshots):** [here](/documentation/iotc-azurertos-sdk/samples/ck-rx65n/DEVELOPER_GUIDE.md).

## Configuring and building

Clone and import project (include nested if needed)

Manually add toolchain to e2 studio - `Help->Add Renesas Toolchains -> Add -> <path to installed toolchain>` (this needs to be done once per E2Studio installation)

Under ck-rx65n project add that installed toolchain by right clicking on top directory for every subdirectory (basic-sample, filex, etc...) in `C/C++ Project settings -> Toolchain`

Open `basic-sample/basic-sample.scfg` in E2 Studio 

Down in the opened window there should be "Components" tab. Open it and download any grey (prompted) packages.

Generate code

Build

### If build fails

#### Fixing missing dependencies

Most of the problems can be fixed with fixing include paths - `Right click failed project top directory (threadx f.e.) -> C/C++ Project Settings -> C/C++ General -> Paths and Symbols -> GNU C -> Add -> Workspace` and provide path to parent directory of missing file.

It's generally a good idea to check if unresolved function/include is actually defined somewhere before doing above.

#### If file is actually missing

That will probably be caused of fails in `Generate Code` step. Code is generated according to `.scfg` files, so it's a good idea to look into it via another text editor or E2Studio Smart Configurator (check components tab firstly).

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