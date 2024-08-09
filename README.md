## _NASSCOM_VSD_SOC_Design

# DAY 1 Inception of open-source EDA, OpenLANE and Sky130 PDK

# How to talk to computers

<img width="1512" alt="Screenshot 2024-08-03 at 10 33 49 AM" src="https://github.com/user-attachments/assets/a6dcf662-8617-4f0f-a06d-1b0cd34e217e">

The Arduino board can be represented using a block diagram. In this diagram, the processor is surrounded by various interfaces such as Vcc/GND, an SRAM chip, JTAG, and others. This layout is typical for designing an Arduino board.

<img width="1512" alt="Screenshot 2024-08-03 at 10 58 57 AM" src="https://github.com/user-attachments/assets/37b28893-d818-4d90-bee7-387ad797ed4d">

The diagram illustrates a system centered around a Processor/SoC (System on Chip).** This core component is connected to various peripherals and memory modules. It communicates with external devices via direct interfaces like I2C, QSPI, and GPIOs. For memory, it utilizes SDRAM chips, likely for high-speed data access. Additionally, the system includes slower memory options like EEPROM and potentially Flash memory (not explicitly shown). The processor also interacts with debugging and programming interfaces like JTAG and UART. Overall, the diagram provides a high-level overview of the system's architecture, emphasizing the processor's role as the central processing unit.

<img width="1226" alt="Screenshot 2024-08-03 at 11 01 53 AM" src="https://github.com/user-attachments/assets/d87efd4b-842c-43d6-81cb-72e7a679c83c">

The image depicts a top-down view of a QFN-48 package.This type of package is a square, flat package with no leads, commonly used for integrated circuits. The package measures 7mm by 7mm and houses the chip at its center. The chip itself is not visible in this image but is connected to the package's external pins, or pads, through microscopic wire bonds. These pins provide the interface for electrical connections to the chip, enabling communication and power supply. The package also includes markings for orientation and pin identification. 

<img width="1214" alt="Screenshot 2024-08-03 at 11 05 08 AM" src="https://github.com/user-attachments/assets/9b7c5cc2-1d2a-4418-8f9c-c4ab731357f8">

<img width="1221" alt="Screenshot 2024-08-03 at 11 05 22 AM" src="https://github.com/user-attachments/assets/e6b41649-06ac-4cdb-a44a-2f106a02345b">

Chip Layout

The image illustrates a top-down view of a chip's architecture.

Core: At the center lies the RISC-V SoC (System on Chip), the chip's core processing unit.

Peripherals: Surrounding the core are various functional blocks, including a GPIO bank for general input/output, SRAM for fast data storage, and an unspecified block labeled "comp_inn."

Pads: The chip's outer perimeter is lined with pads, the physical connection points for power supply (VDD), ground (VSS), clock signals (CLK), and data transfer (e.g., flash_io, ser_tx, ser_rx).

Foundry IP's and Macros: While not visually distinct in this image, the chip likely incorporates pre-designed building blocks from the foundry and custom-designed macros to enhance its functionality.

# Introduction to RISC V
<img width="1459" alt="Screenshot 2024-08-03 at 11 05 42 AM" src="https://github.com/user-attachments/assets/ed8c3d00-53db-4062-9b08-e20bef811971">

The image provides a snapshot of the RISC-V development process. On the left, a code editor displays C code implementing a "swap" function, which will be compiled into machine code. The middle section shows the assembly code generated for the "swap" function, revealing the RISC-V instructions used. On the right, a block diagram outlines the architecture of a potential RISC-V implementation, the "picorv32" core, detailing its components and control logic. The image underscores the relationship between high-level code, assembly language, and the underlying hardware architecture in the context of RISC-V. 

<img width="1458" alt="Screenshot 2024-08-03 at 11 18 30 AM" src="https://github.com/user-attachments/assets/3d986af3-f230-4935-b941-1e3a9d6d2dfd">
The image illustrates the design flow from high-level software instructions to physical hardware implementation in a RISC-V processor.
Starting with assembly code representing an "add" instruction, the assembler converts it into machine code. This machine code, representing the Instruction Set Architecture (ISA), serves as an abstract interface to the hardware. The RTL (Register Transfer Level) code, written in a hardware description language, defines the logic for processing these instructions. This RTL is then synthesized into a netlist, a detailed description of the circuit's components and connections. Finally, the physical design implementation translates the netlist into a layout of transistors and wires on a silicon chip, creating the actual hardware capable of executing the original "add" instruction. 

#  SoC design and OpenLANE

Introduction to all components of open-source digital asic design


<img width="1411" alt="Screenshot 2024-08-03 at 12 11 42 PM" src="https://github.com/user-attachments/assets/aca4a2b1-0974-4a30-b7ff-d4018d504f54">

1. EDA Tools:

qflow: This is the overall flow manager orchestrating the entire design process. It handles task scheduling, resource allocation, and data management.
OpenROAD: A comprehensive tool suite covering various design stages, including synthesis, placement, and routing. It optimizes the chip layout for performance and power efficiency.
OpenLANE: A complete flow for digital ASIC implementation, encompassing tasks like synthesis, physical design, and sign-off. It provides a streamlined approach to the design process.

2. RTL Designs:

These are the initial designs written in hardware description languages like Verilog or VHDL. They capture the chip's functionality at a high level.
librecores.org, opencores.org, and github.com: These platforms offer a rich repository of open-source RTL designs for various components, accelerating the design process.

