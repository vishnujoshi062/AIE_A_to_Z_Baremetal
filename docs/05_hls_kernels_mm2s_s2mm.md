# 05. HLS Kernels: mm2s and s2mm

## Why they exist
The AI Engine only speaks **AXI4-Stream** on its PLIO ports. DDR memory is **memory-mapped**. Something in the Programmable Logic must convert between the two. That is the job of two small HLS kernels.

| Kernel | Direction | What it does |
|---|---|---|
| `mm2s` | DDR -> AI Engine | Reads words from memory and writes them to an AXI4-Stream |
| `s2mm` | AI Engine -> DDR | Reads an AXI4-Stream and writes the words to memory |

Both are controlled by the PS through an AXI4-Lite control interface (start, done, buffer address, size).

## How they were built
Each is an HLS component in Vitis Unified IDE:

1. **C synthesis**: converts the C++ into RTL (the `mm2s - C SYNTHESIS` and `s2mm::synthesis` tabs).
2. **Package**: produces an `.xo` kernel object (the `s2mm::package` task).
3. The `.xo` files are then linked into the system by `v++` ([doc 06](06_system_project_and_linking.md)).

In the **Build Components** dialog ([image 03](../images/03_build_components_dialog.jpeg)) `mm2s`, `s2mm` and `simple_app` are all ticked so that building the system project builds all three first.

## Source
Place the kernel sources in [`src/hls/`](../src/hls/): `mm2s.cpp` and `s2mm.cpp`.
