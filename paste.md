## Summary: **Fast Fourier Transform (FFT)**  
Fast Fourier Transform (FFT) is a computationally efficient algorithm used to calculate the discrete Fourier transform (DFT) and its inverse (IDFT). It transforms a sequence of complex numbers in the time domain into its frequency domain representation, enabling signal analysis, filtering, and data compression. FFT is widely used in engineering, physics, signal processing, image analysis, and more because it's much faster than directly calculating the DFT.

---

### Explanation:
---

### 1. **What is Discrete Fourier Transform (DFT)?**
The DFT decomposes a signal (e.g., time series) into its constituent frequencies. For example:
- A signal in the time domain might be represented as a sum of sine and cosine waves of different frequencies.
- The DFT provides the amplitude and phase of these underlying frequencies.

The formula for the DFT is:
\[
X[k] = \sum_{n=0}^{N-1} x[n] \cdot e^{-j2\pi kn/N}
\]
Where:  
- \(x[n]\): Input sequence (time domain).  
- \(X[k]\): Output sequence (frequency domain).  
- \(N\): Number of points in the sequence.  
- \(k\): Index of the frequency component.  
- \(e^{-j2\pi kn/N}\): Complex exponential representing the Fourier basis functions.

While DFT is mathematically accurate, calculating it directly for a large \(N\) can be slow with a computational complexity of **O(N²)**.

---

### 2. **What Does FFT Do?**
FFT is an optimized method for computing the DFT efficiently, reducing the computational complexity from **O(N²)** to **O(N log N)**. This makes FFT much faster, especially for large datasets.

Key Characteristics:
- **Speed:** It uses clever algorithms like divide-and-conquer techniques to avoid redundant calculations.
- **Structure:** Works best when \(N\) (the number of data points) is a power of 2, though variations like Cooley-Tukey FFT and Bluestein's algorithm handle non-power-of-2 cases.

---

### 3. **Applications of FFT:**
- **Signal Processing:** Analyze audio, radar, and ECG signals to identify frequency components.  
- **Image Processing:** Compression algorithms (like JPEG) rely on frequency domain representations.  
- **Communication Systems:** Orthogonal frequency-division multiplexing (OFDM) in 4G/5G wireless systems uses FFT for modulation and demodulation.  
- **Spectral Analysis:** Helps understand periodic components in data, useful in physics, astronomy, and engineering.  
- **Data Compression:** Lossy compression applications often utilize the frequency analysis capabilities of FFT.  

---

### 4. **How FFT Works Internally (Cooley-Tukey Algorithm):**
The Cooley-Tukey algorithm, one of the most common FFT implementations, follows these steps:
- **Divide:** The sequence is split into smaller sequences using recursive halving (divide-and-conquer).  
- **Transform:** Each smaller sequence is transformed individually.
- **Combine:** The results of the sub-transformations are combined with minimal computation, using symmetry and periodicity properties of the DFT.

This recursive approach leverages the periodicity and symmetry of the Fourier basis functions to reduce calculations.

---

### Example: Using FFT in Python
To demonstrate how FFT works, Python's `numpy` library provides an efficient implementation called `numpy.fft.fft`.

```python
import numpy as np
import matplotlib.pyplot as plt

# Example: Generate a simple signal (sine wave + noise)
fs = 1000  # Sampling frequency (Hz)
t = np.arange(0, 1, 1/fs)  # Time vector
freq1, freq2 = 50, 150     # Frequencies of sine waves
signal = np.sin(2 * np.pi * freq1 * t) + 0.5 * np.sin(2 * np.pi * freq2 * t)
noise = 0.2 * np.random.randn(len(t))
signal += noise

# Apply FFT
fft_result = np.fft.fft(signal)
fft_freq = np.fft.fftfreq(len(t), 1/fs)  # Frequency bins

# Plot the signal in time and frequency domain
plt.figure(figsize=(12, 6))
# Time domain plot
plt.subplot(1, 2, 1)
plt.plot(t, signal)
plt.title('Time Domain Signal')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')

# Frequency domain plot
plt.subplot(1, 2, 2)
plt.plot(fft_freq[:len(fft_result)//2], np.abs(fft_result)[:len(fft_result)//2])
plt.title('Frequency Domain Signal')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Amplitude')
plt.tight_layout()
plt.show()
```

**Explanation of Code:**
- **`np.fft.fft`**: Computes the FFT of the signal (time-to-frequency conversion).  
- **`np.fft.fftfreq`**: Computes the frequency bins associated with the FFT results.  
- **`len(t) // 2`**: Only the positive half of the spectrum is plotted since FFT output is symmetric about zero frequency for real-valued input signals.  

**Plot:**
1. The time-domain signal shows the raw wave with noise.  
2. The frequency-domain plot highlights the dominant frequencies in the signal (expected at 50 Hz and 150 Hz).

---

## References:
https://numpy.org/doc/stable/reference/routines.fft.html  
https://en.wikipedia.org/wiki/Fast_Fourier_transform   
https://www.mathworks.com/help/matlab/ref/fft.html  