# BioCore Tower — Development Journal

This journal tracks real progress, experiments, failures, and iterations.

## Stage 1: Energy Harvesting Schematic Completed! ⚡

### The Challenge
Harvesting energy from biological microbial fuel cells (MFCs) is extremely challenging because the output voltage is very low and unstable. I needed a robust power management circuit to step up this raw energy right at Stage 1 before storing or using it.

### Hardware Choices & The Diode Battle
I chose the **TPS63000** buck-boost converter from Texas Instruments because it can start up with very low voltages and regulate them to a stable rail. However, hooking up two separate MFC strings in parallel brought up a major isolation challenge:
- *First attempts:* I accidentally placed the isolation diodes in a way that either shorted the two strings together or forced the current through multiple diodes, doubling the forward voltage drop ($V_f$) which is catastrophic for low-power harvesting.
- *The Solution:* After iterating, I implemented a clean **Diode OR-ing** configuration using **BAT54J Schottky diodes**. Now, each MFC string is perfectly isolated. If one string drops in performance, the other keeps driving the buck-boost converter with minimal voltage loss (only one diode drop).

### KiCad Quirks (ERC vs PWR_FLAG)
While running the Electrical Rules Check (ERC), I hit a classic KiCad warning: `Input Power pin not driven by any Output Power pins` on the TPS63000 VIN and GND pins. Since connectors and diodes are passive components, KiCad didn't understand where the power source was. Fixed this quirk by placing two `PWR_FLAG` symbols on the VIN and GND nets. 0 Errors, 0 Warnings now!

### Next Steps: PCB Layout
Footprints are locked! I chose **0805 SMD** sizes for the passive components (resistors/capacitors) to balance high-frequency performance with easier hand-soldering. For the MFC cell connections, I'm using **Screw Terminals** for maximum stability. Time to open the PCB editor and start routing the switching traces!
