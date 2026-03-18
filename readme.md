#  SonicLink — Ultrasound Text Transfer

Transfer text between two devices using **inaudible sound** — no internet, no Bluetooth, no WiFi required.

---

## How It Works

Each character is assigned a unique frequency between **19,000 Hz and 22,000 Hz** — above the range of human hearing (~18 kHz max). The transmitter plays these tones through its speaker one by one. The receiver listens through its microphone, detects which frequency is loudest, and maps it back to the character.

```
A → 19000 Hz
B → 19073 Hz
C → 19146 Hz
...
Z → 21195 Hz
0 → 21268 Hz
...
(space) → 22000 Hz
```

**One character = one tone = one frequency. That's the entire system.**

---

## Encoding Logic

| Parameter     | Value  | Why |
|---------------|--------|-----|
| Frequency range | 19,000 – 22,000 Hz | Inaudible to humans |
| Tone duration | 300 ms | Long enough for mic to detect reliably |
| Gap between tones | 80 ms | Silence so receiver knows one char ended |
| Debounce | 380 ms | Blocks duplicate detections during one tone |
| Detection threshold | 92 / 255 | Ignores background noise |

---

## Usage

1. Open `ultrasound-transfer.html` in a browser on **both devices**
2. On **Device A** → choose **Transmitter** → type message → tap Send
3. On **Device B** → choose **Receiver** → tap Start Listening
4. Hold devices **10–30 cm apart**, speaker facing microphone
5. Message appears on receiver screen character by character

> Works entirely in the browser. No installation, no server, no libraries.

---

## Requirements

- A normal browser 
- Microphone permission on the receiver device
- Devices must support audio above 19 kHz (most phones do)
- Quiet room for best results

---

## Known Limitations

- **Supported characters:** A–Z, 0–9, space, and `. , ! ?`
- **Speed:** ~1 character per 380 ms (~2–3 chars/sec)
- **Range:** ~tested for range upto 6 metres in moderately noisy enviroment  
- **Noise:** Loud environments can cause random character detections
- **Some older phones** roll off frequencies above 18 kHz and may not work

---

## Troubleshooting

| Problem | Fix |
|--------|-----|
| Receiver prints random chars | Raise threshold from `92` to `120` in code |
| Duplicate characters (HHEELLLO) | Raise `DEBOUNCE_MS` or increase `TONE_MS` |
| Nothing detected | Lower threshold to `80`, move devices closer |
| Garbled message | Reduce background noise, increase volume on transmitter |

---

## Files

```
ultrasound-transfer.html   — entire app, single file, no dependencies
README.md                  — this file
```

---

## Tech Stack

- **Web Audio API** — tone generation (transmitter) and FFT analysis (receiver)
- **MediaDevices API** — microphone access
- Plain HTML + CSS + JavaScript — zero dependencies, zero frameworks

---

---

*Built as a simple proof-of-concept for acoustic data transfer using only a browser.*