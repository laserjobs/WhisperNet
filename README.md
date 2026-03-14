# 🦇 WhisperNet: Ultrasonic Web Modem

WhisperNet is a completely browser-native, serverless Data-over-Audio (DoA) chat application. It uses the Web Audio API to transmit and receive ASCII text via near-ultrasonic sound waves (18.5 kHz – 19.5 kHz) that are generally inaudible to human ears but easily picked up by standard laptop and smartphone microphones.

## 🚀 Features
*   **Zero Dependencies:** Pure HTML/CSS/Vanilla JS. No build steps, no external libraries.
*   **3-Frequency FSK (Frequency Shift Keying):**
    *   `18.5 kHz` = Binary `0`
    *   `19.5 kHz` = Binary `1`
    *   `19.0 kHz` = Sync / Clock
*   **Return-to-Clock Timing:** Every data bit is preceded by a synchronization pulse, virtually eliminating clock drift over the air.
*   **Error Detection:** Implements Even Parity (9 bits per byte) and ASCII STX/ETX framing to identify and isolate ambient room noise and bit-flips.
*   **Live Waterfall Spectrogram:** Visualizes the 18kHz - 20kHz frequency spectrum in real-time, hardware-accelerated via HTML5 Canvas.

## 🛠️ Usage
1. Clone this repository or serve it via GitHub Pages.
2. Open the page on two different devices (or two browser tabs) in the same room.
3. Click **"Activate Microphone"** on the receiving device.
4. Type a message in the transmitting device and click **"Transmit"**.
5. Watch the real-time spectrogram paint your data in the air, and watch the text appear on the receiver!

> 🔒 **Important Note on Microphone Access:** Modern browsers enforce strict security policies for `getUserMedia()`. This project **requires HTTPS** (or `localhost`) to run. If you are hosting this yourself, use GitHub Pages (which provides automatic HTTPS) or run a local secure server (e.g., `npx serve --ssl`). It will silently fail on standard `http://`.

## 🔬 Technical Details (Browser DSP Limits)
While the Web Audio API theoretically supports up to the Nyquist limit of your hardware (e.g., 48 kHz / 96 kHz), standard consumer transducers (speakers and mics) roll off heavily above 20 kHz. We use the `18.5k - 19.5k` band because it sits perfectly in the "near-ultrasonic" sweet spot: inaudible to most adults, but easily reproducible by standard audio drivers without aggressive OS-level anti-aliasing.

Transmission speed is capped at **8 baud** (bits per second) to account for room reverberations, multi-path fading, and FFT processing windows (`fftSize: 2048` at `48,000` sample rate yields ~42ms temporal resolution).

## ⚠️ Security & Privacy Context
This project serves as an educational Proof-of-Concept for **Data-over-Audio (DoA) side-channels**. Similar ultrasonic beacon technologies have been used historically by marketing frameworks for cross-device tracking. By studying this, security researchers can better understand how to detect and filter unauthorized ultrasonic exfiltration via the Web Audio API.