3. PDK Data:

Process Design Kit (PDK): This essential data package provides detailed information about the manufacturing process, including transistor models, design rules, and library cells.
Google Skywater PDK: A prominent open-source PDK for a 130nm process, enabling the fabrication of chips through Skywater Technology Foundry.


# Simplified RTL2GDS flow

<img width="1407" alt="Screenshot 2024-08-03 at 12 12 07 PM" src="https://github.com/user-attachments/assets/ef8f56a6-e030-4ddf-aaa0-ba7a9aa9dd3b">

1. RTL (Register Transfer Level):

This is the starting point, where the design is captured in a hardware description language like Verilog or VHDL.
It represents the high-level behavior of the circuit, defining how data is transferred between registers.

2. PDK (Process Design Kit):

Contains detailed information about the manufacturing process, including transistor models, design rules, and library cells.
Essential for ensuring the design is compatible with the chosen fabrication technology.

3. GDSII:

The final output, a file format containing the geometric data used to manufacture the integrated circuit (IC).
Flow Stages:

Synthesis:

Translates the RTL design into a gate-level netlist, representing the circuit using logic gates.
Optimization tools are used to minimize area, power consumption, and delay.
Floorplanning (FP) and Power Planning (PP):

Determines the overall chip layout, including the placement of major functional blocks and power distribution networks.
Ensures efficient power delivery and ground connections.
Placement:

Assigns physical locations to individual logic gates and standard cells within the chip area.
Optimization algorithms aim to minimize wire length and delay.
Clock Tree Synthesis (CTS):

Designs the clock distribution network, ensuring that all clock signals arrive at flip-flops with minimal skew and jitter.
Critical for timing closure and circuit performance.
Routing:

Connects the logic gates and other components using metal wires within the chip layers.
Routing algorithms optimize wire length, congestion, and signal integrity.
Sign-Off:

Verifies that the final design meets all design constraints and requirements.
Includes static timing analysis (STA), power analysis, and layout verification.

# Introduction to OpenLANE and Strive chipsets

<img width="1406" alt="Screenshot 2024-08-03 at 12 12 51 PM" src="https://github.com/user-attachments/assets/17a8ae36-b90f-4740-b6fb-96afcf0cf1ac">

Main Goal

The primary objective of the OpenLANE flow is to automate the entire ASIC design process from RTL to GDSII without requiring human intervention. This is a significant step towards achieving a fully autonomous design flow.

Definition of a "Clean" GDSII

A "clean" GDSII, as defined in the image, adheres to the following criteria:

No LVS Violations: Logical and physical representations of the design must match perfectly.
No DRC Violations: The design must comply with all design rules specified by the fabrication process.
Timing Violations: Currently under development (WIP). Achieving timing closure without manual intervention is a challenging but crucial goal.
Significance of OpenLANE

By striving for a completely automated flow with a clean GDSII output, OpenLANE aims to streamline the ASIC design process, reduce time-to-market, and minimize design errors. This aligns with the broader industry trend towards increased design automation and reduced reliance on human expertise.

In essence, the image highlights OpenLANE's ambition to become a fully autonomous ASIC design platform capable of producing high-quality, manufacturable designs without human intervention.


# Introduction to OpenLANE ASIC flow

<img width="1405" alt="Screenshot 2024-08-03 at 12 13 45 PM" src="https://github.com/user-attachments/assets/8b705ab2-4c9f-45a1-bf3e-670306a94d4c">

OpenLANE ASIC flow is a comprehensive, open-source framework designed to automate the entire process of creating an integrated circuit (IC) from Register Transfer Level (RTL) code to a manufacturable GDSII file. It encompasses a wide range of design stages, from synthesis and floorplanning to physical verification.

Key Components and Stages

1) Design Input:

RTL: The initial design is captured in RTL code, typically using Verilog or VHDL. This code describes the circuit's functionality at a high level.
PDK (Process Design Kit): Provides detailed information about the manufacturing process, including transistor models, design rules, and standard cell libraries. It ensures compatibility between the design and the fabrication technology.

2) Synthesis:

RTL Synthesis: The RTL code is translated into a gate-level netlist using tools like Yosys and ABC. This netlist represents the circuit using logic gates.
Synthesis Exploration: Multiple synthesis options are explored to find the best trade-offs between area, power, and performance.

3) Floorplanning:

OpenROAD App: This tool is used for initial chip layout planning. It determines the placement of major functional blocks and power distribution networks.
Placement:

The logic gates and standard cells are assigned specific physical locations within the chip area. Optimization algorithms are employed to minimize wire length and delay.

4) Power Planning:

The power distribution network is designed to ensure efficient power delivery and ground connections throughout the chip.

5) Clock Tree Synthesis (CTS):

The clock distribution network is generated to deliver clock signals to all flip-flops with minimal skew and jitter. This is crucial for timing closure.

6) Optimization:

Various optimization techniques are applied to improve design characteristics such as area, power, and performance.

7) Detailed Routing:

The physical connections between logic gates and other components are established using metal wires within the chip layers. Routing algorithms aim to minimize wire length, congestion, and signal integrity.

8) Static Timing Analysis (STA):

The timing performance of the design is analyzed to ensure it meets the specified timing constraints. OpenSTA is used for this purpose.

9) Physical Verification:

The design is checked for errors such as layout versus schematic (LVS) mismatches and design rule checks (DRC) violations. Tools like Magic and Netgen are employed.

