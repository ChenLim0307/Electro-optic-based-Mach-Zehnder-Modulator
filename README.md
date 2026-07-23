# Electro-optic-based_Mach-Zehnder_Modulator
This repository contains the design, layout generation, and time-domain simulation of a silicon photonic Mach–Zehnder modulator (MZM) using the si_fab photonic design kit.

The project investigates how the phase-shifter length, voltage–length product ((V_\pi L)), and electrical resistance affect the modulation depth, bandwidth, and optical output of a silicon depletion-mode MZM.

Project Overview

A Mach–Zehnder modulator converts an electrical signal into optical intensity modulation by creating a phase difference between two optical waveguide arms.

The input optical signal is divided into two paths. An applied electrical voltage changes the effective refractive index of the phase-shifter waveguides through the free-carrier plasma-dispersion effect. The two signals are then recombined, producing an output intensity given approximately by:

[
I_{\text{out}} \propto
\cos^2\left(\frac{\Delta\varphi}{2}\right)
]

where (\Delta\varphi) is the differential optical phase shift between the two arms.

Objectives

The main objectives of this project are to:

Build a silicon photonic Mach–Zehnder modulator using the si_fab PDK.
Generate a GDSII layout of the modulator.
Simulate the time-domain response of the device.
Study the influence of phase-shifter length on modulation performance.
Compare a practical baseline device with a deliberately degraded device.
Demonstrate the trade-off between modulation efficiency and electrical bandwidth.
Main Features

The simulation includes:

Silicon depletion-mode phase-shifter waveguides
Mach–Zehnder interferometer architecture
Push-pull electrical drive signals
Integrated heater for DC phase-bias control
RF signal and ground electrodes
Parametric phase-shifter length sweep
Time-domain pseudo-random binary sequence simulation
Electrical and optical noise
GDSII layout generation
Comparison of baseline and modified device parameters
Device Configurations

Two modulator configurations are evaluated.

Parameter	Baseline Device	Modified Device
(V_\pi L)	1.2 V·cm	20 V·cm
Series resistance	50 Ω	(1 \times 10^5) Ω
Drive amplitude	1 V	1 V
Bit rate	5 Gb/s	5 Gb/s
RF noise	0.1 V	0.1 V
Optical input power	1 W	1 W
Optical noise	0.001 W	0.001 W
Steps per bit	50	50
Random seed	20	20

The modified device intentionally uses a much higher voltage–length product and resistance to demonstrate the combined effects of poor modulation efficiency and limited electrical bandwidth.

Length Sweep

The phase-shifter length is varied using:

lengths = np.arange(500, 2500, 500)

The simulated lengths are:

500 µm
1000 µm
1500 µm
2000 µm

For a phase shifter with a voltage–length product of 20 V·cm, the corresponding half-wave voltages are approximately:

Phase-Shifter Length	(V_\pi)
500 µm	400 V
1000 µm	200 V
1500 µm	133.3 V
2000 µm	100 V

These values are significantly greater than the applied 1 V drive signal, resulting in a very small optical phase shift for the modified device.

Repository Structure
.
├── README.md
├── src/
│   └── mzm_simulation.py
├── layouts/
│   └── mzm.gds
├── figures/
│   ├── mzm_gds_layout.png
│   ├── sweep_length_baseline.png
│   └── sweep_length_modified.png
├── reports/
│   └── technical_summary.tex
└── requirements.txt

Update this structure to match the actual filenames used in the repository.

Requirements

The project requires Python 3 and the following main packages:

numpy
matplotlib
si_fab
A compatible photonic-layout library required by si_fab

Install the standard Python dependencies with:

pip install numpy matplotlib

Install si_fab according to the installation instructions provided by the course, laboratory, or package maintainer.

A virtual environment is recommended:

python -m venv .venv

Activate it on Linux or macOS:

source .venv/bin/activate

Activate it on Windows:

.venv\Scripts\activate

Then install the required packages:

pip install -r requirements.txt
Running the Simulation

Run the main simulation script from the repository root:

python src/mzm_simulation.py

The script performs the following steps:

