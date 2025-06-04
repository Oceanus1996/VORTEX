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

Windows controller driver project.

## Project Structure

### Source Code
```
simplecontroller/
├── resources/
│   ├── icons/
│   ├── input/
│   │   └── mycontroller_profile.json   # Controller profile
│   ├── localization/
│   │   └── localization.json           # Localization file
│   └── settings/
│       ├── default.vrsettings          # Default VR settings
│       └── driver.vrresources          # Driver resources
├── src/
│   ├── output/                         # Build output directory
│   ├── controller_device_driver.cpp    # Main controller device driver
│   ├── controller_device_driver.h      # Driver header file
│   ├── device_provider.cpp             # Device provider implementation
│   ├── device_provider.h               # Device provider header
│   └── hmd_driver_factory.cpp          # HMD driver factory
├── Win32/                              # 32-bit build files
├── x64/                                # 64-bit build files
├── simplecontroller.vcxproj        
├── simplecontroller.vcxproj.user      
└── driver.vrdrivermanifest             
```

## Build Steps

1. Open `simplecontroller.vcxproj` project file in Visual Studio
2. Select target platform:
   - `Win32` - for 32-bit systems
   - `x64` - for 64-bit systems
3. Choose build configuration (Debug or Release)
4. Build the solution
5. The compiled DLL and related files will be generated in:
   - `Win32/` directory for 32-bit build
   - `x64/` directory for 64-bit build

This step generates the necessary DLL files that serve as the foundation for the VR controller driver.

## Driver Deployment

### Build Output Structure
After compilation, the following files are generated:
```
bin/
├── win32/                              # 32-bit build output
│   ├── driver_simplecontroller.dll
│   ├── driver_simplecontroller.exp 
│   ├── driver_simplecontroller.lib    
│   └── driver_simplecontroller.pdb   
└── win64/                              # 64-bit build output
    ├── driver_simplecontroller.dll   
    ├── driver_simplecontroller.exp   
    ├── driver_simplecontroller.lib     
    └── driver_simplecontroller.pdb     
```

### Installation
1. Copy the appropriate DLL file based on your system architecture:
   - For 32-bit: `bin/win32/driver_simplecontroller.dll`
   - For 64-bit: `bin/win64/driver_simplecontroller.dll`
2. Deploy the complete `resources/` directory structure
3. Include the `driver.vrdrivermanifest` file
4. Final driver structure should be:
   ```
   VR_Driver_Directory/
   ├── bin
   |   ├── win64                              #x64 as example
   |   |   ├── driver_simplecontroller.dll   
   |   |   ├── driver_simplecontroller.exp   
   |   |   ├── driver_simplecontroller.lib     
   |   |   └── driver_simplecontroller.pdb   
   ├── resources/
   │   ├── icons/
   │   ├── input/
   │   │   ├── legacy_bindings_simplecontroller.json
   │   │   ├── mycontroller_profile.json
   │   │   ├── simple_controller_remapping.json
   │   │   └── testui.json
   │   ├── localization/
   │   │   └── localization.json
   │   └── settings/
   │       ├── default.vrsettings
   │       └── driver.vrresources
   └── driver.vrdrivermanifest
   ```
   Place the entire driver folder (such as simplecontroller/ or VR_Driver_Directory/) into the SteamVR drivers directory:
    ```
        SteamVR installation path/steamapps/common/SteamVR/drivers/
    ```
