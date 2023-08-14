# e² studio on Linux

**And some tips on how to use it**

***Note:*** Tested on Ubuntu 22.04

***Note:*** This page was created during work with Blue and Green (the CK-RX65N and the RX65NCloudKit respectively) boards and IoTC codebase for them, so the paths and names may be different for other boards/projects.

## Installation

- Download and install [GCCRX toolchain](https://llvm-gcc-renesas.com/rx-download-toolchains/).

***Note:*** All the testing was done on 2022/04 version of GCCRX. However, others managed to get it working with 2023/05.

***Note:*** Previous note only relates to Blue and Green board projects (CK-RX65N and RX65NCloudKit).

- Download and install [e² studio for Linux](https://www.renesas.com/us/en/software-tool/e2studio-information-rx-family) (2023-07 version was used)
***Note:*** Default install path will be `~/.local/share/renesas/e2_studio/`<br>
The following options are required for CloudKit development: RX support, and optionally Jlink debugging, terminal, and git support.

- Reboot. (this is important for some reason)

## Configuring and building

Clone and import project (include nested if needed)

Manually add toolchain to e² studio - `Help -> Add Renesas Toolchains -> Add -> <path to installed toolchain>` (this needs to be done once per e² studio installation). ***Note:*** Default install directory will be in `~/toolchains/`

Under ck-rx65n project add that installed toolchain by right clicking on top directory for every subdirectory (basic-sample, filex, etc...) in `C/C++ Project settings -> Toolchain`

Under your project, you will need to update the selected toolchain, select the `basic-sample` folder and right click and access `C/C++ Project settings -> Toolchain`, select the correct toolchain and apply. Most probably, you'll need to do it for the rest of subdirectories under top project folder (netxduo, filex, etc...).

Open `basic-sample/basic-sample.scfg` in e² studio.

***Maybe*** You will get a FIT packages pop-up, proceed to update the FIT packages. 

***IMPORTANT:*** Down in the opened window there should be "Components" tab. Open it and download any grey (prompted) packages.

Generate code

Build

### If build fails

#### Fixing missing dependencies

Most of the problems can be fixed with fixing include paths - `Right click failed project top directory (threadx f.e.) -> C/C++ Project Settings -> C/C++ General -> Paths and Symbols -> GNU C -> Add -> Workspace` and provide path to parent directory of missing file.

It's generally a good idea to check if unresolved function/include is actually defined somewhere before doing above.

#### If file is actually missing

That will probably be caused of fails in `Generate Code` step. Code is generated according to `.scfg` files, so it's a good idea to look into it via another text editor or e² studio Smart Configurator (check components tab firstly).

## Flashing

Install dependencies (e² studio fails to enter Debug mode/start GDB server witout this):

`sudo apt-get install libusb-1.0-0-dev`

`sudo apt-get install libusb-1.0-0`

`sudo apt-get install dotnet-sdk-6.0`

[Additional Information and ways to flash](#additional-links)

### GDB

Right-click on project `basic-sample -> Debug As -> Debug Configurations`.

Choose `GDB Hardware Debugging -> basic-sample HardwareDebugging -> Debugger` -> untick `Use Remote Target`-> `Apply -> Debug`

Right-click on project `basic-sample -> Debug As -> Debug Configurations`.

In `Renesas GDB Hardware Debugging -> basic-sample Hardware Debugging -> Debugger`:

***NOTE:*** This setup is valid just for CK-RX65N (Blueboard) and RX65NCloudKit (Greenboard)
In GDB Settings Tab:
 - Debug Hardware - `E2Lite (RX)`
 - Target Device - `RSF565NE`
 - Autostart local GDB Server

In Connection Settings Tab:
 - Connection type - `fine`

`Apply`

`Debug`

When asked if you want to restart server - click yes.

***Note:*** Another way to start debugger is `Right Click on your project -> Renesas GDB Hardware Debugging`. This requires previous setup steps to be completed.
 
## Additional links

- https://www.renesas.com/eu/en/software-tool/renesas-flash-programmer-programming-gui#overview

- https://www.renesas.com/eu/en/document/mat/renesas-flash-programmer-v311-flash-memory-programming-software-users-manual?r=488871

- once you download and unzip RFP head to `docs` directory and read `rfp-cli.md` file 

- [Windows E2Studio installation example (with screenshots)](/documentation/iotc-azurertos-sdk/samples/ck-rx65n/DEVELOPER_GUIDE.md).

- In case e² studio fails to install due to dependencies, click [here](https://en-support.renesas.com/knowledgeBase/19934358)