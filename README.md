# AI/ML-Enabled Adaptive Noise Cancellation for Defence Communication

A hybrid digital speech enhancement system designed for real-time defence communication. The system combines reference-assisted adaptive noise cancellation with AI-based speech enhancement to suppress different types of environmental and defence-related noise while preserving speech intelligibility.

## Development Overview — Up to NLMS

The project was developed in stages to validate the digital signal-processing pipeline before moving to the AI-based speech-enhancement stage.

### Stage 1 — Signal Generation and Microphone Simulation

A controlled dual-microphone test environment was created using clean speech and recorded engine noise.

- Sampling rate: **24 kHz**
- Target SNR: **5 dB**
- Primary microphone: **speech + environmental noise**
- Reference microphone: **environmental noise**
- Signals were resampled, amplitude-normalized, synchronized, and saved as WAV files.
- The 24 kHz sampling rate provides a Nyquist frequency of 12 kHz, allowing the system to process speech up to 8 kHz while retaining an anti-aliasing transition region.

### Stage 2 — Band Limiting and Sub-Band Decomposition

The primary and reference signals were processed to establish the project's sub-band architecture.

- Overall processing band: **80 Hz – 8 kHz**
- Crossover frequency: **1.5 kHz**
- Low-frequency band: **80 Hz – 1.5 kHz**
- High-frequency band: **1.5 kHz – 8 kHz**
- The reference signal was also filtered to the **80 Hz – 1.5 kHz** band for NLMS processing.
- Frequency-domain analysis using FFT was performed to verify the filtering and frequency separation.

This established the parallel processing structure:

```text
Primary Signal
      │
      ├── 80 Hz – 1.5 kHz ──→ NLMS
      │
      └── 1.5 kHz – 8 kHz ──→ AI Enhancement


**Stage 3 — NLMS Adaptive Noise Cancellation**

The low-frequency branch was developed using a Normalized Least Mean Squares (NLMS) adaptive filter.

The NLMS filter uses the reference microphone signal to estimate the noise component correlated with the primary microphone signal. The estimated noise is then subtracted from the primary signal to obtain the error/cleaned signal.

*Several controlled experiments were performed to understand:

->Effect of filter length and step size
->Effect of microphone-path delay
->Effect of noise-path gain
->Importance of correlation between primary and reference noise
->Noise-only cancellation performance
->Effect of continuous adaptation during speech
->Frozen-coefficient operation after an initial learning period
->Behaviour under a changing noise path


**Key NLMS Finding**

In a controlled experiment where the reference noise and primary noise path were known and the NLMS coefficients were learned during an initial noise-only period, the system achieved approximately:

35.6 dB noise reduction

This result validates the basic NLMS cancellation mechanism under controlled conditions.

However, continuous adaptation while speech is present caused speech distortion. This demonstrated an important requirement for the final system: speech-aware adaptation or adaptation control is required during double-talk conditions.

**Current Status**

At this stage, the following components have been successfully developed and validated in Python:

->24 kHz dual-microphone signal simulation
->80 Hz–8 kHz signal conditioning
->1.5 kHz sub-band decomposition
->Low-band reference-assisted NLMS
->Controlled noise cancellation experiments
->Adaptive noise-path experiments
->NLMS performance analysis.