Creates a depletion-mode phase-shifter waveguide.
Creates a heated waveguide for phase-bias control.
Instantiates the Mach–Zehnder modulator.
Generates the GDSII layout.
Simulates the time-domain modulation response.
Sweeps the phase-shifter length from 500 to 2000 µm.
Produces plots comparing the electrical inputs and optical outputs.
GDSII Layout

The generated layout is saved as:

mzm.gds

The layout contains:

Two parallel optical interferometer arms
Phase-shifter waveguides
RF signal electrodes
RF ground electrodes
Electrical contact pads
Heater routing for phase-bias adjustment

The GDS file can be opened using software such as:

KLayout
gdsfactory viewers
Other GDSII-compatible layout tools
Simulation Model

The simulation uses a simplified electro-optic model.

The half-wave voltage is calculated from:

[
V_\pi = \frac{V_\pi L}{L}
]

The electrical response is approximated using an RC-limited model:

[
\tau = RC
]

and the corresponding lumped-element bandwidth is:

[
f_{\text{3dB}} \approx \frac{1}{2\pi\tau}
]

When capacitance or resistance is defined per unit length, the total device length must be included consistently. For realistic high-speed traveling-wave modulators, a distributed transmission-line model should be used instead of a purely lumped RC approximation.

Results Summary
Baseline Device

The baseline device produces a clear optical response that follows the input bit sequence.

Observed behavior includes:

Strong optical power modulation
Clear distinction between different phase-shifter lengths
Greater modulation depth for longer devices
Fast transitions between digital states
Minimal RC-related distortion at 5 Gb/s

Longer phase shifters generate a larger phase shift for the same applied voltage because:

[
V_\pi \propto \frac{1}{L}
]

Modified Device

The modified device produces an almost constant optical output with only small residual variations.

This degradation is caused by two effects:

Low modulation efficiency

The high (V_\pi L) value produces a half-wave voltage far above the available 1 V drive amplitude. The resulting phase change is therefore very small.

Limited electrical bandwidth

The large electrical resistance increases the RC time constant and strongly low-pass filters the electrical response.

As a result, the modified device cannot reproduce the input bit sequence effectively at 5 Gb/s.

Key Conclusion

The comparison demonstrates the fundamental trade-off between modulation efficiency and electrical bandwidth in silicon photonic modulators.

Increasing the phase-shifter length can reduce the required half-wave voltage, but it can also increase:

Junction capacitance
Optical propagation loss
RF attenuation
Device footprint
Electrical delay

A practical modulator must therefore balance:

(V_\pi L)
Insertion loss
Phase-shifter length
Junction capacitance
Electrical resistance
RF impedance matching
Optical and RF velocity matching
Target data rate
Limitations

The current model has several limitations:

It uses a simplified RC approximation.
It does not fully model distributed traveling-wave electrodes.
Optical propagation loss may not be included in full detail.
RF impedance mismatch is not explicitly evaluated.
RF and optical velocity mismatch is not considered.
The simulation does not calculate bit-error rate directly.
The model does not include fabrication-process variations.
The heater bias is fixed rather than automatically optimized.

Therefore, the results should be interpreted as a comparative design study rather than a complete prediction of fabricated-device performance.

Future Work

Potential extensions include:

Optimizing PN-junction doping profiles
Sweeping junction offset and waveguide dimensions
Including insertion-loss calculations
Implementing a distributed transmission-line model
Studying impedance and velocity matching
Optimizing the quadrature bias point
Comparing single-arm and push-pull operation
Generating optical eye diagrams
Calculating extinction ratio and eye opening
Estimating bit-error rate
Evaluating higher bit rates
Comparing silicon with thin-film lithium niobate
Comparing silicon with barium-titanate modulators
Performing Monte Carlo fabrication-tolerance analysis
Example Output

The simulation generates plots containing:

The applied RF drive signal
The complementary or reversed drive signal
The resulting optical output power
Separate traces for each phase-shifter length

Example image links can be added as follows:

![Baseline length sweep](figures/sweep_length_baseline.png)

![Modified length sweep](figures/sweep_length_modified.png)
