# VORTEX
This repository contains the official code and dataset for the paper:
"Virtual Vision, Real Bugs: Characterizing and Detecting Visualization Bugs in VR Applications."

## Directory Structure

project-root
|
├── README.md                              
├── .gitignore    
│
├── data/                   
│   ├── README.md           
│   ├── raw/             
│   └── processed/         
│
├── src/                   
│ 
│   ├── vr_auto_detect.py
│   └── ...            
│
└── requirements.txt    

## Reproduction Instructions
Install dependencies listed in requirements.txt. (Includes HuggingFace libraries; alternatively, you may configure GroundingDINO manually.)

Set up the SteamVR developer environment.

Compile the virtual controller driver to generate a DLL file. Select the appropriate version and place the DLL in steam/steamapp/steamvr/driver/. Once configured, you should see a virtual controller named simplecontroller appear in your SteamVR interface or console.

Use OBS to capture the 2D visual stream from SteamVR. If you use other methods to obtain the video stream, please modify the corresponding functions in the code.

Run src/vr_auto_detect.py (the main entry point). If you run this task within a Unity project, please ensure the project integrates SteamVR and has the correct entry point configured.

  



