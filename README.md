# Wideband Trans-Impedance Amplifier

A differential **wideband trans-impedance amplifier (TIA)** for optical receivers, designed in **45 nm CMOS** with Cadence Virtuoso/Spectre.

Final project for **ESE 5720 – Analog Integrated Circuits**, University of Pennsylvania, Fall 2024.
Author: **Kai Segimoto**

## Architecture

- **Common-gate input stage**: low input impedance that isolates the 100 fF photodiode capacitance
- **Differential amplifier**: single-ended to differential conversion
- **Cascaded common-source stages**: most of the gain; drain resistances kept low to protect bandwidth
- **Source-follower output buffers**: drive the 600 Ω differential load

The design started from hand calculations, which set the gain/bandwidth/noise trade-offs and rough device sizes. Values were then fine-tuned with coarse-to-fine parametric sweeps in Cadence.

## Results

| Parameter | Specification | Achieved |
|---|---|---|
| Supply | 1 V | 1 V |
| Trans-impedance gain (differential) | > 150 kΩ (103.5 dB) | **270.71 kΩ (108.65 dB)** |
| −3 dB upper corner | > 250 MHz | **500 MHz** |
| −3 dB lower corner | < 10 MHz | 5 MHz |
| Input-referred current noise (integrated over the −3 dB band) | < 125 nA rms | **104 nA rms** |
| Differential output swing (pk-pk) | > 0.4 V | 0.65 V |
| Total power | < 15 mW | **6.33 mW** |

The report also covers:

- process characterization: I<sub>D</sub>–V<sub>DS</sub>, V<sub>th</sub>, g<sub>m</sub> vs. I<sub>D</sub>, C<sub>gs</sub>/C<sub>gd</sub> vs. W
- the DC operating point
- AC, transient and noise simulations
- step-response stability
- hand calculations compared with simulation

## Files

- `ESE 5720 Final Project.docx`: full design report
- `ESE 572 data.docx`: Cadence captures of the final TIA schematic, the simulation results (trans-impedance gain, transient, AC, noise) and the integrated-noise summary

Download the files to view them, since GitHub does not render `.docx` files.
