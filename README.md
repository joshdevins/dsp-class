# DSP Class

Based on a compressed and higher level version of the Coursera MOOC
[Audio Signal Processing for Music Applications](https://www.coursera.org/course/audio).
We attended 3-days in Barcelona with Prof. Serra and 2-days in Berlin to finish
things off.

## Lectures

Lecture slides are in [sms-tools](https://github.com/MTG/sms-tools/tree/master/lectures).

## Getting Started

Use the local `Vagrantfile` to build a VM with `sms-tools` and `essentia`
installed.

```
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
  - requires exact signal periodicity in the window, avoid this by applying a window function to reduce the impact of cutting a window and making the signal non-periodic (more on windowing tomorrow)
  - is an identity function/transformation - independently of the number of frequency samples taken for the DFT
  - is reversable, with inverse DFT
- [Fast Fourier transform (FFT)](https://en.wikipedia.org/wiki/Fast_Fourier_transform)
  - an algorithm to compute the DFT and it's inverse
  - complexity is O(N log N)
  - improves over a naive DFT implementation which is O(N^2)
- [Short-time Fourier transform (STFT)](https://en.wikipedia.org/wiki/Short-time_Fourier_transform)
  - like DFT but does not assume that the window or samples chosen are representative of the entire signal
  - more tomorrow
  - STFT calls FFT (implementation of DFT) on a window
- [Nyquist frequency](https://en.wikipedia.org/wiki/Nyquist_frequency), sampling theory

## Day 2

- properties of the DFT for real signals (not complex)
- linearity
  - amplitude increase in input signal
  - phase spectrum stays the same
  - magnitude spectrum reflects no changes in frequencies just amplitude
- shift
  - minor shift in phase (phi)
  - no change in magnitude spectrum
  - very large change in phase spectrum
  - phase spectrum is very sensitive to phase shift
- symmetry
  - ring buffer is used
  - split the window signal in half after windowing
  - push latter half into first half of new window such that at x=0, y=0
  - symmetric around 0
  - zero phase windowing
- convolution
  - for frequency filtering in the time domain, we use convolution
  - for frequency filtering in the frequency domain, we don't need convolution, just multiplication
  - tend to do filtering in the frequency domain as it's cheaper
  - filtering, resonance, dampening, reverberation, etc. are all real world examples
  - related to [impulse response](https://en.wikipedia.org/wiki/Impulse_response)
  - models of environmental impulse response is a convolution
  - every sample of input gets convulved with the entire impulse response
  - put the impulse response into every point of the input
  - potentially affects both magnitude and phase, or one or the other only
  - convolution in the time domain is the same as multiplication in the frequency domain and vice versa,
    convolution in the frequency domain is the same as multiplication in the time domain
- zero-padding
  - for interpolation and smoothing in the frequency domain
  - for small, instantaneous signals/events, zero padding at the end will result in more resolution in the frequency
    domain without affecting the accuracy of the DFT
  - if we just use more samples, we can "blur" the DFT by including more signals that are not relevant to the
    event we are interested in analyzing
  - effectively this increases the number of basis functions, and thus the frequency resolution we can obtain
- [Fast Fourier transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform)
  - O(N^2) to O(N log N)
  - most frequently, the [Cooley-Tukey algorithm](https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm)
  - relies on repetition and reuse of basis functions
- [Short-time Fourier transform (STFT)](https://en.wikipedia.org/wiki/Short-time_Fourier_transform)
  - introduces a notion of a segment or fragment of time of the entire sound
  - adapts from DFT in that it does not assume the window in the DFT is representative of the entire, signal ad infinitum
  - parameters
    - w: analysis window
    - l: frame number
    - H: hop size
  - loop outside DFT to perform the window hopping (with some window overlapping)
  - window overlapping is to ensure we capture the complete sequence of sounds, also considering that windowing is applied to each chunk/window
  - windowing algorithm/signal depends on the application and signal you are dealing with
  - e.g. rectangular window is fine for things like "pure" signals that are single sinusoids, like radar; not good for general audio as there is much "cutoff" noise around the main lobe
  - "FFT size" (window after zero-padding) must always be a power of 2
  - window is applied before zero-padding, which is before STFT
- properties and parameters of STFT
- window size is the most important and has the most effect
