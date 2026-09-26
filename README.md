# AI/ML-Enabled Adaptive Noise Cancellation for Defence Communication

A hybrid digital speech enhancement system designed for real-time defence communication. The system combines reference-assisted adaptive noise cancellation with AI-based speech enhancement to suppress different types of environmental and defence-related noise while preserving speech intelligibility.

## Problem Statement

Defence personnel often need to communicate in environments containing engine, vehicle, rotor, machinery, and impulsive noises. These noises can interfere with speech captured by communication microphones and reduce speech intelligibility.

The objective of this project is to develop an embedded noise-reduction system that can process speech in real time and provide a cleaner speech signal for transmission over a communication system.

## Proposed Solution

The proposed system uses two microphones and parallel signal-processing paths.

- **Primary microphone:** Captures speech + environmental noise.
- **Reference microphone:** Captures environmental noise with minimal speech leakage.
- **Low-frequency path:** Uses reference-assisted NLMS adaptive filtering for correlated noise.
- **High-frequency path:** Uses AI-based speech enhancement for non-stationary and impulsive components.
- **Output:** The processed sub-bands are recombined to produce enhanced speech for communication.


### Dual-Microphone Approach

Two microphones are used to provide different signal information.

The primary microphone captures the desired speech together with environmental noise, while the reference microphone is positioned to capture environmental noise with minimal speech leakage.

The reference signal provides the adaptive filter with information about the noise component that is correlated with the primary microphone noise.


### Hybrid Processing Approach

A hybrid architecture is used because different noise conditions may require different processing strategies.

The reference-assisted NLMS path uses the second microphone to estimate and suppress correlated noise, while the AI path is intended to enhance speech under more complex, non-stationary, and impulsive noise conditions.

The processed sub-bands are subsequently recombined to produce the final enhanced speech signal.

## System Architecture

```text
Primary Microphone
       │
       ▼
   24 kHz ADC
       │
       ▼
 Digital Filtering
       │
   ┌───┴────────────┐
   │                │
80 Hz–1.5 kHz   1.5–8 kHz
   │                │
   ▼                ▼
NLMS Adaptive   AI Speech
Noise           Enhancement
Cancellation
   │                │
   └───────┬────────┘
           │
           ▼
      Recombination
           │
           ▼
     Enhanced Speech
           │
           ▼
   Communication System



##Current Prototype

The current software prototype is being developed in Python using recorded speech and environmental noise.

### Stage 1 – Signal Preparation

- Speech and engine-noise audio loading
- Resampling to **24 kHz**
- Signal duration alignment
- Noise scaling to a target input SNR
- Generation of primary and reference microphone signals



### Stage 2 – Sub-Band Filtering

The signal is divided into the required frequency regions:

- Overall band: **80 Hz – 8 kHz**
- Low band: **80 Hz – 1.5 kHz**
- High band: **1.5 kHz – 8 kHz**
- Sampling rate: **24 kHz**



### Sampling Rate Selection

A sampling rate of **24 kHz** was selected because the system processes speech up to **8 kHz**.

At 24 kHz, the Nyquist frequency is **12 kHz**, providing a 4 kHz transition region above the 8 kHz signal bandwidth for practical anti-aliasing filtering.

Using a 16 kHz sampling rate would place the 8 kHz upper limit directly at the Nyquist frequency, leaving no practical transition band.


### Sub-Band Crossover Selection

The **1.5 kHz crossover** was selected as an engineering design choice to divide the signal into two processing regions.

- **80 Hz–1.5 kHz:** Processed using reference-assisted NLMS adaptive filtering.
- **1.5–8 kHz:** Processed using AI-based speech enhancement.

This allows the two processing methods to operate on frequency regions where their respective processing characteristics can be evaluated separately.


### Dual-Microphone Approach

Two microphones are used to provide different signal information.

The primary microphone captures the desired speech together with environmental noise, while the reference microphone is positioned to capture environmental noise with minimal speech leakage.

The reference signal provides the adaptive filter with information about the noise component that is correlated with the primary microphone noise.




### Stage 3 – NLMS Adaptive Noise Cancellation

The Normalized Least Mean Squares (NLMS) algorithm was implemented and tested using controlled simulations.

Experiments investigated:

- Adaptive filter length
- Step size
- Reference-to-primary signal delay
- Noise-path gain
- Filter convergence
- Noise-only cancellation
- Frozen-coefficient operation

The controlled experiments verified that the NLMS filter can effectively cancel noise when the reference signal has a strong correlation with the noise present in the primary microphone signal.

### Stage 4 – Adaptive NLMS Experiments

The NLMS system was further tested under changing noise conditions.

Experiments included:

- Changing noise paths
- Energy-based speech activity detection
- Hangover-based adaptation control
- Correlation-based detection
- Adaptive coefficient tracking

These experiments showed that continuously adapting the NLMS filter during speech can introduce speech distortion. This motivated the need for better speech-activity control and a complementary AI-based processing path.



## Representative Results

Selected output audio files from the NLMS experiments are included in the `results/` directory.

```text
results/
├── nlms_stage3e_frozen_clean_low.wav
├── stage4_adaptive_nlms_clean_low.wav
└── nlms_stage4d_correlation.wav
## Project Structure

