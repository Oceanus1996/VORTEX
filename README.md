# VORTEX
This repository contains the official code and dataset for the paper:
"Virtual Vision, Real Bugs: Characterizing and Detecting Visualization Bugs in VR Applications."

## Directory Structure
```
project-root/
├── README.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── README.md
│   ├── raw/
│   └── processed/
└── src/
    ├── vr_auto_detect.py  
```
## Reproduction Instructions
Install dependencies listed in requirements.txt. (Includes HuggingFace libraries; alternatively, you may configure GroundingDINO manually.)

Set up the SteamVR developer environment.

Compile the virtual controller driver to generate a DLL file. Select the appropriate version and place the DLL in steam/steamapp/steamvr/driver/. Once configured, you should see a virtual controller named simplecontroller appear in your SteamVR interface or console.

Use OBS to capture the 2D visual stream from SteamVR. If you use other methods to obtain the video stream, please modify the corresponding functions in the code.

Run src/vr_auto_detect.py (the main entry point). If you run this task within a Unity project, please ensure the project integrates SteamVR and has the correct entry point configured.

  

# SimpleController

***This step generates the necessary DLL files that serve as the foundation for the VR controller drive

## Project Structure

### Source Code
```
simplecontroller/
├── resources/
│   ├── icons/
│   ├── input/
│   │   └── mycontroller_profile.json  
│   ├── localization/
│   │   └── localization.json         
│   └── settings/
│       ├── default.vrsettings         
│       └── driver.vrresources        
├── src/
│   ├── output/                         # Build output directory
│   ├── controller_device_driver.cpp    # Main controller device driver
│   ├── controller_device_driver.h      # Driver header file
│   ├── device_provider.cpp             # Device provider implementation
│   ├── device_provider.h               # Device provider header
│   └── hmd_driver_factory.cpp          # HMD driver factory
├── Win32/                              
├── x64/                              
├── simplecontroller.vcxproj            
├── simplecontroller.vcxproj.user     
└── driver.vrdrivermanifest             
```

## Build Steps

1. Open `simplecontroller.vcxproj` project file
2. Select target platform in Visual Studio:
   - `Win32` - 32-bit version
   - `x64` - 64-bit version
3. Choose build configuration (Debug or Release)
4. Click "Build Solution" or press Ctrl+Shift+B
5. After compilation, DLL files will be generated at:
   - `bin/win32/driver_simplecontroller.dll` (32-bit)
   - `bin/win64/driver_simplecontroller.dll` (64-bit)

