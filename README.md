# LABREPORT-3
# DIGITAL PASSBAND MODULATION (ASK AND FSK) 

## AMPLITUDE SHIFT KEYING (ASK)

ASK is a digital modulation technique where the amplitude of a carrier wave is varied in response to a digital signal. In this simulation:
* **Generation:** Uses the "Switching Method" to gate a high-frequency sine wave.
* **Detection:** Implements **Asynchronous Envelope Detection** to recover the signal.
* **Noise:** Demonstrates signal integrity under varying Signal-to-Noise Ratio (SNR) conditions.

# Amplitude Shift Keying (ASK) Simulation & Hardware Implementation

This project documents the implementation of **Amplitude Shift Keying (ASK)**, specifically the **On-Off Keying (OOK)** method. It demonstrates a complete communication loop from generation via the "Switching Method" to recovery using an "Asynchronous Demodulation Chain."

---

## Technical Specifications

| Parameter | Specification |
| :--- | :--- |
| **Carrier Frequency ($f_c$)** | $100\text{ kHz}$ (Sine Wave) |
| **Message Frequency ($f_m$)** | $2\text{ kHz}$ (Digital Bitstream) |
| **Modulation Type** | Digital Passband (ASK/OOK) |
| **Detection Type** | Asynchronous Envelope Detection |

---

## Implementation Stages

### Generation (The Switching Method)
The transmitter uses a **Dual Analog Switch** to gate the carrier. The digital bitstream acts as the control signal:
* **Logic '1' (High):** Switch closes $\rightarrow$ Carrier passes to output.
* **Logic '0' (Low):** Switch opens $\rightarrow$ Output is $0\text{V}$.

### Recovery (The Demodulation Chain)
To restore the $2\text{ kHz}$ data at the receiver, a three-stage process is implemented:

**Rectification:** The bipolar ASK signal is passed through a **Utilities Module (Rectifier)** to convert it into a unipolar signal.
   
**Envelope Extraction:** A **Tuneable Low-Pass Filter (LPF)** is used to remove the $100\text{ kHz}$ carrier ripples, leaving the "envelope" of the original message.

**Decision Circuit (The Comparator):** Because the filtered signal suffers from **integration distortion** (rounded edges), it is passed through a **Comparator with a Variable DC Threshold**. This "squares up" the signal and restores sharp transitions.

---

## Key Performance Indicators
* **Threshold Calibration:** The **Variable DC Threshold** is critical for minimizing Bit Error Rate (BER) in noisy environments.
* **Filter Bandwidth:** The LPF must be tuned specifically to reject the $100\text{ kHz}$ carrier while preserving the $2\text{ kHz}$ message integrity.

---
<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![A1](LABPICS/A1.jpg)
  
  ![A2](LABPICS/A2.jpg)

  ![A3](LABPICS/A3.jpg)
   
  ![A4](LABPICS/A4.jpg)

  ![A5](LABPICS/A5.jpg)

  ![A6](LABPICS/A6.jpg)

  ![A7](LABPICS/A7.jpg)

  ![A8](LABPICS/A8.jpg)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![A9](LABPICS/A9.png)
  
  ![A10](LABPICS/A10.png)

  ![A11](LABPICS/A11.png)
  
  ![12](LABPICS/A12.png)

  ![A13](LABPICS/A13.png)
  
  
</details>

---

# Frequency Shift Keying (FSK) Modulation & Demodulation


This section covers the implementation of **Frequency Shift Keying (FSK)**, a digital modulation scheme where the frequency of the carrier is shifted between two discrete values to represent binary data. This implementation utilizes a **Voltage Controlled Oscillator (VCO)** for generation and a **Zero-Crossing Detector (ZCD)** for asynchronous recovery.

---

## FSK Generation (VCO Switching)
The FSK signal is generated using a **Voltage Controlled Oscillator (VCO)** module. A $2\text{ kHz}$ digital bitstream is applied directly to the VCO's **DATA** input, causing the carrier to shift between two frequencies:

* **Logic '0' (Space):** The VCO outputs a lower carrier frequency ($f_1$).
* **Logic '1' (Mark):** The VCO outputs a higher carrier frequency ($f_2$).

This method ensures a continuous phase transition between symbols, effectively generating a **Continuous Phase FSK (CPFSK)** signal.

---

## Signal Recovery (Zero-Crossing Detection)
Demodulation is achieved through a **Zero-Crossing Detector (ZCD)** approach, which converts frequency variations into measurable DC voltage levels.

### Squaring & Pulse Generation
The incoming FSK signal is converted into a series of fixed-width pulses. 
* The circuit triggers a pulse at every **zero-crossing** of the carrier wave.
* Because the "Mark" frequency ($f_2$) has more cycles per second than the "Space" frequency ($f_1$), it produces a significantly **higher density of pulses** over the same time interval.

