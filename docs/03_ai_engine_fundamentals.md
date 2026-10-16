# 03. AI Engine Fundamentals

The AI Engine is a large topic. This page gives the minimum needed to understand the rest of the repo.

## What it is
The Versal AI Engine (AIE) is an **array of many small, identical processor tiles** built for high-throughput signal processing and machine learning. Work is split across tiles, and data flows between them directly, so nothing waits on a shared bus.

On the VC1902 (VCK190) the array has **400 tiles** (50 columns by 8 rows). This is why the compile summary reports usage as "x of 400".

## Inside one tile

- **VLIW vector processor (core)**: runs one or more kernels. It can issue scalar, vector and memory operations in the same cycle, and each vector operation processes many data lanes (SIMD).
- **Local data memory**: each tile has its own data memory banks and program memory. A core can also reach the memory of its neighbouring tiles, which is how data is handed from kernel to kernel without copying.
- **DMA and locks**: the tile DMA moves data between local memory and the stream network. Hardware locks synchronise producers and consumers.
- **Stream interconnect (switch)**: a network of AXI-stream links between tiles.

## The bottom row: shim tiles
Under the compute rows is a row of **shim tiles**. They connect the array to the rest of the chip:

- **PLIO**: AXI-stream ports to the Programmable Logic. This design uses PLIO.
- **GMIO**: memory-mapped ports straight to global memory through the NoC (not used here).

In the compile summary, "PLIO input 1 of 312, output 1 of 234" are the available shim ports in each direction.

## Where it sits in Versal

```
        +-----------+      NoC / AXI       +------------+
        |  PS (A72) | <------------------> |   DDR      |
        +-----------+                      +------------+
              | control (AXI4-Lite)              ^
              v                                  |
        +-----------+  AXI-Stream  +-------------+--+
        | PL kernels| <==========> | AI Engine array |
        | mm2s,s2mm |              |  (400 tiles)    |
        +-----------+              +-----------------+
```

## Key terms

| Term | Meaning |
|---|---|
| **Kernel** | A C++ function that runs on one AI Engine core |
| **Graph** | A set of kernels and their connections, written with the ADF library |
| **Port / PLIO** | Where data enters or leaves the graph |
| **Buffer (window)** | Block of data in tile memory passed between kernels |
| **Stream** | Continuous word-by-word data flow |
| **Lock** | Hardware flag that stops a kernel reading a buffer before it is filled |
| **Placement** | Which tile each kernel and buffer lands on (done by the compiler) |
| **Routing** | Which stream switches carry each connection |

## The programming model
You write three things, and Vitis ties them together:

1. **AIE graph**: kernels plus connections (C++).
2. **PL kernels**: HLS C++ for the data movers.
3. **PS application**: host code that starts the PL kernels and the graph and waits for completion.

The AI Engine compiler (`aiecompiler`) takes the graph and does placement, routing, memory allocation and lock assignment automatically. Its reports (see [doc 09](09_results_and_discussion.md)) show the decisions it made.
