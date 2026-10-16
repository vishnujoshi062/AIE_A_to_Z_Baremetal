# 07. AI Engine Simulation

## Purpose
Check the AI Engine graph **on its own**, before involving PL kernels or the PS. If the graph is wrong, it is much faster to find out here.

## Two simulation levels

| Level | Speed | Checks |
|---|---|---|
| **x86 simulation** | Very fast | Functional correctness of the C++ kernels and graph |
| **AIE simulator** | Slower | Cycle-approximate behaviour on the real placement, including DMA and locks |

## How it works
- Input data comes from `data/input.txt`, fed to the graph through the PLIO.
- The graph runs.
- Results are written to `data/output.txt`.
- The output is compared with a known-good (golden) file to confirm correctness.

## What the build produced
The AIE simulator / hardware build finished with a green tick and generated the compile summary and the Graph and Array views used in [doc 09](09_results_and_discussion.md). The simulator also provides Mapping, DMA and Lock reports because the compiler has already placed the graph.

## Steps
1. Select `simple_app` in the Flow panel.
2. Run **AIE SIMULATOR / HARDWARE > Build**.
3. Run the simulator.
4. Compare `output.txt` with the golden output.
5. Open **REPORTS** to inspect placement and resource use.
