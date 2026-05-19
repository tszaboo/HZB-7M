# HZB-7M Buffer Characterisation

The HZB-7M is a compact high-impedance active buffer designed for use with oscilloscope probes. It features a 1 MΩ input impedance, 50 Ω output, and approximately 750 MHz bandwidth, allowing signals to be observed with minimal loading while driving standard 50 Ω coaxial test equipment.

## Specifications

| Parameter | Value |
|---|---|
| Bandwidth (−3 dB) | 750 MHz typical |
| Input impedance | 1 MΩ |
| Capacitive loading | ~6 pF |
| Output impedance | 50 Ω |
| Attenuation | 1:2 (DC coupled) |
| Input voltage range | ±35 V (with 1:10 probe) |
| Output voltage range | ±3.5 V active |
| Input connector | BNC (oscilloscope probe compatible) |
| Output connector | SMA |
| Power | USB Type-C, 5 V |
| Power isolation | Isolated — SMA and BNC share ground |
| Housing | Compact aluminium with PCB endplates |

---

## Measurements

### 1. VNA — Input/Output Characterisation

**Setup**

![VNA measurement setup](VNA/vna_measurement_block_diagram_v6.svg)

The buffer was measured with a LiteVNA from 10 MHz to 1 GHz (2× averaging). Two captures were taken: one via the BNC input and one via the SMA input directly.

**Results**

| Capture | S21 (log mag) | S11 at marker (756 MHz) |
|---|---|---|
| BNC input (`HZB7M.png`) | −3.06 dB | 198 + j271 mΩ |
| SMA input (`HZB7M-SMA.png`) | −3.02 dB | 338 + j842 µΩ |

The −3 dB S21 is consistent with the 1:2 voltage attenuation spec: the 50 Ω output driving a 50 Ω load forms a voltage divider, giving −6 dB in power or −3 dB in voltage referred to the source. The −3 dB bandwidth is confirmed at 750 MHz.

| | |
|---|---|
| ![HZB7M BNC input VNA](VNA/HZB7M.png) | ![HZB7M SMA input VNA](VNA/HZB7M-SMA.png) |
| BNC input | SMA input |

---

### 2. Frequency Response — Bode Plot

**Setup**

![Bode measurement setup](Bode/siglent_dual_chain_v2.svg)

Bode plot swept from 10 Hz to 20 MHz using the Siglent SDS824X built-in Bode plot function. Configuration: DUT input C3, DUT output C1, sweep type Simple, frequency mode Decade, 10 points/decade, amplitude 0.30 Vpp, offset 0.00 V, load 50 Ω.

**Results**

![Bode plot configuration](Bode/SDS824X_HD_PNG_19.png)
*Bode plot configuration screen*

![Bode plot result](Bode/SDS824X_HD_PNG_20.png)
*Frequency response: amplitude (yellow) and phase (blue), 10 Hz – 20 MHz*

The amplitude response is flat within ±0.3 dB from 10 Hz through the mid-band, with the phase beginning to rise toward 20 MHz. Representative values from the data table:

| Freq | Ampl (dB) | Phase (°) |
|---|---|---|
| 16 Hz | 0.27 | 0.57 |
| 100 Hz | 0.24 | 0.26 |
| 1 kHz | 0.44 | 1.63 |

---

### 3. Clipping

**Setup**

![Clipping measurement setup](Clipping/siglent_dual_chain_v2.svg)

Triangle wave at 500 kHz applied to the buffer input (C3). Output observed on C1. Two input amplitudes compared: one within the linear range, one exceeding the output swing limit.

**Results**

| | |
|---|---|
| ![No clipping](Clipping/SDS824X_HD_PNG_22.png) | ![Clipping](Clipping/SDS824X_HD_PNG_23.png) |
| Linear — Pk-Pk input: 6.73 V, output: 3.37 V | Clipping — Pk-Pk input: 9.43 V, output: 3.42 V |

With a smaller input signal the output is clean and proportional. With a larger input, the output peak flattens, consistent with the ±3.5 V output swing limit.

---

### 4. Signal Integrity

**Setup**

![Signals measurement setup](Signals/siglent_dual_chain_v2.svg)

Various waveforms applied to the buffer input (C3, 10× probe) and observed at the output (C1) to demonstrate normal operation across signal types.

**Results**

**Square wave — 500 kHz overview**

![Square wave 500 kHz](Signals/SDS824X_HD_PNG_24.png)
*C1 Pk-Pk: 509 mV, C3 Pk-Pk: 953 mV, f = 500 kHz*

**Square wave — 500 kHz rise time detail**

![Rise time](Signals/SDS824X_HD_PNG_25.png)
*Single rising edge; C1 Pk-Pk: 493 mV, C3 Pk-Pk: 939 mV*

**Hyperbolic tangent (tanh) signal — 25 kHz**

![tanh signal](Signals/SDS824X_HD_PNG_26.png)
*C1 Pk-Pk: 1.48 V, C3 Pk-Pk: 2.78 V, f ≈ 24.8 kHz*

**Amplitude modulated burst — ~2 MHz carrier**

![AM burst](Signals/SDS824X_HD_PNG_27.png)
*C1 Pk-Pk: 1.47 V, C3 Pk-Pk: 2.88 V, carrier ≈ 1.99 MHz*

All waveforms are faithfully reproduced at the output with the expected 1:2 attenuation.

---

### 5. Noise Floor

**Setup**

![Noise measurement setup](/Noise/siglent_dual_chain_v3.svg)

Input shorted with an SMA shorting plug. C1 measures the HZB-7M output; C3 is a P2200 passive probe connected directly at the scope input as a reference.

**Results**

![Noise floor](Noise/SDS824X_HD_PNG_29.png)
*Yellow (C1): HZB-7M output noise. Cyan (C3): scope reference channel.*

| Channel | RMS (mean) | Pk-Pk (mean) |
|---|---|---|
| C1 — HZB-7M output | 117.8 µV | 703 µV |
| C3 — scope reference | 1.08 mV | 3.05 mV |

The HZB-7M output noise floor is significantly lower than the scope's own probe channel reference, confirming low self-noise from the buffer.
