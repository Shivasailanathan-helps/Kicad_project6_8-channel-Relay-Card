# Kicad_project6_8-channel-Relay-Card
Modular 8-channel power relay PCB designed for high-current AC/DC switching. Driven by a ULN2803A Darlington IC for 5V TTL/CMOS logic compatibility, it features THT relays, flyback diode protection, individual LED status indicators, DIP switch control, side screw terminals, and 4 aligned M3 mounting holes.

This is a professional 8-channel power relay interface board designed in KiCad for industrial automation and microcontroller load switching applications. Powered by an onboard ULN2803A Darlington transistor array IC, the module translates 5V TTL/CMOS logic signals into high-current switching for up to 8 independent AC/DC electrical loads. The 2-layer PCB features an ergonomic dual-column mechanical architecture, outward-facing Phoenix screw terminal breakouts, flyback diode inductive protection, channel status LEDs, a master power slide switch, 4 aligned M3 mounting holes, and a continuous bottom-layer ground plane.

### Features:
1) Microcontroller & Driver Architecture: ULN2803A Darlington Transistor Array (DIP-18) featuring 2.7kΩ internal base resistors, allowing direct 5V logic control from microcontrollers (Arduino, ESP32, STM32, or Raspberry Pi) without external current-limiting base drivers.
2) 8-Channel High-Power Relay Array: 8 x Omron G5LE-1 SPDT Through-Hole (THT) relays (K1–K8) capable of switching high-voltage AC (250V AC/10A) and DC (30V DC/ 10A) circuits.
3) Inductive Flyback & Visual Diagnostics: 8 x SMD flyback protection diodes (D2, D5, D7, D9, D14, D15, D16, D17, 1206) across relay coils to absorb inductive voltage spikes, paired with 8 x channel activation status LEDs (D3, D4, D6, D8, D10, D11, D12, D13, 1206) and 1 x main power LED (D1).
4) Input Signal Conditioning & Power Control: 16 x 10kΩ SMD resistors (R1–R16, 0805) forming a dedicated input pull-down network and series signal-limiting array, integrated with a 1-position SPST DIP slide switch (SW1) for master power isolation.
5) Robust Terminal Breakout Layout: 8 x 3-pin 5.08mm pitch Phoenix Contact screw terminal blocks (J3–J10) breaking out Normally Open (NO), Common (COM), and Normally Closed (NC) load lines toward the outer PCB edges, with a 2-pin 5.08mm Phoenix terminal (J2) for logic supply (VCC/GND) and an 8-pin 2.54mm pin header (J1) for logic inputs.
6) Enclosure-Ready Mechanical Layout: Symmetric dual-row relay column layout with central logic spine, smooth Edge.Cuts corner fillets, and 4 corner-aligned $3.2\text{mm}$ M3 mounting hole pads (H1, H3, H5, H6).

### Process:
Phase 1: Schematic Capture & Footprint Assignment:
1.1) Schematic & Netlist Setup: Captured the 8-channel relay sub-circuits, ULN2803A driver stage, input pull-down network, status LEDs, and power decoupling block (C1 100nF, C2 100µF). Established clear net connections (INPUT1–INPUT8, COM, VCC, GND) and passed Electrical Rules Check (ERC) with 0 errors and 0 warnings.
1.2) IPC-7351 Footprint Assignment: Assigned standardized footprints across all components (Package_DIP:DIP-18_W7.62mm_LongPads for U1, Relay_THT:Relay_SPDT_Omron-G5LE-1 for K1–K8, TerminalBlock_Phoenix_MKDS-1,5 for J2–J10, Resistor_SMD:R_0805_2012Metric, Diode_SMD:D_1206_3216Metric, and LED_SMD:LED_1206_3216Metric).

