# ALU RTL-to-GDSII: timing notes

This note summarizes the timing-related findings from the supplied Innovus report. See `../reports/RTL_to_GDS_Timing_Report_ALU.docx` for the original report.

## Results

- Setup (WorstCase): WNS +0.990 ns, TNS 0.000 ns, 0 violating paths.
- Reg-to-reg setup (WorstCase): WNS +2.550 ns.
- Hold (BestCase), first summary: WNS +0.032 ns, TNS 0.000 ns, 0 violating paths.
- Hold (BestCase), later SI-aware summary: WNS +0.048 ns, TNS 0.000 ns, 0 violating paths.
- Setup summary reports zero max-capacitance, max-transition, max-fanout, and max-length violations.

## Extraction and SI analysis

The report records post-route RC extraction. A later extraction reports 2,485 resistors, 2,439 ground capacitances, and 3,004 coupling capacitances. The later timing calculation uses SPEF/RCDB parasitics and SI-aware timing. The report describes two SI iterations and glitch analysis.

## Physical checks and caveats

- Routing overflow: 0.00% horizontal and 0.00% vertical.
- Antenna verification: 0 violations.
- GDSII stream-out: completed.
- Review the `IMPEXT-3530` process-node warning and `IMPSP-372` FollowPin mismatch.
- Verify the reported Metal1 spacing violation.
- Connectivity warnings mention unassigned I/O pins, although the final connectivity summary says 0 violations and 0 warnings.

The report supports saying the cited setup and hold summaries pass. It does not by itself establish that every physical verification or foundry signoff check is clean.
