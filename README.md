# esp32-wifi-audit

![C](https://img.shields.io/badge/Language-C-A8B9CC?style=flat-square)
![ESP-IDF](https://img.shields.io/badge/Framework-ESP--IDF%20v5.x-000000?style=flat-square)
![Target](https://img.shields.io/badge/Target-ESP32-E7352C?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

> ESP-IDF firmware for wireless auditing on the ESP32: scanning, sniffing, frame analysis, and capture serialization.

A modular C firmware that turns an ESP32 into a Wi-Fi auditing tool. It scans access points, sniffs air traffic, parses raw 802.11 frames, and exports captures in `pcap` (Wireshark) and `hccapx` (crack tool) formats - all controllable from an on-device web dashboard.

---

## Features

- **AP scanning & sniffing** via the `wifi_controller` component
- **802.11 frame parsing** with a dedicated `frame_analyzer` layer
- **pcap export** for analysis in Wireshark
- **hccapx export** for hashcat / aircrack ingestion
- **On-device web dashboard** to drive attacks from a browser
- **Attack flows**: deauth, handshake capture, PMKID and DoS - documented in `doc/ATTACKS_THEORY.md`
- **WSL toolchain helper** (`wsl_bypasser`) for building under Windows + WSL

---

## Project structure

```
esp32-wifi-audit/
├── main/                      # Application entry + attack handlers
│   ├── attack.c               # Attack dispatcher
│   ├── attack_dos.c           # DoS / deauth floods
│   ├── attack_handshake.c     # Handshake capture flow
│   └── attack_pmkid.c         # PMKID capture flow
├── components/
│   ├── wifi_controller/       # AP scanner, sniffer, Wi-Fi status
│   ├── frame_analyzer/        # Wi-Fi frame parsing & inspection
│   ├── hccapx_serializer/     # hccapx capture format writer
│   ├── pcap_serializer/       # pcap writer for Wireshark
│   ├── webserver/             # HTTP server + HTML dashboard
│   └── wsl_bypasser/          # WSL toolchain helper (by MR-Glint)
└── doc/
    └── ATTACKS_THEORY.md      # Theory behind the implemented attacks
```

---

## Build & flash

Requires **Espressif ESP-IDF v5.x** and the ESP32 toolchain.

```bash
idf.py set-target esp32
idf.py build
idf.py -p COMx flash monitor
```

On Windows + WSL, use the `wsl_bypasser` component to simplify the toolchain setup.

---

## Requirements

- ESP32 development board (ESP32 classic)
- ESP-IDF **v5.x** / ESP-IDF Windows installer or WSL toolchain
- USB-UART driver for `flash monitor`

---

## Documentation

| Document | Contents |
|----------|----------|
| `doc/ATTACKS_THEORY.md` | Theory & walkthroughs for the DoS, deauth and capture flows |

Each component ships its own `README.md` with implementation details.

---

## Disclaimer

This firmware is intended for **research, education, and authorized penetration testing only**. Only use it against networks you own or have explicit permission to test. The authors are not responsible for any misuse.

---

## License

Distributed under the MIT License. See `LICENSE` for details. The `wsl_bypasser` component is an open-source component authored by Mr.Glint