10) Design for Testability (DFT):

Structures are added to the design to enable testing and fault diagnosis.

11) GDSII Output:

The final design is converted into a GDSII file, which is the standard format for manufacturing data

## Get familiar to open-source EDA tools

# OpenLANE Directory structure in detail

OpenLANE: Streamlining Your Design Journey

OpenLANE leverages a collection of open-source Electronic Design Automation (EDA) tools. Its primary purpose is to automate the entire process of transforming your Register Transfer Level (RTL) code into a GDSII file, ready for chip manufacturing.

Navigating Your Design Workflow:

![image](https://github.com/user-attachments/assets/8ccf7f40-3214-404f-8576-3e988fd7b24b)


Here's a step-by-step guide using Linux commands to navigate  OpenLANE project:

Locate Your Tools:

1) Begin by opening a terminal and navigate to your tools directory using the command cd Desktop/work/tools.
Access Your Working Directory:

2) Next, move to your specific OpenLANE project directory with cd openlane_working_dir.
Explore the PDK (Process Design Kit):

3) To delve into the Sky130A process details, enter cd pdks/sky130A/. This directory contains information about the chip fabrication process.
View PDK Files:

4) To see a list of files related to Sky130A (ordered by date with the newest at the top), use ls -ltr.
Locate Reference Library Files:

5) Access the directory containing reference library files specific to the chosen process with cd libs.ref.
Review Reference Library Files:

6) Utilize ls -ltr again to view the files within the libs.ref directory.
Explore Tool-Specific Library Files:

7) Navigate back one level using cd .. and then move to the libs.tech directory containing files specific to the OpenLANE tools themselves.
Check Tool-Specific Library Files:

8) Finally, use ls -ltr one last time to list the files in the libs.tech directory.

