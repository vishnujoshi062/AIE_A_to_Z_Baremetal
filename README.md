# AIE A to Z: Bare-metal Flow in Vitis Unified IDE 2025.2 (VCK190)

My walkthrough of the AMD **AI Engine A to Z** tutorial: a Versal AI Engine graph, two HLS data-mover kernels, a bare-metal A72 application, built and run in **hardware emulation**. This repo explains what the AI Engine is, what I built, why each step exists, and how to read the reports.

Official tutorial: [Xilinx/Vitis-Tutorials, 2025.2, 01-aie_a_to_z](https://github.com/Xilinx/Vitis-Tutorials/tree/2025.2/AI_Engine_Development/AIE/Feature_Tutorials/01-aie_a_to_z)

## What I skipped
The tutorial starts with an optional Vivado step that builds a custom platform. I skipped it and started directly in Vitis with the sample platform `xilinx_vck190_base_202520_1`.

## System at a glance

```
PS (Cortex-A72, bare-metal)
   |  start / configure (AXI4-Lite)
   v
 mm2s (HLS, PL)  --AXI4-Stream-->  AI Engine graph "simple_app"  --AXI4-Stream-->  s2mm (HLS, PL)
 reads DDR                         kernel first -> kernel second                     writes DDR
                                   (input.txt in, output.txt out in simulation)
```

## Components (see [image 00](images/00_vitis_explorer_components.jpeg))

| Component | Type | Role |
|---|---|---|
| `AIE_A_to_Z` | Platform | Hardware platform for VCK190 |
| `AIE_A_to_Z_app` | Application (`standalone_psv_cortexa72_0`) | Bare-metal code on the A72 that drives the system |
| `mm2s` | HLS kernel | Memory-mapped to stream: feeds data into the AI Engine |
| `s2mm` | HLS kernel | Stream to memory-mapped: collects results from the AI Engine |
| `simple_app` | AI Engine | The AI Engine graph and kernels |
| `simple_aie_application_sp` | System project | Links everything, packages the boot image |

## Documentation

| Doc | Topic |
|---|---|
| [01](docs/01_overview.md) | Overview of the project and the flow |
| [02](docs/02_environment_and_setup.md) | Environment and setup |
| [03](docs/03_ai_engine_fundamentals.md) | **AI Engine fundamentals** (read this first if the topic is new) |
| [04](docs/04_aie_graph_and_kernels.md) | The AI Engine graph and kernels |
| [05](docs/05_hls_kernels_mm2s_s2mm.md) | HLS kernels `mm2s` and `s2mm` |
| [06](docs/06_system_project_and_linking.md) | System project, linking and packaging |
| [07](docs/07_aie_simulation.md) | AI Engine simulation |
| [08](docs/08_hw_emulation.md) | Hardware emulation |
| [09](docs/09_results_and_discussion.md) | **Results and discussion**: how to read every report |
| [10](docs/10_troubleshooting.md) | Troubleshooting |
| [11](docs/11_references.md) | References |

## Key results

- AI Engine compile completed for `xcvc1902-vsva2197-2MP-e-S` at 1250 MHz.
- The graph uses **2 of 400 tiles (0.50 %)**: 1 tile for the kernels, 2 for buffers.
- 1 PLIO input and 1 PLIO output, 0 GMIO.
- Hardware emulation booted the Versal boot image (PLM, PDI load, all partitions) and the bare-metal app ran.

Details and interpretation: [docs/09_results_and_discussion.md](docs/09_results_and_discussion.md).

## Repo layout

```
README.md
docs/     explanations (01 to 11)
images/   screenshots 00 to 10
src/hls/      mm2s.cpp, s2mm.cpp
src/ps_app/   bare-metal A72 application
reports/      exported report summaries (no .csv)
```

## Reference
Official AMD tutorial: <https://github.com/Xilinx/Vitis-Tutorials/tree/2025.2/AI_Engine_Development/AIE/Feature_Tutorials/01-aie_a_to_z>
