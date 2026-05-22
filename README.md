# ASK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
1. Google Colab
# Program
```

import numpy as np
import matplotlib.pyplot as plt

data = "1011001"

# Parameters
bit_rate = 1
samples_per_bit = 100
A1 = 5          # ASK amplitude for bit 1
A0 = 0          # ASK amplitude for bit 0

f1 = 10         # FSK frequency for bit 1
f0 = 5          # FSK frequency for bit 0

# Time axis
t = np.linspace(0, len(data), len(data) * samples_per_bit)

digital_signal = []

for bit in data:
    if bit == '1':
        digital_signal.extend([1] * samples_per_bit)
    else:
        digital_signal.extend([0] * samples_per_bit)

digital_signal = np.array(digital_signal)

ask_signal = []

for i, bit in enumerate(data):

    start = i * samples_per_bit
    end = (i + 1) * samples_per_bit

    tt = t[start:end]

    if bit == '1':
        signal = A1 * np.sin(2 * np.pi * 10 * tt)
    else:
        signal = A0 * np.sin(2 * np.pi * 10 * tt)

    ask_signal.extend(signal)

ask_signal = np.array(ask_signal)

demod_ask = []

for i in range(len(data)):

    start = i * samples_per_bit
    end = (i + 1) * samples_per_bit

    segment = ask_signal[start:end]

    energy = np.sum(np.abs(segment))

    if energy > 50:
        demod_ask.append(1)
    else:
        demod_ask.append(0)
 
fsk_signal = []

for i, bit in enumerate(data):

    start = i * samples_per_bit
    end = (i + 1) * samples_per_bit

    tt = t[start:end]

    if bit == '1':
        signal = np.sin(2 * np.pi * f1 * tt)
    else:
        signal = np.sin(2 * np.pi * f0 * tt)

    fsk_signal.extend(signal)

fsk_signal = np.array(fsk_signal)

demod_fsk = []

for i in range(len(data)):

    start = i * samples_per_bit
    end = (i + 1) * samples_per_bit

    segment = fsk_signal[start:end]

    tt = t[start:end]

    ref1 = np.sin(2 * np.pi * f1 * tt)
    ref0 = np.sin(2 * np.pi * f0 * tt)

    corr1 = np.sum(segment * ref1)
    corr0 = np.sum(segment * ref0)

    if corr1 > corr0:
        demod_fsk.append(1)
    else:
        demod_fsk.append(0)

print("Original Data       :", data)

print("ASK Demodulated Data:",
      ''.join(map(str, demod_ask)))

print("FSK Demodulated Data:",
      ''.join(map(str, demod_fsk)))

plt.figure(figsize=(14, 10))

# Digital Signal
plt.subplot(4,1,1)
plt.plot(digital_signal)
plt.title("Digital Input Signal")
plt.ylim(-0.5, 1.5)

# ASK Signal
plt.subplot(4,1,2)
plt.plot(ask_signal)
plt.title("ASK Modulated Signal")

# FSK Signal
plt.subplot(4,1,3)
plt.plot(fsk_signal)
plt.title("FSK Modulated Signal")

# Demodulated ASK
plt.subplot(4,1,4)
plt.step(range(len(demod_ask)), demod_ask, where='mid', label='ASK')
plt.step(range(len(demod_fsk)), demod_fsk, where='mid', label='FSK')
plt.title("Demodulated Signals")
plt.ylim(-0.5, 1.5)
plt.legend()

plt.tight_layout()
plt.show()

```
# Output Waveform

<img width="989" height="990" alt="PCM_DM_DcExp" src="https://github.com/user-attachments/assets/2bd57779-4288-4a59-9fc2-c81b8467d81e" />

# Results

