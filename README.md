# cs-amplifier_lab2
# CMOS Amplifier Design using TSMC 180nm Technology in LTSpice

## 1. Objective

To design and analyze a CMOS amplifier using **TSMC 180nm technology** in **LTSpice** and verify the theoretical calculations with simulation results.

### Given Specifications

VDD = 1.2 V  
Power constraint P ≤ 0.4 mW  
Load capacitance CL = 0.5 pF  
Channel length L = 360 nm  

The objective is to:

1. Design the amplifier using theoretical calculations.
2. Determine transistor sizing.
3. Simulate the circuit in LTSpice.
4. Compare theoretical and simulated gain.
5. Analyze the difference between practical and theoretical values.

---

# 2. Circuit Description

The circuit consists of:

• NMOS transistor acting as the amplifying device  
• PMOS transistor acting as the active load  
• Source degeneration resistor Rs  
• Bias voltage VB  
• Output node at the drain

Configuration: **Common Source Amplifier with PMOS Active Load**

```
           VDD
            |
          PMOS
            |
            |------ Vout
            |
           NMOS
            |
           Rs
            |
           GND
```

Input signal is applied to the NMOS gate.

---

# 3. Theory

A **Common Source MOS amplifier** is a fundamental analog amplifier topology.

Characteristics:

• High voltage gain  
• Phase inversion (180° phase shift)  
• Gain depends on transconductance and output resistance  

---

## MOSFET Saturation Current Equation

For MOSFET operating in saturation:

ID = (1/2) * kn' * (W/L) * (VOV)^2

Where

kn' = μn Cox  
VOV = VGS − VTH  

---

## Transconductance

gm = 2ID / VOV

---

## Output Resistance

ro = 1 / (λ ID)

---

## Voltage Gain

For source degeneration amplifier:

Av = - gm (ro1 || ro2) / (1 + gm Rs)

---

# 4. Design Calculations

## Step 1: Maximum Drain Current from Power Constraint

P ≤ VDD ID

ID = P / VDD

ID = 0.4 mW / 1.2 V

ID = 0.33 mA

ID = 330 µA

---

## Step 2: Output Voltage Selection

For symmetrical output swing:

Vout = VDD/2 + VRS

Vout = 0.6 + 0.2

Vout = 0.8 V

---

## Step 3: Overdrive Voltage

Assume:

VOV = 0.25 V  
VTH = 0.366 V  

VGS = VOV + VTH

VGS = 0.25 + 0.366

VGS = 0.61 V

---

## Step 4: Gate Voltage

VG = VGS + ID RS

Assume:

VRS = 0.2 V

VG = 0.61 + 0.2

VG = 0.81 V

---

## Step 5: Source Resistor

RS = VRS / ID

RS = 0.2 / 0.00033

RS = 606 Ω

---

# 5. NMOS Width Calculation

Drain current equation:

ID = (1/2) kn' (W/L) (VOV)^2

Where

kn' = μn Cox

μn = 273.81 cm²/Vs

Cox = εox / tox

εox = 8.854 × 10⁻¹² × 3.9

tox = 4.1 × 10⁻⁹

kn' = 2.306 × 10⁻⁴

Now solving for W:

W ≈ 16.48 µm

Thus

W1 = 16.48 µm

---

# 6. PMOS Width Calculation

For PMOS:

ID = (1/2) μp Cox (W/L) (VOV)^2

Assume

VTP = 0.39 V  
VOV = 0.25 V  

VSG = VOV + |VTP|

VSG = 0.25 + 0.39

VSG = 0.64 V

Since

VSG = VS − VG

VS = VDD

Therefore

VSG = VDD − VG

VB = 1.2 − 0.64

VB = 0.56 V

Solving current equation gives:

W2 = 39.05 µm

---

# 7. DC Operating Point (Simulation)

After LTSpice simulation transistor sizes were adjusted to:

