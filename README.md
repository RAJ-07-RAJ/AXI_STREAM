

# AXI-Stream Master–Slave RTL 

## Overview

This repository contains a **baseline AXI4-Stream Master and Slave RTL implementation** intended to demonstrate the fundamental **TVALID/TREADY handshake mechanism** and packet-based data transfer using `TLAST`.

The design currently focuses on **endpoint behavior (Master ↔ Slave)**.
It serves as a **starting point** for further protocol refinement and for integrating an AXI-Stream FIFO in later revisions.


## Project Scope (Current State)

**Included in this version:**

* AXI-Stream Master
* AXI-Stream Slave
* Direct Master-to-Slave connection
* Basic functional testbench


## AXI-Stream Signals Used

| Signal       | Direction      | Description                        |
| ------------ | -------------- | ---------------------------------- |
| `TDATA[7:0]` | Master → Slave | Streaming data                     |
| `TVALID`     | Master → Slave | Indicates valid data               |
| `TREADY`     | Slave → Master | Indicates readiness to accept data |
| `TLAST`      | Master → Slave | Indicates last beat of a packet    |



## Data Transfer Rule

Data transfer occurs when:
TVALID == 1 and TREADY == 1
This condition is monitored in the design and testbench to track valid transfers and packet boundaries.


## Module Description

### AXI-Stream Master (`axis_m`)

* Generates a fixed-length packet (4 data beats)
* Uses an FSM to control transmission
* Drives `TVALID`, `TDATA`, and `TLAST`
* Responds to `TREADY` from the slave
* Intended to model a simple AXI-Stream source

### AXI-Stream Slave (`axis_s`)

* Receives AXI-Stream data from the master
* Asserts `TREADY` to accept data
* Uses `TLAST` to detect end of packet
* Outputs received data for observation

---

## Top-Level Integration (`axis_top`)
The top module connects the AXI-Stream Master directly to the AXI-Stream Slave:
AXI-Stream Master → AXI-Stream Slave
This direct connection allows focused validation of basic handshake behavior before introducing buffering elements such as FIFOs.

## Verification

### Testbench (`tb_axis_top`)

The testbench:

* Generates clock and reset
* Initiates a packet transfer from the master
* Monitors `TVALID`, `TREADY`, and `TLAST`
* Checks correct packet length and end-of-packet signaling

The testbench validates **basic functional behavior**, not full protocol robustness.



## Tools & Language

* RTL: Verilog / SystemVerilog
* Simulation: Tool-agnostic (Icarus, Questa, etc.)



