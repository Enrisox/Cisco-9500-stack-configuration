# Cisco Catalyst 9500 StackWise Virtual Configuration

Lab notes and configuration snippets for building a redundant core stack with two Cisco Catalyst 9500 40x 10G switches


Step-by-step guide to building and managing a StackWise Virtual pair using two Cisco Catalyst 9500-40X switches. Covers firmware upgrade, SVL configuration, Dual-Active Detection (DAD), and stack verification.

[![StackWise Virtual](https://img.shields.io/badge/StackWise%20Virtual-Active-brightgreen?style=flat&logo=cisco)](tutorial.md)

## Features Covered

- Cisco IOS XE firmware upgrade via USB and stack-wide  
- StackWise Virtual domain configuration  
- SVL links on TenGigabitEthernet1/0/39-40 (10G SFP+ uplinks)  
- Dual-Active Detection (DAD) on TenGigabitEthernet1/0/38  
- Stack verification and troubleshooting  

## Hardware Requirements

- **2x Cisco Catalyst 9500-40X** switches (same model and IOS XE version)  
- **3x 10G SFP+ links** (2x SVL + 1x DAD): fiber SR/LR or DAC cables  
- **Network Advantage** license on both switches  

## Full Tutorial

See the complete step-by-step guide: [Tutorial](tutorial.md)
