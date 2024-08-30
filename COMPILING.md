# Build Instructions

## Set-up build environment

Tested on Windows 10 based on [this tutorial](https://www.lesimprimantes3d.fr/forum/topic/43956-tuto-bien-installer-son-environnement-de-d%C3%A9veloppement-pour-compiler-son-firmware-marlin/) (French)

1. Install [Git for Windows](https://git-scm.com/downloads). Prefer 64 bits installer. Default settings are OK.
    * _If changing settings during install,_ **make sure to keep Git in PATH**: "Git from the command line and also 3rd-party software".
2. Install [Python](https://www.python.org/downloads/). Make sure to **add python.exe to PATH** when installing.
3. Install [Visual Studio Code](https://code.visualstudio.com/Download). Default settings are OK. Launch Visual Studio Code
4. Install [Auto Build Marlin](https://marlinfw.org/docs/basics/auto_build_marlin.html) in Visual Studio Code
    * Ctrl+Shift+X, then type `Auto Build Marlin` in search field and click `Install`. Wait for install to complete.
5. Install [PlatformIO IDE](https://platformio.org/install/ide?install=vscode) in Visual Studio Code
    * Ctrl+Shift+X, then type `PlatformIO IDE` in search field, make sure it is already installed. Auto Build Marlin should have it as dependency, so Visual Studio Code should have installed it already.
6. Close Visual Studio Code to make sure it finishes installing add-ons

## Open the project

There are additional set-up steps that take place once you open the project for the first time:

1. Clone the repository (or download it as zip)
    * Preferably in a simple folder path such as `C:\MarlinFirmware`, to avoid issues with long paths, spaces or special characters in path.
    * To make sure the project was cloned or extracted correctly, check that folder contains a `platformio` INI file.
2. Open the project folder in Visual Studio Code
    * Use keyboard shortcut Ctrl+K _(nothing happens)_ then keyboard shortcut Ctrl+O _(prompts for folder)_. Navigate to folder and click `Select Folder`.
3. Visual Studio asks to **trust the project folder**. This is required for compiling the project.
4. Look to the Visual Studio Code status bar: It should download and install dependencies in the background. **Wait for this process to complete, it may take a while**.
    * Example: "PlatformIO Installer: Installing PlatformIO Code".
    * PlatformIO may ask you to restart Visual Studio Code. If so, close it and relaunch it to continue the configuration process.
5. Once Visual Studio Code completed all the tasks, proceed to configure firmware

## Configure Firmware

Once you have the project loaded in Visual Studio Code, let's configure it for your printer:

1. Open `platformio.ini` and uncomment the correct motherboard revision by removing the leading `#`.

    * There must be exactly one uncommented line so make sure to comment the others.

```C
default_envs = mks_robin_nano35 # For MKS Robin Nano V1.2
#default_envs = mks_robin_nano_v1_3_f4 # For MKS Robin Nano V1.3
#default_envs = mks_robin_nano_v3_usb_flash_drive # For MKS Robin Nano V3
```

2. In `Marlin` > `configuration.h`, adjust printer model:

    * Adjust printer model by removing the leading `//`. There must be exactly one uncommented printer model so make sure to comment the others.

```C
#define D12_230_v1_2
//#define D12_230_v1_3
//#define D12_500_Pro
//#define D12_300_Pro
```

3. Still in `Marlin` > `configuration.h`, enable or disable settings:

```C
// Enable this if your printer has a 3D Touch
#define BL_TOUCH

// Enable this if your printer has a Direct Drive extruder
//#define Direct_Drive

// Enable this if you replaced all A4988 drivers with TMC2209
//#define D12_230_FULL_TMC

// Enable for printers capable of handling two extruders
#define Dual_Extruder
```

Note: `Dual_Extruder` can be enabled even if the printer currently has only one extruded installed. However, other settings must match exactly your printer configuration.

4. Once firmware is configured, proceed to build the firmware

## Building

Finally, you can build your new firmware:

1. Click on the little ✔ checkmark in status bar or use Ctrl+Alt+B keyboard shortcut
2. Wait for build to complite. This can take a while.
3. Go to `<project folder>\.pio\build` to find your firmware

* For MKS Robin Nano V1.2: `<project folder>\.pio\build\mks_robin_nano35\Robin_nano35.bin`

## Installing firmware

Flashing the firmware is pretty straightformward:

1. Take an empty micro SD card
2. Place firmare file on SD card
3. Insert SD card in 3D printer
4. Turn on 3D printer

Note: On my printer, I sometimes had freezes while flashing the firmware. I needed to turn off the printer, reinsert the micro SD card in computer, delete any leftover files, copy again the firmware file, and try again.