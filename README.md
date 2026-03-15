## Name : Lavanya D
## Reg.no : 212224060133
# PSK & QPSK
# Aim
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
# Tools required
 * Computer with google colab
# Program
# PSK
```
import numpy as np
import matplotlib.pyplot as plt

# Parameters
fs = 1000          # Sampling frequency
fc = 50            # Carrier frequency
bit_rate = 10      # Bits per second
Tb = 1 / bit_rate  # Bit duration
t = np.arange(0, 1, 1/fs)

# Generate random binary data
bits = np.random.randint(0, 2, bit_rate)

# Convert bits to signal
samples_per_bit = int(fs/bit_rate)
message = np.repeat(bits, samples_per_bit)

# Carrier signal
carrier = np.sin(2 * np.pi * fc * t)

# BPSK Modulation
psk_signal = np.zeros(len(message))

for i in range(len(message)):
    if message[i] == 0:
        psk_signal[i] = np.sin(2*np.pi*fc*t[i])
    else:
        psk_signal[i] = np.sin(2*np.pi*fc*t[i] + np.pi)

# Demodulation
demodulated = psk_signal * carrier

# Decision device
decoded_bits = []
for i in range(0, len(demodulated), samples_per_bit):
    sample = np.sum(demodulated[i:i+samples_per_bit])
    if sample > 0:
        decoded_bits.append(0)
    else:
        decoded_bits.append(1)

# Plotting
plt.figure(figsize=(12,8))

plt.subplot(4,1,1)
plt.plot(message)
plt.title("Message Signal")

plt.subplot(4,1,2)
plt.plot(carrier)
plt.title("Carrier Signal")

plt.subplot(4,1,3)
plt.plot(psk_signal)
plt.title("BPSK Modulated Signal")

plt.subplot(4,1,4)
plt.step(range(len(decoded_bits)), decoded_bits)
plt.title("Decoded Bits")
plt.tight_layout()
plt.show()
print("Original Bits:", bits)
print("Decoded Bits :", decoded_bits)
```
# QPSK
```
import numpy as np
import matplotlib.pyplot as plt

# Input binary data (must be even number of bits)
bits = [1,0, 1,1, 0,0, 0,1]

# Convert bits into bit pairs
symbols = []
for i in range(0, len(bits), 2):
    symbols.append(str(bits[i]) + str(bits[i+1]))

# Time axis
t = np.arange(0, 2*np.pi, 0.01)

# QPSK phase signals
s00 = np.sin(t + np.pi/4)      # 00
s01 = np.sin(t + 3*np.pi/4)    # 01
s10 = np.sin(t + 5*np.pi/4)    # 10
s11 = np.sin(t + 7*np.pi/4)    # 11

# Modulation
modulated_signal = []

for s in symbols:
    if s == "00":
        modulated_signal.extend(s00)
    elif s == "01":
        modulated_signal.extend(s01)
    elif s == "10":
        modulated_signal.extend(s10)
    elif s == "11":
        modulated_signal.extend(s11)

# Demodulation (simple sampling method)
decoded_bits = []
sample_point = int(len(t)/2)

for i in range(len(symbols)):
    sample = modulated_signal[i*len(t) + sample_point]

    if sample > 0.7:
        decoded_bits.extend([1,0])
    elif sample < -0.7:
        decoded_bits.extend([0,0])
    elif sample > 0:
        decoded_bits.extend([0,1])
    else:
        decoded_bits.extend([1,1])

# Plotting
plt.figure(figsize=(10,6))

# Input bits
plt.subplot(3,1,1)
plt.step(range(len(bits)), bits)
plt.title("Input Binary Data")
plt.ylim(-0.5,1.5)
plt.grid(True)

# QPSK Signal
plt.subplot(3,1,2)
plt.plot(modulated_signal)
plt.title("QPSK Modulated Signal")
plt.grid(True)

# Demodulated bits
plt.subplot(3,1,3)
plt.step(range(len(decoded_bits)), decoded_bits)
plt.title("Demodulated Signal")
plt.ylim(-0.5,1.5)
plt.grid(True)

plt.tight_layout()
plt.show()

print("Input Bits:", bits)
print("Decoded Bits:", decoded_bits)
```
# Output Waveform
# PSK
<img src="https://img.sanishtech.com/u/03e093604eb2e688cd8fcf0df614f9c0.png" alt="psk" loading="lazy" style="max-width:100%;height:auto;">

```
Original Bits: [0 0 1 0 1 1 1 0 1 0]
Decoded Bits : [0, 0, 1, 0, 1, 1, 1, 0, 1, 0]
```
# QPSK
<img src="https://img.sanishtech.com/u/7f5d0b95a3c1ea0e6481ce3060336442.png" alt="qpsk" loading="lazy" style="max-width:100%;height:auto;">

```
Input Bits: [1, 0, 1, 1, 0, 0, 0, 1]
Decoded Bits: [1, 0, 1, 0, 0, 0, 0, 0]
```
# Results
The experiment of modulation and demodulation of PSK and QPSK was successfully executed.