```text
ANC_python/
├── audio/
│   ├── clean_speech.wav
│   └── engine_noise.wav
│
├── results/
│   ├── nlms_stage3e_frozen_clean_low.wav
│   ├── stage4_adaptive_nlms_clean_low.wav
│   └── nlms_stage4d_correlation.wav
│
├── create_noisy_signal.py
├── main.py
├── README.md
└── .gitignore



## Technologies Used

### Software

- Python
- NumPy
- SciPy
- Matplotlib
- Git
- GitHub
- Visual Studio Code

### Signal Processing

- Digital filtering
- Band-pass filtering
- Sub-band signal processing
- NLMS adaptive filtering
- FFT-based frequency analysis
- SNR-based evaluation

### AI/ML

- Python-based AI/ML development
- Neural-network-based speech enhancement
- High-frequency speech-band processing

### Target Embedded Platform

The final system is intended to be implemented on embedded hardware capable of real-time audio processing.

The current prototype is being validated in Python before moving toward embedded implementation.



## Current Development Status

The project is currently being developed in multiple stages.

### Completed

- Dual-microphone signal model implemented in Python
- Audio resampling to **24 kHz**
- Primary and reference microphone signal generation
- 80 Hz–8 kHz signal filtering
- Low/high sub-band separation at **1.5 kHz**
- NLMS adaptive filtering implementation
- Controlled NLMS noise-cancellation experiments
- Adaptive NLMS experiments with changing noise paths
- Initial GitHub repository setup and documentation

### In Progress

- AI-based speech enhancement for the **1.5–8 kHz** band
- Integration of the NLMS and AI processing paths
- Speech-quality evaluation
- Real-time processing optimization

### Planned

- Embedded hardware implementation
- Real-time audio acquisition
- Low-latency processing
- Hardware-level testing with actual microphones
- Evaluation under different defence-related noise conditions



## Evaluation

The system is evaluated using signal-quality and noise-reduction measurements.

The main evaluation parameters include:

- **Input SNR:** Measures the quality of the noisy microphone signal before processing.
- **Output SNR:** Measures the signal quality after noise reduction.
- **SNR Improvement:** Difference between output and input SNR.
- **Noise Reduction:** Measures the reduction in noise power.
- **Speech Intelligibility:** Used to evaluate whether speech remains understandable after processing.

Controlled NLMS experiments demonstrated significant noise cancellation when the reference noise and primary noise were strongly correlated.

The final system will also evaluate the AI-based processing path and the combined output after sub-band recombination.


## Future Hardware Implementation

The final system is intended to operate as a real-time embedded speech-processing unit.

The planned hardware architecture includes:


Microphone 1 ──┐
               ├──► Audio Acquisition ──► Embedded Processor
Microphone 2 ──┘                              │
                                             ▼
                                  Parallel Signal Processing
                                      │              │
                                      ▼              ▼
                                    NLMS            AI
                                      │              │
                                      └──────┬───────┘
                                             ▼
                                      Signal Recombination
                                             │
                                             ▼
                                       Clean Speech
                                             │
                                             ▼
                                      Radio / Modem



## Future Work

The next development stages will focus on:

- Preparing high-band speech data for AI training
- Developing a lightweight AI speech-enhancement model
- Training and testing the model with different noise conditions
- Combining the NLMS and AI outputs
- Measuring the final system's SNR improvement and speech intelligibility
- Optimizing the complete pipeline for real-time embedded execution
- Testing the system with real microphone inputs

## Project Status

This repository contains the current Python-based research and development prototype.

The embedded hardware implementation and AI-based processing stages are currently under development.


