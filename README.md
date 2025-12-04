
# RISC-V Multicycle Core Integrated with AHB-Lite Bus

This project implements a **RISC-V RV32I multicycle CPU** integrated with an **AMBA AHB-Lite master interface** for both instruction and data memory operations. The design supports **R-type, I-type, Load, and Store instructions**, ensuring correct and cycle-accurate memory transactions through AHB-Lite single-transfer mode.

## 🚀 Features

### ✅ RV32I Multicycle Core

* Supports **R-type**, **I-type**, **Load**, and **Store** instructions
* Five-stage multicycle flow: **IF → ID → EX → MEM → WB**
* Deterministic timing and simplified control unit
* Clean separation of datapath and control logic

### ✅ AHB-Lite Master Integration

* Unified AHB-Lite bus for **instruction fetch** and **data memory**
* Correct handling of:

  * `HADDR`, `HTRANS`, `HWRITE`, `HWDATA`, `HRDATA`
  * `HREADY` wait states
* Fully supports **single transfer mode**
* Works seamlessly with multicycle timing because only one memory operation occurs at a time

### ✅ Master Glue Logic

* Bridges RISC-V memory signals (`if_en`, `mem_en`, `mem_read`, `mem_write`, `address`, `alu_out`) with AHB-Lite
* Waits on `HREADY` before completing fetch/load/store
* Ensures **no address overlap** between instruction and data phases
* Supports both **non-FSM** and **FSM-based** implementations

### ✅ Synthesizable RTL

* Written in clean SystemVerilog
* Modular structure for easy debugging and extension
* Supports simulation on standard tools (Vivado, ModelSim)

### ✅ Testbench Support

* AHB-Lite memory model
* Verification of instruction fetch, ALU ops, load/store behavior
* Waveform support for debugging timing and handshakes

---

## 🧠 Why AHB-Lite Works with a Multicycle Core

A multicycle CPU performs **exactly one memory operation at a time**.
AHB-Lite inherently supports **single, independent transfers** without requiring pipelining.
This makes the integration naturally compatible:

* Instruction fetch → AHB-Lite read transfer
* Load → AHB-Lite read transfer
* Store → AHB-Lite write transfer

If `HREADY = 0`, the CPU simply waits in **MEM stage**, allowing precise synchronization.

---