![image](https://github.com/user-attachments/assets/f637e0cd-7b58-4d8c-b253-940bb4ba2474)

Viewing Library Files:

1. Process-Specific Files (libs.ref):
    * Use `cd libs.ref` to access the directory containing reference library files specific to the chosen chip fabrication process.
    * Run `ls -ltr` to list all files within `libs.ref`, sorted by date with the newest at the top.

2. Tool-Specific Files (libs.tech):
    * Navigate back one directory level using `cd ..`.
    * Enter `cd libs.tech` to move to the directory containing files specific to the OpenLANE tools themselves.
    * Finally, use `ls -ltr` again to list all files in `libs.tech`.

# Commands to invoke OpenLANE Commands 

![image](https://github.com/user-attachments/assets/df3b4e8b-ea42-4035-98d3-1151759030cf)

## Launching OpenLANE



To start OpenLANE, follow these steps:

1. **Navigate to the Tools Directory:**
   * Open your terminal and type `cd Desktop/work/tools` to access the tools folder.

2. **Access the OpenLANE Project:**
   * Move into the specific OpenLANE project directory by typing `cd openlane_working_dir`.

3. **Enter the OpenLANE Directory:**
   * Change to the main OpenLANE directory using `cd openlane`.

4. **Start the Docker Container:**
   * Initiate the Docker container by typing `docker`. This sets up the necessary environment for OpenLANE.

5. **Verify OpenLANE Files:**
   * Use `ls -ltr` to list the contents of the OpenLANE directory and verify that all required files are present.

6. **Run the OpenLANE Flow:**
   * Execute the OpenLANE flow in interactive mode with the command `./flow.tcl -interactive`. This starts the design process and allows you to interact with the tool. 


# Steps to prepare for openlane cd designs

![image](https://github.com/user-attachments/assets/892908a4-2438-4ac1-981e-2264f1bd0c5b)


### Understanding the Commands

The provided commands outline the initial steps for setting up an OpenLANE design environment. Let's break down what each command does:

1. **Navigating to the Design Directory:**
   * `cd designs`: This command changes the directory to the `designs` folder, which likely contains various design projects.

2. **Listing Design Projects:**
   * `ls -ltr`: This command lists the contents of the `designs` directory in a long format, sorted by modification time (newest first). You can see the available design projects here.

3. **Selecting a Design:**
   * `picorv32a`: Assuming `picorv32a` is a design project, this command changes the directory to the `picorv32a` folder.

4. **Exploring the Design Structure:**
   * `ls -ltr`: Again, this lists the contents of the `picorv32a` directory to show its structure.

5. **Accessing the Source Code:**
   * `cd src`: This command moves into the `src` directory, which typically contains the source code for the design.

6. **Reviewing Source Files:**
   * `ls -ltr`: Lists the source files within the `src` directory.

7. **Returning to the Design Root:**
   * `cd ../`: This command navigates back to the `picorv32a` directory.

8. **Checking Configuration:**
   * `less config.tcl`: Opens the `config.tcl` file in a pager (less). This file likely contains configuration settings for the OpenLANE flow.

9. **Clearing the Screen:**
   * `clear`: Clears the terminal screen.
  

## Reviewing Design Results and Exploring Files

![image](https://github.com/user-attachments/assets/44a12b7b-5ba1-4c12-8385-9ef1ccb5f1e4)


The provided commands focus on navigating through the results of a design run and examining specific files.

1. **Accessing Design Runs:**
   * `cd runs`: This command takes you to the directory containing various design runs.

2. **Selecting a Specific Run:**
   * `ls -ltr`: Lists the available design runs in chronological order.
   * `cd 12_08_10_49`: This command enters the directory for the run that occurred on August 12, 2010, at 4:49 PM.

3. **Examining Temporary Files:**
   * `ls -ltr`: Lists the files within the specified run directory.
   * `cd temp`: Navigates to the temporary files directory, likely containing intermediate results.
   * `less merged.lef`: Opens the `merged.lef` file using the `less` command to view its contents. This file often combines layout information from different sources.

## Analyzing Synthesis Results
![image](https://github.com/user-attachments/assets/a8c41da5-2e1d-43aa-ba4e-5c1c0e570eb2)

Flop Ratio = Number of flops / Number of cells = 1613/14876 = 0.108 = 10.8%

1. **Navigating to the Results Directory:**
   * `cd results`: Moves to the directory containing the overall results of the design project.

2. **Accessing the Synthesis Results:**
   * `ls -ltr`: Lists the contents of the `results` directory, showing different design stages.
   * `cd synthesis`: Enters the directory specifically for synthesis results.

3. **Examining the Synthesis Report:**
   * `ls -ltr`: Lists the files within the `synthesis` directory.
   * `less picorv32a.synthesis.v`: Opens the `picorv32a.synthesis.v` file in a pager (less) to view the synthesized Verilog code.

4. **Returning to the Parent Directory:**
   * `cd ../ cd../`: Navigates back two levels to the main project directory.

5. **Exploring Reports:**
   * `cd reports`: Moves to the directory containing various reports generated during the design process.

6. **Checking Synthesis Reports:**
   * `ls -ltr`: Lists the files within the `reports` directory.
   * `cd synthesis`: Enters the directory containing synthesis-related reports.

## Day 2 - Good floorplan vs bad floorplan and introduction to library cells
1
# Chip Floor planning considerations


![Screenshot 2024-08-04 at 12 54 59 PM](https://github.com/user-attachments/assets/60644ad7-eefa-4463-9008-9bb1f44f7521)


![Screenshot 2024-08-04 at 12 55 30 PM](https://github.com/user-attachments/assets/3b441d63-aa17-4b61-adf3-7c6eca6d7332)

## Defining Width and Height of Core and Die

## Understanding the Image: Core, Die, and Utilization Factor

### Core and Die Dimensions

The image provides a simplified representation of a chip layout, illustrating the concepts of core and die.

* **Die:** Represents the entire chip area, including the core and other components like I/O pads, power/ground networks, and clock distribution. In the image, the die is a rectangle with dimensions of 4 units (width) by 2 units (height).

* **Core:** Represents the active area of the chip where the primary logic circuitry resides. It's typically a rectangular area within the die. In the image, the core is also a rectangle with dimensions of 2 units (width) by 2 units (height).

### Utilization Factor


```
Utilization Factor = Core Area / Die Area
```

In this case, the core area is 2 units * 2 units = 4 square units, and the die area is 4 units * 2 units = 8 square units. Therefore, the utilization factor is 4 / 8 = 0.5. This means that only 50% of the die area is used for the core, while the remaining 50% is occupied by other components.

### Aspect Ratio



```
Aspect Ratio = Height / Width
```

For both the core and the die in this example, the aspect ratio is 2 units / 4 units = 0.5.

# Define the location of pre placed cell

![Screenshot 2024-08-04 at 12 55 57 PM](https://github.com/user-attachments/assets/84d90303-6fba-4b35-9e60-0d78da6e3dee)

![Screenshot 2024-08-04 at 12 56 12 PM](https://github.com/user-attachments/assets/e2b1892e-6e2a-4d49-820b-df2a196b1c8d)

## Understanding Pre-Placed Cells 

**Pre-placed cells** are essentially pre-defined blocks or modules that are strategically positioned within a chip's layout before the automated placement and routing process begins. These cells often represent complex logic functions or IP cores that have already been designed and verified.


1. **Identification of Blocks:**
   * The diagram initially shows two blocks labeled "Block 1" and "Block 2," each containing multiple logic gates (represented by symbols like A1, A2, etc.).

2. **Black Boxing:**
   * The process of "black boxing" involves treating these blocks as single entities without considering their internal logic. This is akin to representing the blocks as pre-placed cells.

3. **Separation into IPs or Modules:**
   * The image further suggests that these black boxes can be considered as separate Intellectual Property (IP) cores or modules. This implies that they are reusable components that can be integrated into different designs.

4. **Pre-placed Cell Placement:**
   * The final part of the image shows the placement of these blocks (now represented as IP cores or modules) within a chip's layout. This is the concept of pre-placement. The remaining empty space in the chip would be filled with other logic cells during the automated placement and routing process.

# Decoupling capacitors
![Screenshot 2024-08-04 at 12 56 43 PM](https://github.com/user-attachments/assets/eb65158d-b070-4b1d-bc33-baedf231447c)


**Key Components and Functions:**

- **Power Supply (Vdd):** Represents the main power source for the circuit.
- **Decoupling Capacitors (Cd):** These are small capacitors placed in parallel with the power supply lines near the digital ICs. Their primary function is to provide a local reservoir of charge to quickly supply current during sudden changes in circuit operation.
- **Resistors (R1, R2):** Model the internal resistance of the power supply and the wiring connecting it to the circuit.
- **Inductors (L1, L2):** Represent the inductance of the power supply lines and wiring.

**How Decoupling Capacitors Work:**

1. **Current Spikes:** When digital circuits switch states, they draw sudden bursts of current from the power supply.
2. **Capacitor Discharge:** The decoupling capacitor (Cd) discharges quickly to meet this immediate current demand, preventing voltage drops on the power supply line.
3. **Power Supply Replenishment:** The power supply (Vdd) and the RL network (consisting of resistors and inductors) work together to replenish the charge in the decoupling capacitor over time.

**Benefits of Using Decoupling Capacitors:**

- **Reduced Noise and Interference:** By minimizing voltage fluctuations, decoupling capacitors help reduce electromagnetic interference (EMI) and improve signal integrity.
- **Improved Circuit Performance:** Faster switching speeds and reduced power consumption can be achieved with proper decoupling.
- **Increased Circuit Reliability:** Decoupling capacitors can help prevent circuit malfunctions caused by power supply noise.

**Placement and Selection:**

- Decoupling capacitors should be placed as close as possible to the power and ground pins of the ICs to minimize the length of the power supply traces.
- The value of the decoupling capacitor depends on the switching speed and current requirements of the circuit.

# Power planning


![Screenshot 2024-08-04 at 12 57 10 PM](https://github.com/user-attachments/assets/9f4aa0bf-1330-4fd6-bbce-f4457c43bba9)



![Screenshot 2024-08-04 at 12 57 18 PM](https://github.com/user-attachments/assets/61c779d1-649e-453f-ae2a-333ac15babfc)



The Role of PDN

A robust Power Distribution Network (PDN) is essential to address these challenges. Its primary functions include:

1) Delivering Power Efficiently: Ensuring that all parts of the chip receive the necessary power to operate correctly.
2) Minimizing Power Noise: Reducing fluctuations in the power supply voltage caused by current variations, which can disrupt circuit operation.
3) Controlling Voltage Drops (IR Drop): Minimizing the voltage drop across the power distribution network to prevent performance degradation and functional failures.

# Pin Placement 

![Screenshot 2024-08-04 at 1 01 43 PM](https://github.com/user-attachments/assets/04eca8a2-2213-4443-890e-7bdccbaee493)



**Pin Placement Principles:**

While the image provides a basic overview, here are some general pin placement principles:

* **I/O Placement:** Input/output pins (Din, Dout, CLK) are typically placed along the die's periphery to facilitate connections with external devices.
* **Power and Ground:** Vdd and Vss lines are often placed along the die edges to provide easy access and reduce noise coupling.
* **Decoupling Capacitor Placement:** Decoupling capacitors are strategically placed near the power supply pins of logic blocks to filter out high-frequency noise.
* **Core Placement:** The core is usually located in the center of the die to optimize wire length and signal integrity.
* **Contact Placement:** Contacts are distributed throughout the die to connect metal layers to the underlying transistor levels.


![image](https://github.com/user-attachments/assets/252723b3-2e55-4473-9bc6-8cb649618694)

This sequence navigates to the results directory within a specific OpenLANE design run for the "picorv32a" project. It then launches the Magic tool with a configuration file (sky130A.tech) and instructs it to read the design layout information from two files:

merged.lef: This file likely combines the layout details from various sources.
picorv32a.floorplan.def: This file presumably defines the placement and arrangement of different blocks within the chip design (the floorplan).

# placement 

![Screenshot 2024-08-04 at 6 15 15 PM](https://github.com/user-attachments/assets/3163824d-9516-4364-af86-9b95ee3545fa)

Placement Process Overview

The placement step involves arranging these components within the die area. The goal is to optimize the layout for performance, power, and area.

Key considerations in placement:

* Proximity: Components that frequently interact should be placed close together to minimize wire length and delay.
* Power and Ground: Power and ground lines should be distributed evenly to reduce voltage drops and noise.
* Decoupling Capacitor Placement: These should be placed near power supply pins for effective noise filtering.
* Floorplanning: Larger blocks (like Block a, b, and c) are often pre-placed to define the overall chip structure.
![Screenshot 2024-08-04 at 6 19 30 PM](https://github.com/user-attachments/assets/94aa2b98-7cd4-4e94-8a51-21f5a320f8b2)

Optimized Placement with Buffers:

The image showcases a refined version of the placement process where buffers are strategically inserted to enhance performance and signal integrity.

![image](https://github.com/user-attachments/assets/28715996-f9c0-4fe0-8ac7-b20c52cbdccc)

Here's a rephrased version of the commands in a more user-friendly format:

**Running Placement and Viewing the Results in Magic:**

These commands initiate the placement stage within OpenLANE and then open the generated placement information in the Magic tool for visualization.

**Step 1: Running Placement ( script called `run_placement`):**

- This part (not explicitly shown) executes the placement process to determine the layout of different blocks within the chip design.

**Step 2: Viewing the Placement Results in Magic:**

1. **Navigate to the Results Directory:**
   - `cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-07_09-24/results/placement/`
   - This command changes your directory to the specific "placement" subfolder within the results directory of a previous OpenLANE run for the "picorv32a" project.

2. **Launch Magic with Configuration:**
   - `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech`
   - This command starts the Magic tool, specifying the configuration file `sky130A.tech` located within the OpenLANE PDK (Process Design Kit) directory. This file provides information about the technology used to manufacture the chip.

3. **Read Layout and Placement Files:**
   - `lef read ../../tmp/merged.lef`
   - This command instructs Magic to read the design layout information from the `merged.lef` file located two directories above the current location. This file likely combines layout details from various sources.
   - `def read picorv32a.placement.def`
   - This command instructs Magic to read the placement information from the `picorv32a.placement.def` file. This file defines the positions and arrangements of different blocks within the chip design (the floorplan).

4. **Run Magic in the Background (Optional):**
   - `&` (ampersand)
   - Adding the ampersand symbol at the end allows the command to run in the background, and you can regain control of the terminal for other tasks.

**In summary, these commands guide you through running a pla![Screenshot 2024-08-04 at 6 43 32 PM](https://github.com/user-attachments/assets/219a6ee8-fdbe-44d7-87c8-4bcb0dad02df)
cement phase in OpenLANE and then visualizing the generated placement data within the Magic tool.**

# Cell  Design And characterization
![Screenshot 2024-08-04 at 6 37 10 PM](https://github.com/user-attachments/assets/82b9eab6-a7f3-42d2-80f7-de1177b8545b)
![Screenshot 2024-08-04 at 6 37 56 PM](https://github.com/user-attachments/assets/8ab1a02c-1aab-445e-b54a-149039b78271)

![Screenshot 2024-08-04 at 6 43 17 PM](https://github.com/user-attachments/assets/f5ca8205-f3a1-4db3-b3c7-2a1e11908556)
![Screenshot 2024-08-04 at 6 43 32 PM](https://github.com/user-attachments/assets/11e6867a-6332-4b89-a54f-134db83809bb)

![Screenshot 2024-08-04 at 6 43 52 PM](https://github.com/user-attachments/assets/d6c25686-bd50-42f9-af84-6dc2e37b366a)

Th

### Image : Cell Design Flow Overview
- **Inputs:**
  - **Process Design Kits (PDKs):** These include essential components like Design Rule Check (DRC) and Layout Versus Schematic (LVS) rules, SPICE models, and standard libraries. These kits help in setting the constraints and specifications for the design.
  - **User-Defined Specifications:** This includes the specific requirements that the designer inputs based on the target application.
  
- **Design Steps:**
  - **Circuit Design:** This involves creating the circuit schematic based on the desired logic function.
  - **Layout Design:** Translating the circuit design into a physical layout that can be fabricated. This process involves placing and routing the components according to the design rules.
  - **Characterization:** The process of analyzing the performance of the cell, including its timing, power consumption, and other key parameters.
  
- **Outputs:**
  - **CDL (Circuit Description Language):** A representation of the circuit that can be used for verification and as input for further design processes.

### Image : Euler's Path and Stick Diagram
- **Art of Layout:**
  - The left side of the image seems to show a logic circuit with multiple transistors. The transistors are grouped into PMOS and NMOS networks.
  - **Euler’s Path:** Euler's path is a method used in the layout design where the goal is to minimize the number of crossings in the layout. This simplifies the routing and reduces the area used.
  - The **stick diagram** represents the layout at a higher level, abstracting the physical details but showing the connectivity.
  
- **Network Graphs:**
  - The PMOS and NMOS networks are represented as graphs. These help in determining the possible Euler paths that simplify the layout.
  - The sequence "A-C-E-F-D-B" represents the Euler path chosen for the NMOS network to minimize the layout complexity.


### Cell Design and Characterization 
- **Cell Design** involves multiple stages: starting from defining the logic function, selecting the right transistor sizing, and performing schematic capture, to generating the layout and verifying it with respect to the design rules.
- **Characterization** then ensures that the designed cell meets the required performance metrics under various conditions (like voltage, temperature, etc.). This stage is crucial for ensuring the reliability and efficiency of the cell when integrated into larger designs. 

## Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization
![Screenshot 2024-08-04 at 7 50 41 PM](https://github.com/user-attachments/assets/5125a0ae-8528-44a1-97fa-20e7a8860701)
![Screenshot 2024-08-04 at 7 50 49 PM](https://github.com/user-attachments/assets/f794a7b5-585a-42ca-a540-ac9dbe05fca8)
![Screenshot 2024-08-04 at 7 50 55 PM](https://github.com/user-attachments/assets/e958c2c4-36d9-47f4-bb29-678543175b65)
![Screenshot 2024-08-04 at 7 51 00 PM](https://github.com/user-attachments/assets/1f2fe7c6-b5a6-499d-941b-259f6b1bd3ed)



### Image 1: CMOS Inverter Circuit Schematic and Nodes
This image shows the CMOS inverter circuit schematics with different configurations at the top and the detailed node labeling at the bottom. The key components are:
- **M1 and M2**: PMOS and NMOS transistors respectively.
- **Vdd**: The supply voltage.
- **Vss**: The ground.
- **Vin**: The input voltage to the inverter.
- **Vout**: The output voltage of the inverter.
- **Cload**: The load capacitance.

The nodes in the circuit are annotated, indicating where various measurements and connections are made.

### Image 2: SPICE Deck Description
This image shows the SPICE deck for simulating the CMOS inverter. Key sections include:


The diagram on the right visualizes the netlist description.

 Image 3: SPICE Waveform Output
This image shows the SPICE simulation output waveform for the CMOS inverter:
- **X-axis (in)**: Input voltage (Vin) ranging from 0 to 2.5V.
- **Y-axis (out)**: Output voltage (Vout).
- **Curve**: Indicates the inverter's switching behavior, showing how the output voltage changes as the input voltage varies.

The parameters for the transistors are:
- \( W_n = W_p = 0.375u \)
- \( L_n = L_p = 0.25u \)


### Image 4: Comparison of SPICE Waveforms
This image shows two SPICE waveforms with different device dimensions:
- **Left waveform**: Same as Image 3.
- **Right waveform**: Different transistor dimensions.
  - \( W_n = 0.375u \)
  - \( W_p = 0.9375u \)
  - \( L_n = L_p = 0.25u \)
  

The right waveform shows the impact of the increased width of the PMOS transistor (M1), which affects the switching characteristics and the output voltage behavior.

### Explanation of SPICE Deck Creation
A SPICE deck is essentially a text file containing:
1. **Model Descriptions**: Specifies the parameters of the transistors used in the circuit.
2. **Netlist Description**: Defines the connectivity and components of the circuit.
3. **Simulation Commands**: Instructions for running the simulation, such as operating points (.op), DC sweep (.dc), and including external models (.include, .LIB).

![image](https://github.com/user-attachments/assets/4d8b13fb-7623-4060-ade6-5635c60eedc0)

![image](https://github.com/user-attachments/assets/b9d7dbf8-50df-4c19-b483-e29ce26da013)

![image](https://github.com/user-attachments/assets/87a993f8-2569-4830-8d51-013a8a1c972e)

![image](https://github.com/user-attachments/assets/3098412e-ba70-48a2-9a2c-32e1baf742cc)

![image](https://github.com/user-attachments/assets/432aa9c1-58d9-4309-bbf3-50d7589e318c)


**1. Change Directory:**

- `cd Desktop/work/tools/openlane_working_dir/openlane`
   - This command navigates you to the "openlane" directory within your OpenLANE working directory on your Desktop.

**2. Clone the Repository:**

- `git clone https://github.com/nickson-jose/vsdstdcelldesign.git`
   - This command uses the `git clone` command to download the contents of the "vsdstdcelldesign" repository from GitHub, created by the user "nickson-jose", into your current directory.

**3. List Directory Contents:**

- `ls -ltr`
   - This command lists the contents of your current directory in long format, providing details like file permissions, owner, group, size, and last modification time. This helps you confirm the successful cloning of the repository.

**4. Enter the Repository Directory:**

- `cd vsdstdcelldesign`
   - This command changes your directory to the newly cloned "vsdstdcelldesign" directory.

**5. List Directory Contents (Optional):**

- `ls -ltr`
   - This command (optional) lists the contents of the "vsdstdcelldesign" directory, showing the files and folders within the custom inverter design.
 
Within the "vsdstdcelldesign" directory, this command sequence first lists the directory contents using `ls -ltr` and then launches the Magic tool (`magic`) with the configuration file `sky130A.tech` and the design file `sky130_inv.magic`. The ampersand symbol (`&`) runs Magic in the background, allowing you to continue using the terminal for other tasks. 

The "16 Mask CMOS Process" refers to a specific fabrication process for creating CMOS (Complementary Metal-Oxide-Semiconductor) integrated circuits, typically using 16 photolithographic masks. Each mask corresponds to a specific step in the manufacturing process, defining different layers and patterns on the semiconductor wafer. Here's a brief overview of the 16 Mask CMOS Process:

### Key Steps in the 16 Mask CMOS Process:

1. **Substrate Preparation**: Start with a clean silicon wafer.

2. **Well Formation**:
   - **Mask 1**: Define the n-well regions using photolithography.
   - **Mask 2**: Define the p-well regions.

3. **Threshold Voltage Adjustment**:
   - **Mask 3**: Adjust the threshold voltage (Vt) for NMOS transistors.
   - **Mask 4**: Adjust the threshold voltage for PMOS transistors.

4. **Isolation**:
   - **Mask 5**: Create isolation regions, typically using LOCOS (Local Oxidation of Silicon) or STI (Shallow Trench Isolation).

5. **Gate Oxide Formation**: Grow a thin layer of silicon dioxide on the wafer surface.

6. **Polysilicon Gate Formation**:
   - **Mask 6**: Deposit and pattern polysilicon to form the gate electrodes for NMOS and PMOS transistors.

7. **Lightly Doped Drain (LDD) Implantation**:
   - **Mask 7**: LDD implantation for NMOS transistors.
   - **Mask 8**: LDD implantation for PMOS transistors.

8. **Spacer Formation**: Deposit and etch a spacer layer (typically silicon nitride or silicon dioxide) on the sides of the polysilicon gates.

9. **Source/Drain Implantation**:
   - **Mask 9**: Source/drain implantation for NMOS transistors.
   - **Mask 10**: Source/drain implantation for PMOS transistors.

10. **Silicide Formation**:
    - **Mask 11**: Form silicide contacts on the polysilicon gates and source/drain regions to reduce resistance.

11. **Interlayer Dielectric (ILD) Deposition**: Deposit an insulating layer over the entire wafer.

12. **Contact Formation**:
    - **Mask 12**: Etch contact holes through the ILD to the silicide regions.

13. **Metal Deposition and Patterning**:
    - **Mask 13**: Deposit and pattern the first layer of metal interconnects.
    - **Mask 14**: Deposit and pattern the second layer of metal interconnects.

14. **Passivation Layer**:
    - **Mask 15**: Deposit a passivation layer to protect the circuit and etch openings for bond pads.

15. **Bond Pad Formation**:
    - **Mask 16**: Define the bond pad openings for external connections.

### Explanation of the Masks:

1. **Mask 1 & 2**: N-well and P-well formation for the PMOS and NMOS transistors.
2. **Mask 3 & 4**: Threshold voltage adjustment implants.
3. **Mask 5**: Isolation regions to separate different transistors.
4. **Mask 6**: Gate polysilicon patterning for the transistor gates.
5. **Mask 7 & 8**: Lightly doped drain implants to reduce hot carrier effects.
6. **Mask 9 & 10**: Source/drain implants for NMOS and PMOS transistors.
7. **Mask 11**: Silicide formation to improve conductivity.
8. **Mask 12**: Contact holes for electrical connections.
9. **Mask 13 & 14**: Metal interconnect layers for wiring the circuit.
10. **Mask 15**: Passivation layer to protect the circuit.
11. **Mask 16**: Bond pad openings for connecting the chip to external circuits.

# Lab introduction to Sky130 basic layers layout and LEF using inverter

<img width="815" alt="Screenshot 2024-08-09 at 9 37 20 AM" src="https://github.com/user-attachments/assets/77a5198b-62a6-4d7c-8123-821b75e9d4c8">

<img width="825" alt="Screenshot 2024-08-09 at 9 37 42 AM" src="https://github.com/user-attachments/assets/20741c07-4610-40b9-9c75-abc6f1ae79c6">

<img width="825" alt="Screenshot 2024-08-09 at 9 37 54 AM" src="https://github.com/user-attachments/assets/7d4e17ef-88ad-457e-8252-4c176e9a37cf">

<img width="826" alt="Screenshot 2024-08-09 at 9 38 09 AM" src="https://github.com/user-attachments/assets/0f05a4ca-38ad-497f-95bf-3add724ae06f">

<img width="825" alt="Screenshot 2024-08-09 at 9 38 23 AM" src="https://github.com/user-attachments/assets/84be0cab-2f29-41b4-b971-e12c1d5d8fbf">

<img width="827" alt="Screenshot 2024-08-09 at 9 38 42 AM" src="https://github.com/user-attachments/assets/cf25cc22-2731-4919-acbb-1770964d1be2">
<img width="829" alt="Screenshot 2024-08-09 at 9 38 51 AM" src="https://github.com/user-attachments/assets/7c8e1fff-8e81-4e36-9311-b0737292a0ae">
<img width="829" alt="Screenshot 2024-08-09 at 9 39 01 AM" src="https://github.com/user-attachments/assets/4c4b3111-346e-4ecc-b3c1-c6cdff4df562">\

<img width="830" alt="Screenshot 2024-08-09 at 9 39 11 AM" src="https://github.com/user-attachments/assets/ea7a35c7-73e6-467d-a31a-a6139eaca0fd">
<img width="836" alt="Screenshot 2024-08-09 at 9 39 21 AM" src="https://github.com/user-attachments/assets/e27e5d71-1cc6-4a71-87b2-832d46a450bf">
<img width="822" alt="Screenshot 2024-08-09 at 9 39 29 AM" src="https://github.com/user-attachments/assets/0167f324-475f-46f2-9176-65de9cf2c8ff">

<img width="815" alt="Screenshot 2024-08-09 at 9 40 00 AM" src="https://github.com/user-attachments/assets/97b99b28-03b7-4852-900a-e583c75cef1e">
<img width="815" alt="Screenshot 2024-08-09 at 9 39 42 AM" src="https://github.com/user-attachments/assets/2c27dde0-11f6-478d-ae28-88ef98a1438a">
<img width="724" alt="Screenshot 2024-08-09 at 9 40 25 AM" src="https://github.com/user-attachments/assets/e2d2f907-8ace-464a-8588-718a702ce363">
<img width="788" alt="Screenshot 2024-08-09 at 9 40 31 AM" src="https://github.com/user-attachments/assets/0fb89720-7237-45e9-8398-0b58522d0fb9">
<img width="800" alt="Screenshot 2024-08-09 at 9 40 39 AM" src="https://github.com/user-attachments/assets/b312bf87-fc3e-4942-81b4-90e16e329fb7">
<img width="826" alt="Screenshot 2024-08-09 at 9 40 51 AM" src="https://github.com/user-attachments/assets/99670d9c-7a3f-4b34-a313-ae82377c7463">
<img width="702" alt="Screenshot 2024-08-09 at 9 40 58 AM" src="https://github.com/user-attachments/assets/45a1e54c-004a-464f-9e10-0ace442b7a0a">
<img width="803" alt="Screenshot 2024-08-09 at 9 41 10 AM" src="https://github.com/user-attachments/assets/839be831-6ef9-42bd-860a-065a455fad94">
<img width="829" alt="Screenshot 2024-08-09 at 9 41 20 AM" src="https://github.com/user-attachments/assets/e4012694-f231-4d6d-badf-0b976e124cfb">
<img width="748" alt="Screenshot 2024-08-09 at 9 41 26 AM" src="https://github.com/user-attachments/assets/9134fb23-bf69-4b56-a0f1-728ab38b1512">
<img width="810" alt="Screenshot 2024-08-09 at 9 41 35 AM" src="https://github.com/user-attachments/assets/0fd1e4ac-0e27-45e6-9a35-07111c75d150">
<img width="812" alt="Screenshot 2024-08-09 at 9 41 43 AM" src="https://github.com/user-attachments/assets/bccf6dd0-6769-4248-888f-bab6adf17e7d">
<img width="828" alt="Screenshot 2024-08-09 at 9 41 54 AM" src="https://github.com/user-attachments/assets/0501f864-3342-486a-bd7b-e4bbb89306bc">
