# VORTEX
This repository contains the official code and dataset for the paper:
"Virtual Vision, Real Bugs: Characterizing and Detecting Visualization Bugs in VR Applications."

## Directory Structure
```
project-root/
├── README.md
└──  VORTEX
    ├── data/
    │   ├── RQ1&RQ2/
    │   │   ├── visible_objects.xlsx    # have 16 files of it 
    │   │   └── name_num.txt
    │   │  
    │   ├── RQ3/
    │   │   ├── BYPROJECT/  #VB by project
    │   │   ├── v1/ #VB1
    │   │   ├── v2/  #VB2
    │   │   ├── v3/   #VB3
    │   │   └── ...
    ├── Driver/
    │   ├── simplecontroller/
    │   └── VR_Driver_Directory/
    └── VR_Detect/
        ├── cam_pos_st.py           # Utilities for acquiring/converting HMD/controller poses and coordinates
        ├── indle.py                # Performs depth estimation (MiDaS) and computes object distances
        ├── remapping_motion.py     # Controller movement, command generation, and remapping logic
        ├── reprojection.py         #  reprojection and first-frame calibration/correction
        ├── script_st.py            # Inter-process communication with drives
        ├── vr_auto_detect.py       # Main entry: automated object detection, tracking, and VR controller interaction
        └── environment.yml         # Conda environment configuration for reproducible setup

```
# Setup Instructions

## Create and Activate Conda Environment

```bash
conda env create -f environment.yml
conda activate dino_new
```
(You can customize the environment name as needed. Includes HuggingFace libraries. For custom GroundingDINO setup, configure environment manually)

## Configure SteamVR Development Environment

Ensure SteamVR is correctly installed and working on your system.

## Compile Virtual Controller Driver

Build the simplecontroller driver DLL with Visual Studio. See the "driver setup" section in this repository for detailed configuration and placement instructions.

## Prepare Video Stream

Use OBS software to capture the 2D video stream from SteamVR. If using alternative capture methods, modify the video acquisition function in the code.

## Run Main Entry Script

```bash
python src/vr_auto_detect.py
```

This script is the main entry point for automated detection and control.

## (Optional) Unity Integration

If integrating with Unity projects, ensure SteamVR is properly integrated and the correct script entry point is configured.


## Setup Driver

openvr-driver in virtual driver injection module, after this step set succcessfully, you will see a virtual controller shown in SteamVR interface

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

### Build Dll File

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