Phase 2: PCB Layout & Physical RoutingComponent Placement & Symmetry: 
2.1) Transferred schematic data to PCB Editor (F8). Positioned the 8 relays into two symmetrical 4-relay columns along the left and right edges, placing the ULN2803A IC, passive arrays, power switch, and status LEDs inside the central channel to minimize ratsnest clutter.
2.2) Edge Geometry & Alignment: Created the board perimeter on Edge.Cuts with 5mm corner radius fillets. Aligned the 4 corner mounting holes (H1, H3, H5, H6) to identical X/Y coordinate offsets to ensure strain-free mounting in standard enclosures.
2.3) Trace Routing & Zone Fills: Routed low-voltage signal paths, widened power and AC relay contacts, poured a continuous Bottom Copper (B.Cu) GND plane, and validated all clearances with Design Rules Check (DRC).

### Extras:
1) Why Use a ULN2803A Darlington Array for Relay Driving?

Microcontroller I/O pins typically source/sink a maximum of 10mA–20mA, whereas standard 5V electromechanical relay coils require 70mA–90mA each to energize. Activating all 8 relays simultaneously demands over 600mA—far exceeding direct MCU output capabilities. The ULN2803A integrates 8 NPN Darlington pairs capable of sinking up to 500mA per channel with internal 2.7k ohm series base resistors, allowing safe, direct control straight from logic lines without external base driving circuitry.

2) Why the ULN2803A (3rd Series) Variant Was Selected:

The ULN2800 family consists of five distinct input-stage variants (ULN2801A through ULN2805A), each tailored for specific input logic family voltage levels and gate structures:
2.1) ULN2801A: General-purpose Darlington array without internal base resistors. Using this variant would require adding 8 external base resistors on the PCB to limit microcontroller pin current.
2.2) ULN2802A: Designed specifically for 14V–25V PMOS logic; a standard 5V logic signal cannot provide enough voltage to drive this input stage.
2.3) ULN2803A (Selected): Features an internal 2.7k ohm series resistor per channel, specifically optimized for 5V TTL and 5V CMOS logic (such as ATmega328P, Arduino, 5V PICs, and standard logic gates). It draws roughly 1.35mA per input pin at 5V, perfectly saturating the Darlington pair while keeping MCU current minimal.
2.4) ULN2804A: Features a higher 10.5k ohm internal series resistor intended for 6V–15V CMOS logic. Driving this variant with 5V logic fails to supply enough base current to fully saturate the transistors under heavy relay coil loads.
2.5) ULN2805A: Designed for higher-voltage 6V–15V PMOS/CMOS logic applications.

3) Why Are Flyback Diodes (D2, D5, D7, D9, D14–D17) Required Across Relay Coils?
   
Relay coils are inductors (L). When a Darlington channel turns off to deactivate a relay, the sudden collapse of the coil's magnetic field creates a high-voltage reverse voltage spike (V = -L*di/dt) known as inductive kickback. The parallel flyback diodes provide a safe recirculation path for this energy, protecting the ULN2803A transistors from high-voltage breakdown.

### Problems Faced:

Zone Fill Out-of-Date Warnings during Gerber Generation:
Problem: KiCad issued a Zone fills are out-of-date. Refill? prompt during Gerber plot generation.
Theory: Component repositioning and trace modifications performed after initial copper zone creation alter pad clearance boundaries, leaving the ground plane out of sync.
Rectification: Executed a full copper zone recalculation (B) prior to plotting, ensuring all Gerber layers (F_Cu, B_Cu, F_Mask, B_Mask, Edge_Cuts, PTH/NPTH) reflected accurate clearance gaps and copper pours.

### Outputs:

<img width="1125" height="781" alt="image" src="https://github.com/user-attachments/assets/3c67465b-711f-4314-8ef1-f33bd644950a" />

<img width="1600" height="680" alt="image" src="https://github.com/user-attachments/assets/98479aa9-2806-49aa-82a1-1fe4db9e6dfe" />

<img width="1600" height="801" alt="image" src="https://github.com/user-attachments/assets/1ec2609d-fa93-4aa2-97f1-3b6ca5f1b0b8" />

<img width="1600" height="840" alt="Relay Board (frnt img)" src="https://github.com/user-attachments/assets/fa6b89c0-a527-4c00-a62e-2a05de5deb63" />

<img width="1600" height="840" alt="Relay Board (back img)" src="https://github.com/user-attachments/assets/e7f7edbf-0395-494d-87af-4ff47ead0305" />




