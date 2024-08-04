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

