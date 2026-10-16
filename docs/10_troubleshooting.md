# 10. Troubleshooting

| Symptom | Likely reason | What to do |
|---|---|---|
| **HARDWARE > Build All** shows a red cross while **HARDWARE EMULATION** works | The full hardware build runs complete Vivado implementation and is much more demanding than emulation. It may fail on environment, resources or timing. | Open *Link - binary_container_1 > Log* and *Timing Summary* under HARDWARE to find the real error. Emulation does not need this build. |
| Problems panel shows 1 error, 2 warnings after a clean | Leftover state from a failed target | Clean, rebuild the target you need, and read the first error in the Output panel |
| `nic ethernet@0xff0d0000 has no peer` in the emulator log | No network backend attached to QEMU | Ignore; this design does not use Ethernet |
| Build starts but a component is missing | Component not ticked in **Build Components** | Tick `mm2s`, `s2mm` and `simple_app` |
| Application does not start after boot | ELF not added in `package.cfg` | Add `AIE_A_to_Z_app.elf` as the bare-metal ELF for the A72 and repackage |
| Stale results after changes | Old build artefacts | Use Clean for the target, then Build All |

General tip: always read the **first** error in the log, not the last.
