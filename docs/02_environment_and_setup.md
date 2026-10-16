# 02. Environment and Setup

## Tools

| Item | Value |
|---|---|
| IDE | Vitis Unified IDE 2025.2 |
| AI Engine compiler | Vitis AI Engine Compiler Release 2025.2 |
| Target device | `xcvc1902-vsva2197-2MP-e-S` (VCK190) |
| System platform | `xilinx_vck190_base_202520_1` (sample platform) |
| PS processor | `standalone_psv_cortexa72_0` (bare-metal, no OS) |
| Host | Linux, workspace folder `aie_baremetal` |

## Workspace
The Vitis Explorer shows the workspace `AIE_BAREMETAL - VITIS COMPONENTS` with five entries plus the AI Engine component. See [image 00](../images/00_vitis_explorer_components.jpeg).

## Why a sample platform
A platform defines the hardware the design runs on (clocks, AXI interfaces, AI Engine connections, boot setup). The tutorial first builds one in Vivado (optional). The sample platform has the same ingredients ready, so the Vitis steps are identical from here on.

## Setup steps done
1. Create the Vitis workspace and select the sample platform.
2. Create the AI Engine component `simple_app`.
3. Create HLS components `mm2s` and `s2mm`.
4. Create the bare-metal application `AIE_A_to_Z_app` for the A72.
5. Create the system project `simple_aie_application_sp` on `xilinx_vck190_base_202520_1` and add the three components.
