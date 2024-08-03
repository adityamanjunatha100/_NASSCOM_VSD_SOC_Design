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