W1 = 75 µm  
W2 = 95 µm  

Measured values:

ID1 = 323.33 µA  
ID2 = 323.33 µA  

Vout = 0.83 V

---

# 8. Transient Analysis

Measured values from simulation:

Vin(pp) = 0.019 V

Vout(pp) = 0.235 V

Voltage gain:

Av = Vout / Vin

Av = 0.235 / 0.019

Av = 12.36 V/V

---

## Gain in dB

Av(dB) = 20 log(Av)

Av(dB) = 20 log(12.36)

Av(dB) = 21.84 dB

---

# 9. AC Analysis

From AC simulation:

Av ≈ 21 dB

Upper cutoff frequency:

FH = 125.69 MHz

Lower cutoff frequency:

FL ≈ 0

Bandwidth:

BW = FH − FL

BW = 125.69 MHz

---

# 10. Gain Bandwidth Product

GBP = Av × BW

Convert gain to linear:

Av = 10^(21/20)

Av = 11.22

GBP = 11.22 × 125.69 MHz

GBP ≈ 1.41 GHz

---

# 11. Theoretical Gain Calculation

Given

ID = 300 µA  
VOV = 0.25 V

Transconductance:

gm = 2ID / VOV

gm = 2 × 300µA / 0.25

gm = 2.4 × 10⁻³

---

Assume channel length modulation:

λ = 0.1 V⁻¹

ro = 1 / (λ ID)

ro = 1 / (0.1 × 300µA)

ro = 33333 Ω

Parallel resistance:

ro1 || ro2

= 33333 || 33333

= 16665 Ω

---

Voltage Gain

Av = - gm (ro1 || ro2) / (1 + gm Rs)

Av = - (2.4×10⁻³ × 16665) / (1 + 2.4×10⁻³ × 606)

Av = 16.294 V/V

Gain in dB:

Av = 20 log(16.294)

Av ≈ 24.24 dB

---

# 12. Comparison of Results

| Parameter | Theoretical | Practical |
|----------|-------------|-----------|
Gain | 24.24 dB | 21.52 dB |
Bandwidth | Ideal | 125 MHz |
GBP | Ideal | 1.41 GHz |

---

# 13. Reasons for Difference Between Theoretical and Practical Values

The theoretical calculations assume **ideal MOSFET behavior**, while LTSpice simulation uses **realistic transistor models**. The main reasons for the difference are:

### 1. Short Channel Effects

In 180 nm technology, transistors experience short channel effects which reduce current compared to ideal equations.

---

### 2. Mobility Degradation

Carrier mobility decreases at high electric fields, reducing drain current.

---

### 3. Channel Length Modulation

In practical devices, the drain current increases with VDS, reducing output resistance.

This decreases the gain.

---

### 4. Parasitic Capacitances

MOSFET includes internal capacitances such as:

Gate-source capacitance  
Gate-drain capacitance  
Junction capacitances  

These affect the AC response and bandwidth.

---

### 5. Non-Ideal MOSFET Models

LTSpice uses **BSIM transistor models** which include many second-order effects ignored in hand calculations.

---

### 6. Process Variations

Fabrication variations cause changes in:

Threshold voltage  
Mobility  
Oxide thickness  

This causes deviations from theoretical predictions.

---

# 14. Conclusion

A CMOS amplifier using **TSMC 180 nm technology** was successfully designed and simulated in LTSpice.

Results obtained from simulation:

Gain ≈ 21.5 dB  
Bandwidth ≈ 125 MHz  
Gain Bandwidth Product ≈ 1.41 GHz  

The practical gain is slightly lower than the theoretical value due to **short channel effects, parasitic capacitances, mobility degradation, and channel length modulation**.

The experiment demonstrates the importance of **circuit simulation tools** in verifying analog circuit performance.

---

# Tools Used

LTSpice  
TSMC 180nm MOSFET Models  
Manual Design Calculations
