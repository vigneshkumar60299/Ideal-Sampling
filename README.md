# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.
# Tools required
# Program
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs = 1000  
t = np.linspace(0, 1, fs, endpoint=False)
f_signal = 5 
x = np.sin(2 * np.pi * f_signal * t)

fs_sample = 20  
Ts = 1 / fs_sample
sample_indices = np.arange(0, len(t), int(fs/fs_sample))


ideal_samples = np.zeros_like(x)
ideal_samples[sample_indices] = x[sample_indices]

pulse_width = int(0.05 * fs)  
natural_samples = np.zeros_like(x)
for idx in sample_indices:
    natural_samples[idx:idx + pulse_width] = x[idx:idx + pulse_width]


flattop_samples = np.zeros_like(x)
for i in range(len(sample_indices) - 1):
    start = sample_indices[i]
    end = sample_indices[i+1]
    flattop_samples[start:end] = x[start]

def lowpass_filter(signal, cutoff=10, fs=1000, order=5):
    nyq = 0.5 * fs
    normal_cutoff = cutoff / nyq
    b, a = butter(order, normal_cutoff, btype='low')
    return lfilter(b, a, signal)

recon_ideal = lowpass_filter(ideal_samples)
recon_natural = lowpass_filter(natural_samples)
recon_flattop = lowpass_filter(flattop_samples)

plt.figure(figsize=(12, 10))

plt.subplot(4,1,1)
plt.plot(t, x, label='Original Signal')
plt.title('Original Signal')
plt.legend()

plt.subplot(4,1,2)
plt.stem(t, ideal_samples, linefmt='r-', markerfmt='ro', basefmt=" ")
plt.title('Ideal Sampling')

plt.subplot(4,1,3)
plt.plot(t, natural_samples, 'g')
plt.title('Natural Sampling')

plt.subplot(4,1,4)
plt.plot(t, flattop_samples, 'm')
plt.title('Flat-top Sampling')

plt.tight_layout()
plt.show()
plt.figure(figsize=(12, 8))

plt.plot(t, x, 'k--', label='Original')
plt.plot(t, recon_ideal, 'r', label='Reconstructed Ideal')
plt.plot(t, recon_natural, 'g', label='Reconstructed Natural')
plt.plot(t, recon_flattop, 'm', label='Reconstructed Flat-top')

plt.title('Signal Reconstruction Comparison')
plt.legend()
plt.show()
```
# Output Waveform
```
<img width="1189" height="989" alt="image" src="https://github.com/user-attachments/assets/8715a9bb-e22e-4a9e-b85b-6c11071327e9" />

```
# Results
```
<img width="1189" height="989" alt="image" src="https://github.com/user-attachments/assets/b0de8356-cffb-4bb6-b85f-506ba38d64f4" />

```