### Integration & Smoothing
The pulse train is fed into a **Tuneable Low-Pass Filter (LPF)** which acts as an integrator:
* **High Pulse Density ($f_2$):** Results in a higher average DC voltage at the filter output.
* **Low Pulse Density ($f_1$):** Results in a lower average DC voltage.
* **Result:** The filter effectively "traces" the pulse density, recreating the original $2\text{ kHz}$ square wave bitstream.

---

##  Technical Specifications

| Parameter | Value |
| :--- | :--- |
| **Bit Rate** | $2\text{ kbps}$ |
| **Modulation Method** | VCO-based Frequency Switching |
| **Demodulation Method** | Zero-Crossing Pulse Density Detection |
| **Recovery Logic** | Integration via Low-Pass Filtering |

---

<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![B1](LABPICS/B1.png)
  
  ![B2](LABPICS/B2.png)

  ![B3](LABPICS/B3.png)
  
  ![B4](LABPICS/B4.png)

  ![B5](LABPICS/B5.png)
  
  ![B6](LABPICS/B6.png)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![B7](LABPICS/B7.png)
  
  ![B8](LABPICS/B8.png)

  ![B9](LABPICS/B9.png)
  
  ![B10](LABPICS/B10.png)

  ![B11](LABPICS/B11.png)
  
</details>

## PHASE SHIT KEYING (BPSK AND QPSK)

This project demonstrates the implementation of **Binary Phase Shift Keying (BPSK)**, a robust digital modulation scheme. The experiment covers the generation of BPSK signals using the **Balanced Modulator Method** and recovery via **Coherent Product Detection**.

## System Architecture

### Generation (Modulator)
- **Input:** TTL Digital Signal (0V to 5V).
- **Conversion:** Bipolar mapping (Logic 1 = +V, Logic 0 = -V).
- **Modulation:** Multiplication of a 100 kHz sinusoidal carrier by the bipolar message.
- **Result:** A constant-amplitude carrier with 180° phase reversals at bit transitions.

### Recovery (Demodulator)
- **Product Detection:** Multiplication of the incoming BPSK signal by a synchronized "stolen" carrier.
- **Low-Pass Filtering:** Extraction of the unipolar bitstream.
- **Restoration:** Comparator-based thresholding to restore sharp digital edges.

## Key Performance Characteristics
- High immunity to noise (Low SNR environments).
- Maximum phase separation (180°) between binary states.
- Constant envelope modulation.

<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![C1](LABPICS/C1.jpg)
  
  ![C2](LABPICS/C2.jpg)

  ![C3](LABPICS/C3.jpg)
  
  ![C4](LABPICS/C4.jpg)

  ![C5](LABPICS/C5.jpg)

  ![C6](LABPICS/C6.jpg)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![C7](LABPICS/C7.png)
  
  ![C8](LABPICS/C8.png)

  ![C9](LABPICS/C9.png)
   
</details>

# QPSK Generation and Parallel Coherent Detection

This project explores **Quadrature Phase Shift Keying (QPSK)**. Unlike BPSK, QPSK transmits two bits per symbol by utilizing four distinct phase states ($45^\circ, 135^\circ, 225^\circ, 315^\circ$). This effectively doubles the data rate without increasing the spectral bandwidth.


### QPSK Generation (The IQ Modulator)
The QPSK signal is constructed using parallel bit-splitting:
- **Bit Splitting:** A high-speed serial bitstream is divided into two slower parallel streams: **In-phase (I)** and **Quadrature (Q)**.
- **Quadrature Carriers:** Two $100\text{ kHz}$ carriers—a sine wave and a cosine wave (shifted by $90^\circ$)—are modulated by the I and Q streams respectively.
- **Summation:** The I and Q modulated signals are combined in an Adder module to produce the final QPSK waveform.

### Signal Recovery (Parallel Coherent Detection)
Because the I and Q components are orthogonal (at right angles), they can be recovered independently:
- **Demodulation:** The signal is fed into two separate Product Detectors driven by the local I and Q carriers.
- **Filtering:** Individual Tuneable Low-Pass Filters (LPFs) extract the baseband I and Q streams.
- **Recombination:** The parallel streams are merged back into the original serial bitstream.

