# Wideband Trans-Impedance Amplifier Design

ESE 5720 Analog Integrated Circuits · Kai Segimoto · Fall 2024

> Markdown version of [`ESE 5720 Final Project.docx`](ESE%205720%20Final%20Project.docx) for reading on GitHub. The original Word file is in this repository.

**Contents**

- [I. Introduction and Literature Search](#i-introduction-and-literature-search)
  - [Common TIA topologies](#common-tia-topologies)
  - [Feedback TIA](#feedback-tia)
  - [Common Gate TIA](#common-gate-tia)
  - [Conventional Regulated Cascode](#conventional-regulated-cascode)
  - [Design](#design)
- [II. Process Characterization](#ii-process-characterization)
  - [ID vs. VDS for 0<VDS<1V](#id-vs-vds-for-0vds1v)
  - [Vth vs. ID for 0<ID<20mA](#vth-vs-id-for-0id20ma)
  - [gm vs. ID for 0<ID<20mA](#gm-vs-id-for-0id20ma)
  - [Cgs and Cgd vs. W for 0.4m<W<10m](#cgs-and-cgd-vs-w-for-04mw10m)
  - [Hand calculations](#hand-calculations)
- [III. Design Strategy](#iii-design-strategy)
- [IV. Hand Calculations](#iv-hand-calculations)
- [V. Cadence Simulation Results](#v-cadence-simulation-results)
  - [Circuit Schematics](#circuit-schematics)
- [VI. Comparison of hand calculations and simulations](#vi-comparison-of-hand-calculations-and-simulations)
- [VII. Conclusion](#vii-conclusion)
- [VIII. References](#viii-references)

## I. Introduction and Literature Search

Transimpedance amplifiers (TIAs) are essential components used in circuits for converting current signals into voltage signals (hence the name, trans-impedance). They convert small signal changes in a current and convert and amplify it into a corresponding voltage signal, making them accurate current detectors. This property allows them to be widely used in applications such as photodetectors, current-output sensors, and communication systems where precise and low-noise signal conversion is critical such as fiber optic cables. Applications for TIAs extend into anywhere where precise detection of small currents is required such as biomedical devices, light detecting and ranging (LIDAR) systems, or any sensor interface.

### Common TIA topologies

The topology of the TIA plays a fundamental role in the performance of the TIA. Several different topologies are used depending on the target application such as low noise, high gain-bandwidth product, lower power consumption, or a wideband design. The large parasitic capacitance of the input photodiode often limits the bandwidth of the TIA since it forms the dominant pole at the input,

<img src="report_images/image33.png" width="194" alt="Figure" />

Due to this, the input of the TIA must be designed to be low impedance to get a larger bandwidth. A few low input impedance TIA topologies are presented with design tradeoffs.

### Feedback TIA

<img src="report_images/image24.png" width="287" alt="Figure" />

The Feedback TIA is a commonly accepted TIA design for the low input impedance and low output impedance as desired for a voltage amplifier. The transimpedance gain is given by Rt, which is approximately R<sub>F</sub> for sufficiently large gain.

<img src="report_images/image22.png" width="180" alt="Figure" />

The 3 dB bandwidth then becomes the following:

<img src="report_images/image63.png" width="235" alt="Figure" /><img src="report_images/image15.png" width="137" alt="Figure" />

Due to noise considerations, the amplifier is typically chosen to be a simple design with few components, and the noise for the feedback TIA is given by:

<img src="report_images/image45.png" width="553" alt="Figure" />

From this we can clearly see that in order to achieve a low noise by increasing the R<sub>F</sub>, there are sacrifices in bandwidth. The TIA also suffers from stability issues with large parasitic C<sub>D</sub> values due to limiting bandwidth and decreasing the phase margin.

### Common Gate TIA

<img src="report_images/image27.png" width="316" alt="Figure" />

The common gate is another common TIA topology with a low input impedance of R<sub>in</sub>=1/g<sub>m1</sub>. The 3 dB bandwidth then becomes the following, given the output pole is not dominant:

<img src="report_images/image49.png" width="317" alt="Figure" />

Because all the signal current flows through R1, the transimpedance is found to be R<sub>T</sub> = R1. This makes the input impedance and the transimpedance decoupled, allowing for them to be individually adjusted. With all the current flowing through one branch, there is relatively low power consumption and no stability issues with feedback, and decreasing the effect of input capacitance. This design has a relatively high noise as R<sub>D</sub> and M2 contribute directly to the input referred noise:

<img src="report_images/image43.png" width="401" alt="Figure" />

This design is good for high bandwidth, and with additional gain stages after can achieve high gain. Due to the large amount of input referred noise, it may be unsuitable for low-noise applications.

### Conventional Regulated Cascode

<img src="report_images/image57.png" width="233" alt="Figure" /><img src="report_images/image26.png" width="250" alt="Figure" />

The Conventional Regulated Cascode (RGC) TIA is a common-gate TIA with active feedback often in the form of a CS amplifier. This topology reduces the input resistance by an additional factor of (1+g<sub>mB</sub>R<sub>B</sub>) or (1+A) where g<sub>mB</sub>R<sub>B</sub> is the gain of the amplifier.

<img src="report_images/image39.png" width="220" alt="Figure" />

This topology increases the transconductance of M1, which makes the mid-band transimpedance gain R<sub>T</sub> = R<sub>D</sub>. However, due to the parasitic capacitances such as the gate-drain capacitance across M<sub>B</sub>, the miller effect makes the capacitance look larger which increases the input capacitance. This may propose a bandwidth limitation despite decreased input impedance which pushes the limiting input pole to higher frequencies.

### Design

For this project, a CG TIA will be implemented due to the simple design with minimal complexity relative to the other designs. In addition, due to the reduced effects of input capacitance and the rubric not requiring low noise operation, we do not need to overly consider and design for the least amount of noise, making the CG TIA a good choice for the project.

Due to the differential output requirement, the design takes the single ended output from the CG TIA and creates a differential output using a differential pair, then if the output swing is not great enough it will be amplified through another stage, then finally an output buffer stage will drive a 600 ohm differential load.

## II. Process Characterization

### ID vs. VDS for 0\<VDS\<1V

The gate voltage is set to 0.5 V for PMOS and NMOS plots, and the drain voltage is swept from 0 to 1V. A parametric analysis is performed on the width to find the curves for each width.

**NMOS**

Test schematic

<img src="report_images/image52.png" width="425" alt="Output Plots" />

Output Plots

<img src="report_images/image25.png" width="830" alt="PMOS" />

**PMOS**

Schematic

<img src="report_images/image36.png" width="423" alt="Output Plots" />

Output Plots

<img src="report_images/image12.png" width="830" alt="Figure" />

### Vth vs. ID for 0\<ID\<20mA

**NMOS**

Schematic

<img src="report_images/image29.png" width="455" alt="Output Plot" />

Output Plot

<img src="report_images/image6.png" width="830" alt="PMOS" />

**PMOS**

Schematic

<img src="report_images/image5.png" width="456" alt="Output Plot" />

Output Plot

<img src="report_images/image16.png" width="830" alt="Figure" />

### gm vs. ID for 0\<ID\<20mA

We use the same schematics as before but use gm instead of vth as the parameter to plot in the function

**NMOS**

schematic

<img src="report_images/image29.png" width="498" alt="Output plot" />

Output plot

<img src="report_images/image32.png" width="673" alt="PMOS" />

**PMOS**

Schematic

<img src="report_images/image5.png" width="337" alt="Output plot" />

Output plot

<img src="report_images/image20.png" width="721" alt="Figure" />

### Cgs and Cgd vs. W for 0.4m\<W\<10m

**NMOS**

Schematic

<img src="report_images/image28.png" width="471" alt="Output Plot" />

Output Plot

**PMOS**

Schematic

<img src="report_images/image14.png" width="507" alt="Output Plot" />

Output Plot

<img src="report_images/image56.png" width="830" alt="What is the process fT? Does it change with drain current?" />

What is the process f<sub>T</sub>? Does it change with drain current?

The process f<sub>T</sub> is the unity gain cutoff frequency, which is the frequency at which the gain of the transistor is unity, or 1. The f<sub>T</sub> of a transistor can be approiximated as

<img src="report_images/image18.png" width="207" alt="Figure" />

<img src="report_images/image9.png" width="226" alt="Figure" />

We also know that the transconductance of a transistor is the partial derivative of the drain current with respect to the gate voltage:

<img src="report_images/image58.png" width="326" alt="Figure" />

Thus, we know that the process f<sub>T</sub> will change with drain current.

However, from the lecture

<img src="report_images/image3.png" width="689" alt="Figure" />

And since Veff is directly proportional to the drain current (changing Id changes Veff), f<sub>T</sub> changes with drain current.

### Hand calculations

We have the experimental parameters found from homework 3:

<img src="report_images/image40.png" width="830" alt="Figure" />

<img src="report_images/image64.jpg" width="830" alt="Figure" /><img src="report_images/image54_crop1.jpg" width="830" alt="Figure" />

## III. Design Strategy

The overall design strategy in this project was to first be able to achieve the required bandwidth while maintaining good gain in each stage, then if the gain at the output is not high enough additional CS gain stages can be used to amplify our output into the desired result. To do this, we design each stage with both bandwidth and gain in mind. Because the allowed energy is very high and the noise can be achieved with relative ease, as long as the first stage noise is low, these parameters can be ignored in initial design and bandwidth and gain. The design flow is as follows. As outlined above, the design strategy for our first stage is to implement a CG topology for the TIA to convert our current input into a single ended voltage output. We implement the CG topology, making sure to keep the drain resistance small because in order to achieve high bandwidth, the pole at the output should be kept as large as possible. Recall that the pole frequency is inversely proportional to the capacitance and the resistance seen at the node, so the resistance should be as small as possible. For this reason, a smaller decoupling capacitor is also used, below the maximum 50pF size because even though the signal integrity is not as great. However, we cannot make the drain resistor too small because it would cause the noise too large, as well as make the gain so small that too many amplification stages at the output are required.

The second stage used is a differential amplifier, which takes the output of the single ended CG TIA and converts it into a differential ended signal. To do this, the output is tied directly into one of the inputs of the differential amplifier, and the other end is grounded. The gain for a differential amplifier is approximately g<sub>m</sub>R<sub>D</sub>, so we want to maximize this value. However, to maintain high bandwidth the drain resistance cannot be too large. Thus, to achieve appropriate gain while maintaining bandwidth, a resistance smaller than that of the CG amplifier should be used (in order to maintain the dominant pole), and the width of the transistor is increased to increase the transconductance. Additionally, the current mirror should be configured to drive a current through the transistors that achieve the transconductance wanted. We want the output impedance of the current mirror to be high in order to achieve a stable current that does not change much with the input current changing. This is wanted to have a stable output.

After the differential output stage, the signal is decoupled as always with small capacitors and then goes through an amplification stage using CS amplifiers. Similar to before, the drain resistance is kept low in order to not limit the bandwidth, and the width is increased to increase the transconductance and get high gain from each stage. Finally, a source follower is used to buffer the output, providing high input impedance but lower output impedance allows for small loads such as the one given to be driven with minimal voltage drop.

These initial calculations and outlines are written in more detail in the hand calculations section, however due to higher order effects hand calculations can only provide a rough approximation of behavior of the circuit and cannot be used to give concrete values. Thus, in the hand calculation section equations are used to show relationships between gain, bandwidth, and noise in order to provide guidance on the magnitude of values to be chosen. For the fine tuning of values to find maximum bandwidth and gain, large parametric sweeps around rough magnitudes and values, then to find specific numbers for best results, finer parametric analysis is used.

## IV. Hand Calculations

<img src="report_images/image53_crop1.jpg" width="830" alt="Figure" /><img src="report_images/image66.jpg" width="830" alt="Figure" /><img src="report_images/image65.jpg" width="830" alt="Figure" />

## V. Cadence Simulation Results

### Circuit Schematics

<img src="report_images/image59.png" width="830" alt="Stage 1: Common Gate TIA" />

Stage 1: Common Gate TIA

<img src="report_images/image42.png" width="830" alt="Stage 2: Differential Amplifier" />

Stage 2: Differential Amplifier

<img src="report_images/image34.png" width="830" alt="Stage 3: CS amplifiers" />

Stage 3: CS amplifiers

<img src="report_images/image44.png" width="591" alt="Figure" />

<img src="report_images/image23.png" width="594" alt="Stage 4: buffer" />

Stage 4: buffer

<img src="report_images/image46.png" width="338" alt="Figure" /><img src="report_images/image13.png" width="339" alt="Figure" />

#### DC operating point summary table

Note: additional screenshots of operating points shown at bottom of document for region of operation

<img src="report_images/image19.png" width="830" alt="Figure" />

<img src="report_images/image31.png" width="830" alt="Figure" />

<img src="report_images/image38.png" width="830" alt="Voltage source DC operating point" />

**Voltage source DC operating point**

<img src="report_images/image50.png" width="830" alt="Voltage source DC operating point" />

As seen there is **6.33 mW of power consumption**

1.  AC simulation clearly showing the TIA transfer function (1kHz to 10GHz)

<img src="report_images/image2.png" width="830" alt="As seen, our max transimpedance gain is roughly 108.65 dB" />

As seen, our **max transimpedance gain is roughly 108.65 dB**

**The -3dB upper corner frequency is 500MHz**

**The -3dB lower corner frequency is 5MHz**

#### Transient simulation clearly showing the differential output waveform and differential peak-to-peak output voltage (use 100MHz input sinusoidal current with 2.67App amplitude and show 8 to 10 periods of the differential output voltage)

In the Cg amplifier schematic image it shows 2.67 uA pk-pk amplitude or 1.335 uA amplitude. The following transient simulation is found:

<img src="report_images/image61.png" width="830" alt="Figure" />

Zooming into 8-10 periods:

<img src="report_images/image17.png" width="830" alt="We get the following differential input" />

We get the following differential input

<img src="report_images/image35.png" width="830" alt="Figure" />

Subtracting the two waveforms, we get a peak to peak differential output amplitude of **650 mV pk-pk or .65V differential**.

#### Noise simulation (simulate the input referred current noise and integrate it over the -3dB bandwidth)

**input referred noise spectrum**

<img src="report_images/image51.png" width="830" alt="input referred noise spectrum" />

Note at low frequencies the noise is high due to the flicker noise from the source transistor in the CG amplifier.

**Integrating from the lower corner to upper corner -3dB corner frequencies**

<img src="report_images/image7.png" width="830" alt="Integrating from the lower corner to upper corner -3dB corner frequencies" />

The total input referred current noise integrated over the -3dB bandwidth is **104 nA<sub>RMS</sub>**

#### Evidence of stable operation (run transient analysis to find the step response and investigate the stability of the TIA)

**Transient analysis of step response**

<img src="report_images/image67.png" width="733" alt="Transient analysis of step response" />

As seen the amplifier is stable

#### Table of comparison

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Requirement</th>
<th>Achieved performance</th>
</tr>
<tr class="odd">
<th>Technology Node</th>
<th>45 nm</th>
<th>45 nm</th>
</tr>
<tr class="header">
<th>V<sub>dd</sub></th>
<th>1V</th>
<th>1V</th>
</tr>
<tr class="odd">
<th>C<sub>P</sub></th>
<th>100fF</th>
<th>100fF</th>
</tr>
<tr class="header">
<th>Load Resistance</th>
<th>600 ohm (differential)</th>
<th>600 ohm (differential)</th>
</tr>
<tr class="odd">
<th>Input current amplitude</th>
<th>2.67A peak-to-peak</th>
<th>2.67A peak-to-peak</th>
</tr>
<tr class="header">
<th>Output pk-pk voltage swing</th>
<th>&gt;0.4V (differential)</th>
<th><strong>.65 V</strong></th>
</tr>
<tr class="odd">
<th>Trans-impedance gain</th>
<th><p>&gt;150k (differential)</p>
<p>&gt;103.5 dB</p></th>
<th><p><strong>270.71 k</strong></p>
<p><strong>108.65 dB</strong></p></th>
</tr>
<tr class="header">
<th>-3dB upper corner frequency</th>
<th>&gt;250MHz</th>
<th><strong>500 MHz</strong></th>
</tr>
<tr class="odd">
<th>-3dB lower corner frequency</th>
<th>&lt;10MHz</th>
<th><strong>5 MHz</strong></th>
</tr>
<tr class="header">
<th>Total input referred current noise</th>
<th>&lt;125nA<sub>rms</sub></th>
<th><strong>104 nA<sub>RMS</sub></strong></th>
</tr>
<tr class="odd">
<th>Largest capacitor available</th>
<th>50pF</th>
<th><strong>10 pF</strong></th>
</tr>
<tr class="header">
<th>Smallest capacitor available</th>
<th>10fF</th>
<th><strong>10 pF</strong></th>
</tr>
<tr class="odd">
<th>Largest resistor available</th>
<th>250k ohm</th>
<th><strong>250k ohm</strong></th>
</tr>
<tr class="header">
<th>Smallest resistor available</th>
<th>1 ohm</th>
<th><strong>1.2k ohm</strong></th>
</tr>
<tr class="odd">
<th>Total power consumption</th>
<th>&lt;15mW</th>
<th><strong>6.33mW</strong></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

## VI. Comparison of hand calculations and simulations

As shown in our hand calculations, we assign a theoretical limit to the resistance of any node to be 5kOhms. Because for a low output resistance design for any node, the resistance is approximately equal to the drain resistor, the drain resistance can be a max of 5 kOhms. This is confirmed in our schematic design with the largest drain resistance used as 5kOhms with the drain resistance of the differential amplifier and common source amplifier to be 3.5k Ohms and 1.2k Ohms respectively. We can expect the drain resistance of the CG stage to be larger than that of the other stages because the input stage affects the input referred noise, with the resistance being inversely proportional to the input noise. Thus we assigned the maximum drain resistance value of 5kOhms to the drain resistance. This is the most important limit to calculate because it controls the bandwidth of our circuit. Additionally, we used parametric sweeps to adjust the widths and the lengths (to find the current and resistance) that fit the criteria outlined in the hand calculations. For the differential amplifier stage, we once again see that the drain resistance is small, at 3.5kOhms, and that the width of the transistor is large at 10u in order to get high gain out of the stage. Additionally, it is seen that the current mirror has a ratio of approximately 2:1 in order to get high gain which additionally boosts the transconductance, with a larger width than min size to increase the input impedance too keep the current stable. For the CS amplifiers, we again see a drain resistance of 1.2kOhms which is under the assigned limit, with the width of the transistors at 10u to achieve high gm. The input of each stage is biased such that the Vg is roughly .7V, which is higher than the gate voltage so the gm is large and we have a large gain. Finally for the source follower, we use an extremely large width as described in the hand calculation to get as large of a transconductance in order to get a gain of roughly .8V. Then a voltage bias at the input was chosen in order to get a drain current of 1.44mA, which is close to our wanted drain current of 1/600 = 1.67mA to bias our output at roughly .5V. These values confirm the validity of our hand calculations as a rough gauge of how to bias our circuit. However due to higher order effects, we cannot obtain exact values for every bias point in our hand calculations. We can only calculate trends and values we want, which are found in exact through fine tuning using parametric sweeps in our design. In doing the hand calculations, we found important figures such as the node resistance limit and reducing our decoupling capacitors from the max capacitance to a value that achieved high bandwidth while maintaining signal integrity. We also found important trends such as biasing the source follower to achieve a .5V common mode output and having large output impedance of the current mirrors, which were all reflected in the design.

## VII. Conclusion

This project successfully demonstrated the design and implementation of a wideband trans-impedance amplifier tailored for optical receiver applications using the 45nm CMOS process. By methodically combining hand calculations and Cadence simulations, the TIA design met or exceeded the specified performance requirements, achieving a differential trans-impedance gain of 270.71 kΩ, a -3 dB upper corner frequency of 500 MHz, and a total power consumption of 6.33 mW—well below the 15 mW limit.

The iterative design process emphasized balancing gain, bandwidth, and stability. Key design strategies, including the adoption of a common-gate topology for the input stage and a differential amplifier for the output stage, were validated through extensive simulations. These configurations effectively addressed challenges such as input capacitance and noise while maintaining the required output swing and bandwidth.

Comparison of hand calculations and simulation results underscored the utility of analytical methods as a foundation for design. Despite minor discrepancies attributed to higher-order effects, the parametric sweeps and fine-tuning in simulation refined the circuit's performance.

This project not only highlights the effectiveness of the selected topology in wideband TIA design but also provides a framework for future enhancements, such as optimizing for even lower noise or higher gain-bandwidth products. The practical insights gained from this work contribute to the broader field of high-performance analog circuit design.

## VIII. References

[https://ieeexplore.ieee.org/document/6573594](https://ieeexplore.ieee.org/document/6573594)

[https://www.mdpi.com/2079-9292/8/10/1073](https://www.mdpi.com/2079-9292/8/10/1073)

[https://www.seas.ucla.edu/brweb/papers/Journals/BR_SSCM_1_2023.pdf](https://www.seas.ucla.edu/brweb/papers/Journals/BR_SSCM_1_2023.pdf)

Additional screenshots

<img src="report_images/image60.png" width="547" alt="Figure" />

<img src="report_images/image55.png" width="830" alt="Figure" />

<img src="report_images/image4.png" width="830" alt="Figure" />

<img src="report_images/image30.png" width="830" alt="Figure" />

<img src="report_images/image41.png" width="830" alt="Figure" />

<img src="report_images/image8.png" width="830" alt="Figure" />

<img src="report_images/image10.png" width="830" alt="Figure" />

<img src="report_images/image1.png" width="830" alt="Figure" />

<img src="report_images/image48.png" width="830" alt="Figure" />

<img src="report_images/image21.png" width="830" alt="Figure" />

<img src="report_images/image47.png" width="830" alt="Figure" />

<img src="report_images/image62.png" width="830" alt="Figure" />

<img src="report_images/image37.png" width="830" alt="Figure" />

<img src="report_images/image11.png" width="830" alt="Figure" />
