ARM32-BIT CPU — RTL Design (Verilog + SystemVerilog)
Project Overview

This repository contains a complete ARM32-bit CPU implemented using Verilog/SystemVerilog, verified on Vivado 2025.1 and EDAPlayground.
The design follows a simplified ARM architecture and implements:

32-bit datapath

Instruction fetch → decode → execute

ALU operations (ADD, SUB, AND, OR)

ARM conditional execution (N, Z, C, V)

Branch logic and PC update

Register file (R0–R15)

Separate IMEM & DMEM

The CPU successfully executes the reference program from Digital Design and Computer Architecture — ARM Edition (Harris & Harris).

Module Details
ALU (Arithmetic Logic Unit)

Operations: ADD, SUB, AND, OR

Outputs: 32-bit result + ARM flags (N, Z, C, V)

Status:  Verified (Vivado + EDAPlayground)

Datapath

PC logic (PC+4, PC+8)

MUX routing and control signal handling

Register file interface

ALU interface

Status:  Verified

Controller + Decoder

Generates all control signals:
RegWrite, ALUSrc, MemWrite, MemtoReg, PCSrc, FlagW

Status:  Verified

Instruction Memory (IMEM)

64 × 32-bit memory

Loads program using memfile.dat

Status:  Fully working in EDAPlayground

Note: Vivado requires memfile.dat in simulation directory

Data Memory (DMEM)

64 × 32-bit RAM

Controlled by MemWrite

Status:  Verified

CPU Top

Integrates:

Datapath

Controller

IMEM

DMEM

Status:  Fully operational in EDAPlayground

Compliance Checklist
Specification	Status	Notes
32-bit ALU	 Pass	ADD, SUB, AND, OR
ARM flags (N,Z,C,V)	 Pass	Correct updates
Register File	 Pass	R15 outputs PC+8
Datapath	 Pass	Fully functional
Instruction Fetch	 Pass	memfile.dat
Memory Operations	 Pass	DMEM verified
Full CPU Execution	 Pass	EDAPlayground
Vivado Compatibility	 Partial	memfile.dat path issue
Testbench Coverage	 Pass	Clock, reset, signals
Local Simulation Commands (ALL MODULES)

Below are clean commands for Icarus Verilog (iverilog) so users can simulate without Vivado.

1. ALU Simulation
cd ALU
iverilog -g2012 alu.sv alu_tb.sv -o alu_tb.out
vvp alu_tb.out
gtkwave alu.vcd

2. Datapath Simulation
cd DATAPATH
iverilog -g2012 datapath.sv alu.sv regfile.sv mux2.sv extender.sv flopr.sv flopenr.sv adder.sv datapath_tb.sv -o datapath_tb.out
vvp datapath_tb.out
gtkwave datapath.vcd

3. Controller + Decoder Simulation
cd CONTROLLER
iverilog -g2012 controller.sv decoder.sv condlogic.sv condcheck.sv flopenr.sv controller_tb.sv -o controller_tb.out
vvp controller_tb.out
gtkwave controller.vcd

4. IMEM Simulation
cd IMEM
iverilog -g2012 imem.sv imem_tb.sv -o imem_tb.out
vvp imem_tb.out
gtkwave imem.vcd

5. DMEM Simulation
cd DMEM
iverilog -g2012 dmem.sv dmem_tb.sv -o dmem_tb.out
vvp dmem_tb.out
gtkwave dmem.vcd

6. Full CPU Simulation (TOP + CPU)

This runs the complete ARM32-bit CPU exactly like EDAPlayground.

cd CPU
iverilog -g2012 top.sv cpu.sv datapath.sv controller.sv extend.sv alu.sv regfile.sv mux2.sv flopr.sv flopenr.sv imem.sv dmem.sv testbench.sv -o cpu_tb.out
vvp cpu_tb.out
gtkwave cpu.vcd


 This simulation shows:

PC updates

Instruction fetch

ALU operations

Register writes

Memory activity

Final results

EDAPlayground Simulation
Full CPU

 https://www.edaplayground.com/x/Hk54

Waveform Viewer

 https://www.edaplayground.com/launchEpwave

These show a successful full CPU execution with correct PC, ALU, IMEM, DMEM, and register activity.

Design & Verification Flow

RTL Design: Verilog + SystemVerilog

Simulation: Vivado + Icarus Verilog

Waveforms: Vivado Waveform Viewer + EPWave

Memory Initialization: memfile.dat

Testing:

ALU test (individual)

Register file test

Datapath test

Controller test

CPU full integration

Outcome

✔ ALU verified
✔ Datapath functional
✔ Controller working
✔ IMEM loads instructions
✔ DMEM performs writes
✔ CPU runs successfully on EDAPlayground
✔ Ready for GitHub & résumé

Acknowledgments

This project is inspired by Digital Design and Computer Architecture — ARM Edition (Harris & Harris).
Thanks to:

EDAPlayground for quick simulation

Vivado 2025.1 for RTL debugging

FPGA/VLSI open-source community

License

Released under the MIT License.
See LICENSE for details.
