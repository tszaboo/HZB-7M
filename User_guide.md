# HZB-7M User Guide

![HZB-7M](https://github.com/tszaboo/HZB-7M/blob/main/Pictures/HZB-7M.jpg)

The HZB-7M is a high-impedance active buffer that converts a standard oscilloscope probe input (1 MΩ BNC) into a 50 Ω SMA output. It has no controls — connect power, connect your probe, and connect your instrument.

---

## Contents

1. [Safety](#safety)
2. [Connections](#connections)
3. [Input impedance](#input-impedance)
4. [Output levels and attenuation](#output-levels-and-attenuation)
5. [Setup — oscilloscope](#setup--oscilloscope)
6. [Probe compensation](#probe-compensation)
7. [Setup — spectrum analyzer](#setup--spectrum-analyzer)
8. [Setup — tinySA Ultra](#setup--tinysa-ultra)
9. [Setup — frequency counter](#setup--frequency-counter)
10. [Setup — VNA](#setup--vna)
11. [Setup — NanoVNA](#setup--nanovna)

---

## Safety

- The maximum input voltage is **±35 V** when used with a 1:10 passive probe. Without a probe, do not apply more than **±3.5 V** directly to the BNC input.
- The BNC and SMA connectors share a common ground. Do not connect the probe ground to a potential different from your instrument's ground.
- Do not float your oscilloscope or instrument chassis ground.
- The USB-C power input ground is **isolated** from the BNC/SMA signal ground. Any standard 5 V USB source works — a USB port on the instrument, a laptop, or a phone charger.
- Use indoors only. Do not use in wet environments or atmospheres with explosive gases.
- Do not use the buffer if there is any visible damage to the housing or connectors.

---

## Connections

| Connector | Description |
|---|---|
| **BNC input** | Connect an oscilloscope probe or BNC cable here. Maximum ±3.5 V direct; ±35 V with a 1:10 probe. |
| **SMA output** | Connect to your instrument via SMA cable. |
| **USB-C** | Power input, 5 V. No data. Any standard USB charger or instrument front-panel USB port works. |

---

## Input impedance

The HZB-7M input presents **1 MΩ ∥ 6 pF** — identical to a standard oscilloscope input. Standard passive probes can be connected and compensated exactly as they would be on a scope.

![Input impedance](https://github.com/tszaboo/HZB-7M/blob/main/Diagrams/Input%20impedance.png)

The 6 pF capacitance is intentional: it allows passive probes to be trimmed correctly and makes the loading on the circuit under test match what you would see when probing with a real oscilloscope.

---

## Output levels and attenuation

The HZB-7M has a fixed **1:2 voltage attenuation**, DC coupled, into a 50 Ω load. This attenuation is a result of the 50 Ω output impedance forming a voltage divider with the 50 Ω load.

When using a 1:10 passive probe, the probe itself attenuates the signal by a further factor of 10 before it reaches the buffer. The total attenuation from circuit to instrument is therefore 1:20.

| Probe | Attenuation from circuit to instrument input |
|---|---|
| Direct BNC or 1:1 probe | 1:2 (÷2, −6 dB) |
| 1:10 passive probe | 1:20 (÷20, −26 dB) |

Set the corresponding correction factor on your instrument so amplitude readings are correct.

| Parameter | Value |
|---|---|
| Output impedance | 50 Ω |
| Maximum output voltage | ±3.5 V into 50 Ω |
| Maximum output power (direct BNC, ±3.5 V input) | ~+21 dBm |
| Maximum output power (1:10 probe, ±35 V input) | ~+1 dBm |
| Noise floor | −65 dBm RMS (input shorted) |

---

## Setup — oscilloscope

1. Power the HZB-7M via USB-C.
2. Connect the SMA output to the oscilloscope with an SMA-to-BNC cable. Use a **50 Ω** input for best bandwidth; 1 MΩ works if 50 Ω is not available.
3. Connect your probe or BNC cable to the BNC input.
4. Set the probe correction factor on the oscilloscope channel:
   - Direct BNC or 1:1 probe: **×2**
   - 1:10 passive probe: **×20**

---

## Probe compensation

When using a 1:10 passive probe, it must be compensated for the HZB-7M's 6 pF input capacitance, exactly as you would compensate it on a scope input.

1. Connect the HZB-7M SMA output to your oscilloscope (**50 Ω** input preferred).
2. Connect the probe to the BNC input of the HZB-7M.
3. Touch the probe tip to the **square wave calibration output** on your oscilloscope's front panel (typically 1 kHz, 1 Vpp).
4. Clip the probe ground to the calibration output ground.
5. Observe the square wave on the oscilloscope channel connected to the HZB-7M output.
6. Adjust the trimmer capacitor in the probe body until the square wave has flat tops with sharp corners — no overshoot (over-compensated) and no rounding (under-compensated).

> **Note:** With 50 Ω termination the displayed amplitude will be ÷20 of the circuit voltage (1:10 probe × 1:2 buffer). The compensation procedure is the same on a 1 MΩ input, but the displayed amplitude will read twice as high.

---

## Setup — spectrum analyzer

The HZB-7M output is DC coupled and will carry any DC component present at the input. Most spectrum analyzers cannot tolerate DC at their input — a DC block is always required. For larger signals an attenuator may also be needed to keep the level within the analyzer's safe input range.

### DC block

Always place a DC block immediately after the HZB-7M SMA output.

Recommended Mini-Circuits options:
- **BLK-89-S+** — 0.1 MHz to 8 GHz, SMA, 50 V max DC. Good general-purpose choice.
- **BLK-18-S+** — 10 MHz to 18 GHz, SMA, 50 V max DC. Better if you need the full 750 MHz bandwidth and beyond.

### Attenuator

The required attenuator depends on the signal at the probe tip and the probe used:

| Signal at circuit | Probe | HZB-7M output | Attenuator needed (bench SA, +30 dBm damage) |
|---|---|---|---|
| < 200 mVpp | Direct / 1:1 | < −6 dBm | None |
| 200 mVpp – 2 Vpp | Direct / 1:1 | −6 to +14 dBm | 10–20 dB |
| Up to ±3.5 V | Direct / 1:1 | up to +21 dBm | 20–30 dB |
| Up to ±35 V | 1:10 probe | up to +1 dBm | None |

Recommended signal chain:

**HZB-7M SMA → DC block → attenuator (if needed) → spectrum analyzer input**

### Instrument-specific notes

- **Agilent / Keysight (e.g. N9320B, E4402B):** Damage level +30 dBm; safe continuous operation ≤ +20 dBm. Use the analyzer's internal attenuator to set an appropriate reference level.
- **Rohde & Schwarz (e.g. FSH4, FPC1500):** Damage level +30 dBm. Good overload protection on most models, but the DC block is still required.
- **Siglent (e.g. SSA3021X, SVA1015X):** Damage level +30 dBm, recommended continuous ≤ +20 dBm. Use the DC block; add a 10–20 dB attenuator when probing signals above ~200 mVpp without a 1:10 probe.

---

## Setup — tinySA Ultra

The [tinySA Ultra](https://www.tinysa.org/wiki/) is a popular low-cost handheld spectrum analyzer. The maximum input is **+6 dBm** as marked on the device — significantly lower than a bench analyzer. A DC block and attenuator are mandatory.

### Recommended chain

**HZB-7M SMA → DC block (BLK-89-S+) → 20 dB SMA attenuator → tinySA Ultra input**

With a 20 dB attenuator the worst-case levels are:
- **1:10 probe, ±35 V input:** +1 dBm − 20 dB = **−19 dBm** ✓
- **Direct BNC, ±3.5 V input:** +21 dBm − 20 dB = **+1 dBm** ✓

For small signals with a 1:10 probe where the output stays well below −10 dBm, a 10 dB attenuator is sufficient.

### Amplitude correction

Account for the full signal path when reading levels on the tinySA Ultra:

| Path | Correction to add to displayed level |
|---|---|
| Direct BNC + 20 dB attenuator | +26 dB (20 dB att. + 6 dB buffer) |
| 1:10 probe + 20 dB attenuator | +46 dB (20 dB att. + 6 dB buffer + 20 dB probe) |

> **Warning:** Never connect the HZB-7M output directly to the tinySA Ultra without a DC block and attenuator. Even a moderate signal at the probe tip can permanently damage the input mixer.

---

## Setup — frequency counter

A 50 Ω frequency counter can typically be connected directly to the HZB-7M — most counters tolerate the DC level on the output. Check the maximum DC input voltage of your counter; if it is below ±3.5 V, add a DC block.

The 1:2 buffer attenuation (and ÷10 from the probe if used) reduces the signal amplitude at the counter input. For frequency measurement this is rarely an issue as long as the signal remains above the counter's trigger threshold. For a 1:10 probe on a weak signal it may be worth checking the signal amplitude before connecting.

### Typical setup

**HZB-7M SMA → SMA-to-BNC cable → frequency counter 50 Ω input**

- Set the counter input to **50 Ω** to avoid reflections at higher frequencies.
- Set the trigger level to approximately half the expected peak amplitude at the counter input (i.e. half of the circuit voltage ÷ 2 for direct BNC, or ÷ 20 for a 1:10 probe).
- No correction factor is needed for frequency measurement — only amplitude readings are affected by attenuation.

Most frequency counters (e.g. Keysight 53210A, Siglent SDG counters, older HP/Agilent counters) work directly with the HZB-7M output. The low noise floor of the buffer (−65 dBm input-shorted) helps avoid false triggering on weak signals compared to a direct BNC connection to the circuit.

---

## Setup — VNA

A VNA measures transmission and reflection characteristics. When the HZB-7M is in the signal path, its 1:2 attenuation and 6 dB insertion loss affect the raw S-parameter readings. These effects can be calibrated out.

### What the HZB-7M is suited for

The HZB-7M is a **unidirectional active buffer** — signal flows from BNC input to SMA output only. This means:

- ✓ **S21 transmission measurements:** probing a node inside a circuit and measuring its frequency response or signal level at the VNA port
- ✓ **Frequency response of amplifiers and filters** where the probe tip is the input node and the VNA drives the circuit through a separate port
- ✗ **S11 reflection measurements of the DUT** — the 1 MΩ input impedance will dominate the reflection reading and does not represent the DUT's impedance

The buffer's bandwidth is 750 MHz. Measurements above this frequency will show the buffer's rolloff.

### Calibrating out the HZB-7M

The cleanest approach is a **1-port SOLT calibration at the BNC input**, which moves the VNA reference plane to the probe tip:

1. Connect the HZB-7M SMA output to the VNA measurement port. Power it via USB-C.
2. Perform a 1-port calibration at the BNC input of the HZB-7M:
   - **Short:** attach a BNC short cap to the BNC input (or use an SMA short + SMA-BNC adapter)
   - **Open:** leave the BNC input unconnected
   - **Load:** attach a 50 Ω BNC terminator to the BNC input
3. Save the calibration. The VNA reference plane is now at the BNC input and the buffer's attenuation, phase shift, and insertion loss are normalized out.

If your VNA supports **port extension / e-delay** instead:

1. Calibrate normally at the SMA connectors.
2. Connect the HZB-7M and measure a known reference.
3. Apply electrical delay until the reference shows flat phase.
4. Manually add **+6 dB** to all S21 magnitude readings to compensate for the buffer's 1:2 attenuation.

---

## Setup — NanoVNA

The NanoVNA (H4, F, V2, and similar) covers approximately 50 kHz to 1.5–3 GHz depending on variant. The same principles as a bench VNA apply; the calibration menu is built into the device.

### Calibrating out the HZB-7M

1. Connect the HZB-7M SMA output to **Port 2** of the NanoVNA. Power it via USB-C.
2. Connect Port 1 to the circuit or test fixture as usual.
3. In the NanoVNA menu go to **CALIBRATE** and perform SOLT at the BNC input of the HZB-7M:
   - **OPEN** — BNC input unconnected
   - **SHORT** — BNC short cap on the BNC input
   - **LOAD** — 50 Ω BNC terminator on the BNC input
   - **THROUGH** — connect Port 1 directly to the HZB-7M BNC input via a short cable
4. Select **DONE** and save the calibration slot.

The NanoVNA now references the BNC input as its port 2 plane. The buffer's attenuation and phase are calibrated out.

### Port extension (simpler alternative)

1. Calibrate the NanoVNA normally at its SMA connectors, without the HZB-7M.
2. Connect the HZB-7M to Port 2.
3. Measure a known through (connect Port 1 directly to the BNC input with a short cable).
4. Go to **CAL** → **ELECTRICAL DELAY** and adjust until the through reads 0° phase.
5. Add **+6 dB** to all S21 magnitude readings to correct for the buffer's 1:2 attenuation, or note it as a fixed offset.

### Limitations

- The HZB-7M is not suitable for S11 measurements of the DUT — same as with a bench VNA.
- The NanoVNA's dynamic range (typically 70–90 dB) is reduced by the 6 dB insertion loss of the buffer.
- There is no lower frequency limitation from the HZB-7M — it is DC coupled and passes from DC to 750 MHz.
