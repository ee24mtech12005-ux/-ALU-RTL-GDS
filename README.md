# ALU — RTL to GDSII

A digital-design project documenting an Arithmetic Logic Unit (ALU) from RTL through physical implementation and GDSII stream-out, using Cadence Genus and Innovus.

> **Important:** This repository documents the timing results available in the supplied Innovus timing report. The RTL source, testbench, SDC, tool scripts, and complete design database were not included in the uploaded material, so this repository does not invent or claim to contain them. Add your actual project files in the folders provided.

## Project overview

An Arithmetic Logic Unit (ALU) is a combinational digital block that performs arithmetic and bitwise logic operations selected by control inputs. A typical ALU may support operations such as addition, subtraction, AND, OR, XOR, shifts, and comparisons. The exact operations, operand width, control encoding, and RTL behavior should be documented from the actual design source.

## RTL-to-GDSII flow

1. **RTL design** — describe the ALU in Verilog/SystemVerilog.
2. **Functional verification** — use a testbench and simulation to check each supported operation and corner cases.
3. **Timing constraints (SDC)** — specify clocks (if applicable), input/output delays, transitions, loads, uncertainty, and timing exceptions.
4. **Logic synthesis (Cadence Genus)** — map RTL into a standard-cell netlist and optimize for timing/area/power.
5. **Floorplanning and placement (Cadence Innovus)** — define the core, place cells, and prepare the design for routing.
6. **Clock-tree synthesis (CTS)** — build the clock network for sequential designs; applicability depends on the ALU architecture.
7. **Routing** — connect signal and clock nets.
8. **Parasitic extraction** — extract interconnect resistance and capacitance for realistic delay analysis.
9. **Post-route static timing analysis (STA)** — check setup and hold across the configured MMMC views/corners.
10. **Physical verification and GDSII stream-out** — review DRC/connectivity/antenna results and export GDSII.

## Timing results from the supplied report

The uploaded report records these post-route timing summaries:

| Stage / view | Check | WNS (ns) | TNS (ns) | Violating paths |
|---|---|---:|---:|---:|
| Post-route `timeDesign` | Setup, WorstCase | +0.990 | 0.000 | 0 |
| Post-route `timeDesign` | Reg-to-reg setup, WorstCase | +2.550 | Not stated | Not stated |
| Post-route `timeDesign` | Hold, BestCase (first summary) | +0.032 | 0.000 | 0 |
| Later SI-aware post-route | Hold, BestCase | +0.048 | 0.000 | 0 |

**Reported status:** setup and hold pass in the supplied summaries, with zero reported violating paths in the summaries above. The report also states zero max-capacitance, max-transition, max-fanout, and max-length violations in the setup timing summary.

### How to interpret the metrics

- **WNS (Worst Negative Slack):** the worst slack among checked paths. A non-negative value indicates that no checked path has negative slack in that timing view.
- **TNS (Total Negative Slack):** the sum of negative slack across violating paths. A value of zero is consistent with no negative-slack paths in that view.
- **Setup check:** verifies data arrives early enough before the capturing clock edge.
- **Hold check:** verifies data does not arrive too soon after the capturing clock edge.
- **SI-aware timing:** accounts for signal-integrity effects such as coupling/crosstalk in timing analysis.

### Physical-design notes / limitations

The supplied report notes:
- Post-route RC extraction, including a later extraction with 2,485 resistors, 2,439 ground capacitances, and 3,004 coupling capacitances.
- SI-aware post-route timing iterations and glitch analysis.
- 0.00% horizontal and vertical routing overflow.
- Antenna verification reported 0 violations.
- GDSII stream-out completed.
- A process-node extraction warning (`IMPEXT-3530`) and a physical implementation warning (`IMPSP-372`, mismatched FollowPin in rows).
- The report asks that a reported Metal1 spacing violation be verified. Connectivity warning messages mention unassigned I/O pins, although the final connectivity summary is reported as 0 violations and 0 warnings.

These caveats should be resolved or explained before describing the entire physical flow as fully clean. Timing PASS alone does not prove all DRC, LVS, connectivity, or signoff checks passed.

## Repository layout

```text
ALU-RTL-GDS/
├── README.md
├── .gitignore
├── rtl/              # Add actual ALU RTL source here
├── constraints/      # Add actual SDC constraints here
├── scripts/          # Add Genus/Innovus scripts and run instructions
├── results/          # Add simulation, synthesis, area, power, and signoff summaries
├── reports/
│   └── RTL_to_GDS_Timing_Report_ALU.docx
└── docs/
    └── TIMING_NOTES.md
```

## Reproducing / extending the project

1. Add the actual RTL and testbench to `rtl/`.
2. Add the real SDC and libraries/corner configuration references to `constraints/` (do not commit proprietary PDK/library files unless permitted).
3. Add the scripts used to run Genus and Innovus to `scripts/`.
4. Record tool versions, target process/node, clock assumptions, ALU width and supported operations.
5. Add the actual synthesis area/timing reports and final signoff reports to `results/`.
6. Confirm the final GDSII was generated and document DRC/LVS status based on actual reports.

## Timing commands referenced by the supplied report

Examples recorded in the report include:

```tcl
# Innovus timing checks
timeDesign -preCTS -setup
timeDesign -postCTS -setup
timeDesign -postCTS -hold
extractRC
timeDesign -postRoute -setup
timeDesign -postRoute -hold

# Inspect minimum-delay paths
report_timing -delay_type min -max_paths 10

# Timing optimization, if required
optDesign -postRoute -hold
optDesign -postRoute -setup
```

Use commands appropriate to your installed tool version and flow. These are documentation examples, not a complete runnable flow script.

## Source

Numerical timing results and physical-flow observations are transcribed from `reports/RTL_to_GDS_Timing_Report_ALU.docx`, supplied for this repository.