<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![D1](LABPICS/D1.jpg)
  
  ![D2](LABPICS/D2.jpg)

  ![D3](LABPICS/D3.jpg)
  
  ![D4](LABPICS/D4.jpg)

  ![D5](LABPICS/D5.jpg)

  ![D6](LABPICS/D6.jpg)
  
  ![D7](LABPICS/D7.jpg)
  
  ![D8](LABPICS/D8.jpg)

  ![D9](LABPICS/D9.jpg)
  
  ![D10](LABPICS/D10.jpg)

  ![D11](LABPICS/D11.jpg)

  ![D12](LABPICS/D12.jpg)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![D13](LABPICS/D13.png)
  
  ![D14](LABPICS/D14.png)

  ![D15](LABPICS/D15.png)

  ![D17](LABPICS/D17.png)
  
  ![D18](LABPICS/D18.png)

  ![D19](LABPICS/D19.png)
   
</details>

# Direct Sequence Spread Spectrum (DSSS)

**Direct Sequence Spread Spectrum (DSSS)** is a modulation technique that expands the bandwidth of a signal significantly beyond the original information bandwidth. By multiplying the digital message with a high-speed **Pseudo-Noise (PN)** sequence (the "chipping code"), the signal's power is spread across a wide frequency range.

## Spreading (The Transmitter)
The spreading process utilizes high-speed logic to mask the message:
- **The Chipping Code:** A PN sequence is generated at a rate much higher than the $2\text{ kHz}$ message bit rate.
- **The XOR Operation:** Each message bit is XORed with multiple "chips." 
    - If the message bit is '0', the PN sequence is transmitted normally.
    - If the message bit is '1', the PN sequence is inverted.
- **Result:** The narrow-band message becomes a wide-band, low-power signal that appears as background noise to unauthorized receivers.

## Despreading & Recovery (The Receiver)
Recovery requires a process of **Correlation**:
- **Synchronization:** The receiver must use an identical PN Sequence Generator, perfectly synchronized in time with the transmitter.
- **The Despreading XOR:** The spread signal is XORed again with the local PN code. Because of the XOR logic property ($A \oplus B \oplus B = A$), the high-speed code is removed, "collapsing" the wide-band signal back into the original narrow-band message.
- **Integration:** A Low-Pass Filter smooths out spikes, and a Comparator restores the final digital bitstream.

<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![J1](LABPICS/J1.jpg)
  
  ![J2](LABPICS/J2.jpg)

  ![J3](LABPICS/J3.jpg)
  
  ![J4](LABPICS/J4.jpg)

  ![J5](LABPICS/J5.jpg)

  ![J6](LABPICS/J6.jpg)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![J7](LABPICS/J7.png)
  
  ![J8](LABPICS/J8.png)

  ![J9](LABPICS/J9.png)
  
  ![J10](LABPICS/J10.png)

  ![J11](LABPICS/J11.png)
  
</details>

# Software Defined Radio (SDR) and Undersampling

This section explores the transition from hardware-defined communication to **Software Defined Radio (SDR)**. By shifting components like mixers, filters, and demodulators into the digital domain, we achieve a highly flexible system capable of reconfiguring for multiple modulation schemes (ASK, FSK, BPSK) without hardware changes.

## Core Concepts

### The Undersampling Architecture
SDR utilizes **Undersampling (Sub-Nyquist Sampling)** to down-convert high-frequency signals:
- **Aliasing as a Tool:** By intentionally sampling a 100 kHz carrier at a lower rate (e.g., 10 kHz), the signal "aliases" into a lower frequency baseband.
- **Nyquist Zones:** The spectrum is divided into zones of width $f_s / 2$. Undersampling translates the signal from a higher Nyquist zone down to the first zone, effectively replacing the physical local oscillator and mixer.
- **S/H Module:** The Sample-and-Hold module acts as the physical bridge between the continuous analog signal and the discrete digital domain.

### Software-Based Recovery
Once digitized via an Analog-to-Digital Converter (ADC), recovery is handled by software:
- **Digital Filtering:** Replaces analog components with software algorithms (FIR/IIR filters) to isolate the bitstream.
- **Reconfigurability:** The primary advantage of SDR is that a single hardware front-end can switch between decoding ASK, FSK, or BPSK simply by updating the software logic.

<details>
  <summary> Wiring Diagram</summary>

  <!-- Images are located in the LABPICS directory -->
  
  ![E1](LABPICS/E1.jpg)
  
  ![E2](LABPICS/E2.jpg)

  ![E3](LABPICS/E3.jpg)
  
  ![E4](LABPICS/E4.jpg)

  ![E5](LABPICS/E5.jpg)
  
</details>
<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![E6](LABPICS/E6.png)
  
  ![E7](LABPICS/E7.png)

  ![E8](LABPICS/E8.png)
  
  ![E9](LABPICS/E9.png)
  
</details>

---

# QUESTIONS AND ANSWERS

<details>
  <summary> Results</summary>

  <!-- Images are located in the LABPICS directory -->
  ![M1](LABPICS/M1.png)
  
  ![M2](LABPICS/M2.png)

  ![M3](LABPICS/M3.png)
  
</details>