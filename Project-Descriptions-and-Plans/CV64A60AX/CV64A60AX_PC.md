<!--
 *  Copyright 2025 CEA*
 *  *Commissariat a l'Energie Atomique et aux Energies Alternatives (CEA)
 *
 *  SPDX-License-Identifier: Apache-2.0 WITH SHL-2.1
 *
 *  Licensed under the Solderpad Hardware License v 2.1 (the “License”); you
 *  may not use this file except in compliance with the License, or, at your
 *  option, the Apache License version 2.0. You may obtain a copy of the
 *  License at
 *
 *  https://solderpad.org/licenses/SHL-2.1/
 *
 *  Unless required by applicable law or agreed to in writing, any work
 *  distributed under the License is distributed on an “AS IS” BASIS, WITHOUT
 *  WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
 *  License for the specific language governing permissions and limitations
 *  under the License.
-->

# CV64A60AX - Project Concept Proposal

**Date of proposal**

2025-01-TBD


**Author(s)**

Tanuj Khandelwal - Research Engineer - CEA List (Grenoble)


## High Level Summary of project, project components, and deliverables

### Summary
Many applications in the HPC domain require a 64 bit processor capable of fast memory transfer and floating point calculation. 
CVA6 is one such processor. CVA6 is derived from ARIANE core developed by PULP team of ETH. It is highly configurable and different configuration can be derived from the same RTL source. 
Thales currently derives the design and verification effort of CVA6. And their effort currently concetrate at a 32 bit core. 
We propose to extend the existing verification environnemnt of CV32A65 to CV64A60AX

The most important features of the proposed 64-bit core are:

  - Atomic Support: The CPU system support atomic accesses in memory. Those accesses are handled by the HPDCache as Non-Cachable access and forwarded to memory. 

  - CACHE MANAGEMENT OPERATION SUPPORT: The support of Cache-Management Operation (or CMO) instructions allows the software to perform operations on copies of data in the memory hierarchy and in particular on cached copies of data 

  - HIGH PERFORMANCE DATA CACHE - HPDCACHE: The HPDcache is the responsible for serving data accesses issued by a RISC-V core, tightlycoupled accelerators (when implemented) and hardware memory prefetchers. 32KB Data Cache 
  - INSTRUCTION CACHE: CPU subsystem implements a 32KB Level 1 Instruction cache

  - MMU: The Memory Management Unit (MMU) module is a crucial component in the RISC-V-based processor, serving as the backbone for virtual memory management and address translation

  - EXTENSION PORT: The CORE-V eXtention Interface is a RISC-V extension interface that provides a generalized framework suitable to implement custom coprocessors and ISA extensions for existing RISCV processors

  - ISA extensions: Support of G, C, Zicount and Zb extensions 

  - FPU: Single and Double precision Floating point support
  
  - Support 3 privilege levels: Machine, Superviser (eq. OS), User


### Components

The main goal of the project is to bring CV64A60AX at TRL-4 with following deliverables. 

### Deliverables

- Design Deliverables:
  - Updated CVA6 RTL to support the CV64A60AX configuration
  - CV64A60AX User Manual
  - RTL bug fixes
- Verification Deliverables:
  - UVM Verification Environment for the CV64A60AX configuration, including:
    - Functional coverage model
    - Spike-based reference model
  - CV32A65X-compatible CI regression
  - Complete DV plans for the CV64A60AX
  - Verification Documentation
  - Regression test-suite to demonstrate TRL-4

## Summary of market or input requirements


### Known market/project requirements at PC gate
  - 64 bit RISCV based open source processor
  - With FPU and MMU enabled


### Potential future enhancements
Increasing verification maturity


## Who would make use of OpenHW output

It will benefit anyone with following requirements: 
- System Integrators looking for an "Industrial Grade" 64 bit RISCV core
- IP Deverlopers interested in developing advanced Application-class cores

## Initial Estimate of Timeline

The TRL-4 release is expected at the end of 2026

## Explanation of why OpenHW should do this project

  - First move for OpenHW into 64 bit core.
  - It compliments the verification of CV32A65X 
  - Deepen engagement of key EU research organizations with OpenHW projects

## Industry landscape: description of competing, alternative, or related efforts in the industry
A comparable study was done for CODASIP A70 (not an open source project) which has following features: 
 - 64 bit 
 - FPU 
 - Single Issue 
 - AXI-4 64-bit interface

## OpenHW Members/Participants committed to participate
10xEngineers
ETH eXpect 
Thales 

## Principal responsible of verification
Tanuj Khandelwal (CEA List, Grenoble)

Support of Thales is expected in some way.
Additional participation for specification and verification plan
reviewing welcomed.

## Project Leader(s)

Mastromauro Florian (CEA List, Grenoble)


### Technical Project Leader(s)

Philippe Anthony (CEA List, Grenoble)

### Project Manager, if a PM is designated
Mastromauro Florian (CEA List, Grenoble)


## Next steps/Investigation towards Project Launch (**PC only**)

  - Review of specifications from interested parties and the Cores Task
    Group.

  - Review existing verification plan. Discuss approaches for verification with
    interested parties and the verification task group. Determine a
    verification phase approach.

  - Meeting with other OpenHWG members/staff to discuss about repository
    structure, simulation tools, CI, compilation flow and project board.

### Repository Requirements


