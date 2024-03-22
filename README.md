# Proposal for IP block development for the Efabless 2024 Chipalooza challenge

IP Block name:		High gain Operation Amplifier
Designer/Design Team:	BKIC3
Date:				March 22, 2024

## Circuit description:
The proposed circuit is high gain operation amplifier designed for SkyWater 130 nm process technology. The circuit should performance should pass the specifications shown in the following table:

| Parameter                           | Min | Typical | Max        | Unit   | Notes |
| ----------------------------------- | --- | ------- | ---------- | ------ | ----- |
| Operating Temperature               | -40 |      25 |         85 |     °C |.      |
| Load Resistance                     |.  5 |.        |            |   kOhm |.      |
| Load Capacitance                    |.    |         |         30 |     pF |       |
| Total Harmonic Distortion.          |     |     --- |        --- |      % | At nominal output power |
| Signal-to-Noise Ratio               |  90 |     100 |.       110 |.    dB | |
| Output Voltage Swing                | 100 |.        | avdd - 100 |     mV |At 5 kOhms maximum load |
| Output Voltage Swing.               |  10 |         |  avdd - 10 |     mV | At 50 kOhms load |
| Power Consumption (Enabled)         |     |         |.       1.8 |     mA | At 5 kOhms maximum load |
| Power Consumption (No load)         |     |         |        300 |     uA | |
| Power Consumption (Disabled).       |     |       1 |.        10 |     nA | |
| PSRR (Power Supply Rejection Ratio) |  70 |      80 |            |     dB | |
| CMRR (Common mode Rejection Ratio)  |  80 |.        |.           |     dB | |
| Gain Bandwidth Product.             |     |.    2.5 |.           |    MHz | |
| Open loop gain                      |     |.    100 |.           |     dB | |Without load 
| Open loop gain                      |     |.    98  |.           |     dB |  With load 5kOhms & 30pF capacitor 
| Phase margin                        |     |.     70 |.           |      ° | |
| Wakeup time (ena transition 0 to 1) |     |     --- |.           |.    ns | |
| Common-mode input voltage           |   0 |.        |.      avdd |      V | Rail-to-rail operation |
| Equivalent Input Noise              |     |.        |.        30 | nV/√Hz | At 4kOhms maximum load, measured at 10kHz |



## Circuit pinout:

| Pinout | Pin name | Use |
| --- | --- | --- |
| avdd | analog power | 5V |
| dvdd | digital power | 1.8V |
| avss | analog ground |GND = 0V |
| dvss | digital ground | |
| ena | enable | dvdd domain |
| ibias | bandgap-controlled current bias | 10uA |
| iptat | PTAT current bias from bandgap | |
| isel | Bias selection bit | dvdd domain | |
| vinn | negative input | |
| vinp | positive input | |
| vout | output voltage | |

If the top level designer should choose to implement an analog test bus (ATB), the circuit would require extra pins to select the internal voltage nodes to be measured. A good idea should be to add an extra pin to choose between the on-chip biasing currents and an external one. Another possibility is to add an offset voltage trimming pin, but the extra circuit will add complexity and area usage.

## Circuit architecture:
The chosen operational amplifier topology is based on the folded cascode operation amplifier [1], also designed for the SkyWater 130 nm process by the author of this proposal. 

External resources (if any) (all resources must be open source): Vcm = 2,5V for comon mode feedback


## Specification challenges:
The chosen topology folded is relatively easy, bt the challenges is the structure makes it difficult to achieve rail to rail at the output, and the structure needcommon mode feedback for operate stability 


## Testbenches required for verifying circuit performance:
The simulation testbenches cover mostly DC and AC performance characterization (DC gain, input and output range, power consumption, output current, bandwidth, phase margin, PSRR, CMRR, input referred noise), and transient ones (THD, slew-rate). 
The simulation testbenches for run dc operating point, AC, DC gain, input and output range, power consumption, bandwith, phase margin, PSRR, CMRR, 


## Connections required for standalone (breakout) implementation:
The actual circuit input pins should be located as close as possible to the pads. The parasitic input current is mostly composed of the ESD protection. The output current can be quite high, so extra care must be taken for the output node and the supply nets of the output stage.
Connections pin : INN, INP, VDD, GND, VCM, VON,VOP

Current consumption measurements are only possible if the circuit has a dedicated power supply pin. If the circuit is working as intended, the current consumption can be inferred by the biasing current, which certainly will be measured from the on-chip standalone current bias generator. ATB points could be inserted to measure the biasing voltages, but are not strictly necessary.

## Test plan for standalone (breakout) implementation:
The proposed circuit, an operational amplifier, can be tested in most university labs, with conventional oscilloscopes. The proposed circuit specifications do not require high speed, ultra-low-noise, and ultra-low-current measurement equipment. Detailed measurement guidelines can be found in Analog Devices tutorials [4], among many other references.

## References
[1] Rodovalho, Luis Henrique, Cesar Ramos Rodrigues, and Orazio Aiello. "Rail-to-rail input/output bulk driven class AB operational amplifier with improved composite transistors." Analog Integrated Circuits and Signal Processing (2023): 1-13. DOI: https://doi.org/10.1007/s10470-023-02160-0
[2] Hogervorst, Ron, et al. "A compact power-efficient 3 V CMOS rail-to-rail input/output operational amplifier for VLSI cell libraries." IEEE journal of solid-state circuits 29.12 (1994): 1505-1513. DOI: https://doi.org/10.1109/ISSCC.1994.344656
[3]  Ian Williams. “Designing with a complete simulation test bench for op amps, Part 2: Small-signal bandwidth”. Electrical Design News. Online: https://www.edn.com/designing-with-a-complete-simulation-test-bench-for-op-amps-part-2-small-signal-bandwidth/
[4] Zumbahlen, Hank, ed. Linear circuit design handbook. Newnes, 2011. https://www.analog.com/media/en/training-seminars/design-handbooks/Op-Amp-Applications/Section1.pdf

