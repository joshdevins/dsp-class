# DSP Class

Based on a compressed and higher level version of the Coursera MOOC
[Audio Signal Processing for Music Applications](https://www.coursera.org/course/audio).
We attended 3-days in Barcelona with Prof. Serra and 2-days in Berlin to finish
things off.

## Getting Started

Use the local `Vagrantfile` to build a VM with `sms-tools` and `essentia`
installed.

```bash
$ vagrant up
$ vagrant ssh
```

## Outline

- Day 1, June 22
  - basic mathematics
  - Fourier transform (FT)
  - discrete Fourier transform (DFT)
- Day 2, June 23
  - Fourier transform properties
  - Short-time Fourier transform (STFT)
    - segmentation
    - onset detection
- Day 3, June 24
  - sinusoidal modeling
  - harmonic models
    - detecting fundamental frequencies
    - leading to mid and high-level features
- Day 4, August xx
  - sinusoidal plus residual modeling
  - sound/music description
    - spectral-based audio features
    - description of sound/music events and collections
- Day 5, August xx
  - cont. of Day 4

## Day 1
### Notes

- mathematics
  - [sinusoidal functions](https://en.wikipedia.org/wiki/Sine_wave)
  - [complex numbers](https://en.wikipedia.org/wiki/Complex_number)
  - [Euler's formula](https://en.wikipedia.org/wiki/Euler's_formula)
  - complex sinusoids
  - scalar product of sequences
  - even and odd functions
  - convolution
- [Fourier transform (FT)](https://en.wikipedia.org/wiki/Fourier_transform)
  - actually asking, "does this frequency f exist and if so, what is the amplitude and phase"
  - sample-based frequency range (define range and steps), will find out amplitude and phase if the frequency exists
  - mathematical, theoretical transform but requires infite time and sinsusoidal wave
- [Discrete Fourier transform (DFT)](https://en.wikipedia.org/wiki/Discrete_Fourier_transform)
  - projection of the input signal onto a series of basis functions
  - assumes that the samples you choose are a stable representation of all time
  - discretized FT, requires to choose number of samples in time domain and the series of basis functions, output is the projection into the frequency domain of both amplitude and phase (samples are collapsed and axis is now frequency response)
- [Fast Fourier transform (FFT)](https://en.wikipedia.org/wiki/Fast_Fourier_transform)
  - an algorithm to compute the DFT and it's inverse
- [Short-time Fourier transform (STFT)](https://en.wikipedia.org/wiki/Short-time_Fourier_transform)
  - like DFT but does not assume that the window or samples chosen are representative of the entire signal
  - more tomorrow
  - STFT calls FFT (implementation of DFT) on a window
- [Nyquist frequency](https://en.wikipedia.org/wiki/Nyquist_frequency) on sampling frequency necessary
