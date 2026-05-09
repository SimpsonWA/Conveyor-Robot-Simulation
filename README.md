# Requirements

## Software Requirements

The following software and packages are required to run the robot simulation and PLC environment.

---

## 1. Open Industry Project

This robot simulation is built using the Open Industry Project framework, which includes a custom fork of the Godot runtime/editor and industrial communication plugins.

The release package already contains:
- Project files
- Custom Godot editor/runtime
- Industrial communication extensions
- Required simulation dependencies

A separate Godot installation is **not required** for normal use.

Repository:  
https://github.com/Open-Industry-Project/Open-Industry-Project

### Installation

Download the latest release package from the repository releases page and extract the archive.

After extraction:

1. Open the extracted project directory
2. Launch the included executable or custom Godot editor
3. Open the simulation project from the Project Manager

---

## 2. OpenPLC Editor

PLC programs are created using OpenPLC Editor.

Official website:  
https://openplcproject.com

Downloads:  
https://autonomylogic.com/downloads/

Supported IEC 61131-3 languages include:

- Ladder Logic (LD)
- Structured Text (ST)
- Function Block Diagram (FBD)

---

## 3. OpenPLC Runtime v4.0

The simulation communicates with the PLC through OpenPLC Runtime version 4.0 using OPC UA communication.

Repository:  
https://github.com/thiagoralves/OpenPLC_v3

Installation Guide:  
https://autonomylogic.com/docs/openplc-runtime/installation/

### Recommended Configuration

| Setting | Value |
|---|---|
| Runtime Version | v4.0 |
| Communication Protocol | OPC UA |
| OPC UA Endpoint | opc.tcp://127.0.0.1:4840 |
| Host Address | 127.0.0.1 |

---

# Communication Architecture

The project uses the Open Industry Project communication framework (`OIPComms`) to exchange data between the simulation and OpenPLC Runtime.

Communication flow:

```text
Open Industry Simulation
        ⇅ OPC UA
OpenPLC Runtime v4.0
```

# Setup Instructions

This project consists of two main components:

- The **Open Industry Project simulation** (Godot-based `.tscn` scene)
- The **OpenPLC project** (provided as a zipped PLC project)

Communication between both systems is handled exclusively using **OPC UA**.

---

# 1. Open Industry Project Setup

The simulation runs on the Open Industry Project framework, which includes a custom Godot-based editor/runtime.

## Important
You must start from the provided **default project**, not directly from the `.tscn` file.

---

## Requirements
- Open Industry Project release package (includes executable + default project)
- No separate Godot installation required for runtime usage

---

## Steps

### Step 1 — Launch the Open Industry Project

1. Download or clone the repository:
   ```
   git clone https://github.com/Open-Industry-Project/Open-Industry-Project.git
   ```

2. Extract the release package (if not already extracted)

3. Launch the included Open Industry Project executable or editor

---

### Step 2 — Open the Default Project

1. In the Project Manager, open:
   ```
   default_project
   ```

2. Wait for the editor to fully load the default scene

---

### Step 3 — Replace the Default Simulation Scene

1. In the Scene panel, locate the default scene (usually named something like):
   ```
   Main.tscn / DefaultScene.tscn
   ```

2. Remove or unload the existing scene content

3. Import or open your project scene:
   ```
   YourSimulation.tscn
   ```

4. Set it as the active main scene:
   ```
   Project → Project Settings → Run → Main Scene
   ```

5. Save the project

---

### Step 4 — Run the Simulation

1. Press Play inside the Open Industry editor
2. Verify the simulation loads correctly
3. Confirm no missing node or resource errors

---

# 2. OpenPLC Project Setup

The PLC logic is provided as a zipped OpenPLC project.

## Requirements
- OpenPLC Editor
- OpenPLC Runtime v4.0

---

## Steps

### Step 1 — Install OpenPLC Editor

https://openplcproject.com  
https://autonomylogic.com/downloads/

---

### Step 2 — Import Project

1. Extract:
   ```
   OpenPLC_Project.zip
   ```

2. Open OpenPLC Editor

3. Select:
   ```
   File → Open Project
   ```

4. Load the extracted folder

---

### Step 3 — Compile PLC Program

- Build → Compile
- Generate PLC code

---

### Step 4 — Upload to Runtime

1. Start OpenPLC Runtime v4.0
2. Open web interface:
   ```
   http://127.0.0.1:8080
   ```
3. Upload PLC program
4. Start runtime

---

# 3. OPC UA Communication Setup (ONLY MODE USED)

This project uses OPC UA exclusively.

---

## OpenPLC Configuration

Ensure OPC UA server is enabled in OpenPLC Runtime:

```
opc.tcp://127.0.0.1:4840
```

---

## Open Industry Project Configuration

1. Open **Comms Panel**
2. Enable:
   ```
   Enable Comms ✓
   ```
3. Create Tag Group:
   - Protocol: `opc_ua`
   - Endpoint:
     ```
     opc.tcp://127.0.0.1:4840
     ```

4. Use OPC UA NodeIds as tag names:

```
ns=2;s=Motor_Start
ns=2;s=Conveyor_Running
ns=2;s=Robot_Arm_Enable
```

---

# 4. Run Order

1. Start OpenPLC Runtime
2. Open Open Industry Project (via default_project)
3. Replace scene with `YourSimulation.tscn`
4. Enable Comms
5. Run simulation

---

# 5. Troubleshooting

### Scene does not load
- Ensure you opened `default_project` first
- Confirm main scene is set in project settings

### No PLC communication
- Verify OPC UA endpoint is running
- Check `Enable Comms` is enabled

### Tags not updating
- Confirm NodeIds match OpenPLC export
- Check Tag Group assignment
