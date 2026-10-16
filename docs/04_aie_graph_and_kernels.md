# 04. The AI Engine Graph and Kernels

## What we built
The AI Engine component `simple_app` holds a graph named `mygraph` with two kernels, **`first`** and **`second`**, each with one input (`in`) and one output (`out`). Data enters the graph through one PLIO input (`mygraph.in`, file `data/input.txt`) and leaves through one PLIO output (`mygraph.out`, file `data/output.txt`).

You can see this in the Array view: [image 08](../images/08_array_view_kernels_tile_24_0.jpeg) shows `first` and `second` as boxes with `in`/`out` ports, and [image 10](../images/10_array_view_shim_plio_ports.jpeg) shows the two PLIO ports labelled `Input: mygraph_in (data/input.txt)` and `Output: mygraph_out (data/output.txt)`.

## Why a graph
The graph describes *what* talks to *what*. The compiler decides *where* things run. This keeps the algorithm separate from the hardware mapping.

## Build targets
| Target | Purpose |
|---|---|
| **x86 simulation** | Fast functional check on the host PC. No timing, no placement. |
| **AIE simulator / hardware** | Compiles for the real array (placement, routing, locks). Used for cycle-approximate simulation and for the hardware flow. |

In the Flow panel, **AIE SIMULATOR / HARDWARE > Build** has a green tick ([image 04](../images/04_aie_compile_summary.jpeg)), meaning the graph compiled for the real array.

## What the compiler produced
The build output folder (`build/hw/Work`) contains, among others:

| File | What it is |
|---|---|
| `project.aiecompile_summary` | Opens the Summary, Graph and Array views |
| `aie_hw.cfg`, `Work.spr` | Compiled configuration for the array |
| `AIECompiler.log`, `aiemetacpp.log` | Compiler logs |
| `baremetal_metadata_compile.cpp` | Generated bare-metal metadata |
| `report_heap.txt`, `report_stack.txt`, `report_pm.txt` | Memory use per kernel (heap, stack, program memory) |
| `Stream_report.txt`, `tilemap_per_traceport.txt` | Stream and trace information |
| `sync_buffer_address.json` | Buffer addresses and synchronisation |
| `v++_compile_Work_guidance.html` | Guidance messages |

## Summary of the graph run
- 1 core used for both kernels, 2 buffers, 2 stream-switch tiles.
- 1 PLIO in, 1 PLIO out, 2 interface channels in total.

Interpretation of the numbers is in [doc 09](09_results_and_discussion.md).
