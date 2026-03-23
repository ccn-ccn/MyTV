# MyTV 
**The World's First Voice Operated Agentic TV Platform**

MyTV is a cutting edge, voice first television system powered by open source Linux and AI agents. Designed for secure and intelligent control over media, communication, and productivity.

<img width="1536" height="1024" alt="MyTV3" src="https://github.com/user-attachments/assets/f581b328-6460-4de4-bac6-81a3e982b47d" />
<img width="1536" height="1024" alt="MyTV" src="https://github.com/user-attachments/assets/528d71ee-1d85-44f1-9471-999271d4f79d" />
<img width="1536" height="1024" alt="nec1" src="https://github.com/user-attachments/assets/8bf0feb4-616b-48e6-b33e-157b502dde6c" />
<img width="1536" height="1024" alt="nec2" src="https://github.com/user-attachments/assets/3b2c9893-8847-40ab-a519-5533d6376a10" />

## What Makes MyTV Different

Most smart TV platforms are streaming front-ends with a voice assistant bolted on. MyTV is architected the other way around — voice control and local AI are the foundation, and everything else is built on top.

- **Everything by voice** — Every function, across every app, is accessible through a single wake word. Common commands respond in under one second, entirely on-device.
- **Truly offline-first** — Local media, retro gaming, and voice control work with no internet connection and no account. The cloud unlocks additional capabilities for users who want them; it is never a requirement for core function.
- **End-to-end encrypted communication** — Video calls are encrypted client-to-client via Matrix. The server relays signalling only and cannot see call media. No third-party has access to your conversations.
- **Privacy by architecture** — No telemetry. No behavioural data collection. No advertising infrastructure. The hardware TPM seals your disk encryption key against your verified boot chain — your data stays on your device.
- **One device, three rooms** — Switch between TV, conference station, and workstation by voice. The hardware handles all three without compromise.

---

## Hardware Stack

| Component          | Specification                                                              |
|--------------------|----------------------------------------------------------------------------|
| **Compute**        | Raspberry Pi 5 (8GB)                                                       |
| **AI Accelerator** | Hailo-8L 13 TOPS NPU (USB3)                                                |
| **Storage**        | 256GB NVMe SSD                                                             |
| **Security**       | Infineon SLB9670 Hardware TPM 2.0                                          |
| **Camera**         | Leopard Imaging LI-IMX179-USB-130H-CONN · 8MP · 130° FOV · USB3 UVC        |
| **Microphone**     | Blue Yeti Nano (fixed install) · Rode Wireless ME (large room / body-worn) |
| **Displays**       | Sharp / NEC M, MA, P Series large-format commercial displays               |
| **Keyboard**       | Man & Machine · Rii wireless                                               |
| **Enclosure**      | Argon ONE V3                                                               |

---

## Software Stack

| Layer             | Components                                                                                                                      |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------|
| **OS**            | Raspberry Pi OS Bookworm 64-bit                                                                                                 |
| **Compositor**    | Labwc (wlroots-based Wayland)                                                                                                   |
| **Audio**         | PipeWire + WebRTC AEC                                                                                                           |
| **Security**      | PCR-bound LUKS full-disk encryption · Signed A/B OTA updates · nftables default-deny firewall · AppArmor application sandboxing |
| **Communication** | Matrix Rust SDK · Conduit homeserver · Olm/Megolm E2EE                                                                          |
| **AI Pipeline**   | Whisper Small INT8 (Hailo-8L NPU) · MiniLM-L6 intent classifier · Gemma 2 2B IT INT4 · Piper TTS                                |

---

## Voice Pipeline

All system functions are accessible through the **Hey MyTV** wake word. A hybrid intent architecture keeps common commands under one second, with the full language model available for complex and conversational queries. All inference runs on-device. No audio is sent to any server.

```
[ You   ] "Hey MyTV, play my Venice photos."
[ MyTV  ] Finds and streams from the best available source automatically.

[ You   ] "Hey MyTV, start retro multiplayer with Alex."
[ MyTV  ] Hosts a Netplay session → sends Alex a game invitation via encrypted message.

[ You   ] "Hey MyTV, join the 2pm call."
[ MyTV  ] Activates camera and room audio → joins the Matrix E2EE conference room.

[ You   ] "Hey MyTV, switch to HDMI 2."
[ MyTV  ] Sends CEC command → activates cable input → voice pipeline stays live on all inputs.
```

---

## Native Applications

### 📺 TV
Live television via built-in tuner or IPTV streams. Optimised for 10-foot large-screen navigation by voice or remote. HDMI-CEC input switching lets you call MyTV back from any input at any time — say "Hey MyTV, come back" from cable, satellite, or any connected device.

### 🎵 Media
A unified, offline-first media library covering audio, video, photos, and documents in a single voice-searchable collection. Three-tier encrypted access scales from fully local to fully cloud-synced:

- **Tier 1 — USB** · Plug in a drive. Auto-indexed in under 30 seconds. Always available, no network required.
- **Tier 2 — Local network** · Paired devices on the same Wi-Fi serve their on-device libraries live over an encrypted, authenticated connection. Browse and stream as if it were local.
- **Tier 3 — MyCloud** · Your synced library is accessible from anywhere with an internet connection. Client-side encrypted before upload — the server holds only ciphertext.

### 👾 Retro Games
Two clean, non-overlapping gaming modes — no setup complexity, no third-party accounts required.

| Mode              | How it works                                                                                                                                                                                                       |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Solo Retro**    | RetroPie + EmulationStation on-device. NES through PlayStation 1 at 1080p upscaled. USB and Bluetooth gamepad support. No networking required.                                                                     |
| **Play Together** | RetroArch Netplay cross-device multiplayer. Each player's device renders the game locally — only controller input is synchronised, not video. Session invitations travel through your encrypted messaging channel. |

### 🎥 Conference
End-to-end encrypted video calling built on the Matrix protocol. 1:1 calls use Olm encryption. Group calls use Element Call with Megolm encryption. The server relays signalling only — call media is never visible to any intermediary. CalDAV calendar integration enables automatic meeting join by voice.

> **⚠️ Emergency Calling** — MyTV does not support E911 / 112 / 999. For emergencies, use a cellular phone or any PSTN-connected device.

### 🌐 Browser
Chromium in a TV-optimised shell with VP9 forced for YouTube 4K hardware decode. uBlock Origin and Privacy Badger pre-installed. DNS-over-HTTPS. Switches automatically to a standard desktop layout in Workstation mode. Designed for voice and remote navigation at comfortable viewing distance.

### ⚙️ Settings
Wi-Fi, display calibration, audio, user profiles, paired device management, presence detection controls, and system updates. Fully voice-accessible.

---

## Security Model

MyTV is a shared room device. Its security is calibrated for that environment — hardened without overreaching, honest about its boundaries.

- **Hardware root of trust** — The Infineon SLB9670 TPM 2.0 seals the LUKS disk encryption key against your verified boot chain. Removing the NVMe SSD and connecting it to a different machine does not yield the decryption key.
- **Verified boot** — Every boot stage is verified against the next. OTA packages are signed by a dedicated key held in an HSM under a multi-party quorum — no single person can produce a valid firmware update.
- **Signed A/B OTA updates** — Updates write to the standby partition. A failed update always leaves the previous working partition available. Users never lose a working system.
- **Application sandboxing** — AppArmor profiles restrict every media, conference, and gaming application to declared paths and resources. nftables default-deny inbound firewall. No SSH, no VNC, no Telnet in production firmware.
- **No wallet keys, no biometrics** — MyTV holds no cryptographic wallet keys and no biometric data. It never has, by design. These belong to personal devices and never traverse a shared room device.

---

## MyCloud Subscription

MyTV is fully functional with no account. The MyCloud subscription unlocks internet-dependent services at a single flat rate, covering all features across all your enrolled devices.

| Feature                                   | Without subscription | With MyCloud subscription |
|-------------------------------------------|----------------------|---------------------------|
| Local media, voice control, retro gaming  | ✅                   | ✅                        |
| Screen cast and local network features    | ✅ Local network     | ✅                        |
| Matrix E2EE video calling (1:1 and group) | —                    | ✅                        |
| MyCloud media sync (client-side E2EE)     | —                    | ✅                        |
| Cross-device Netplay via internet relay   | —                    | ✅                        |
| Encrypted settings and profile backup     | —                    | ✅                        |
| MyCloud App Store                         | —                    | ✅                        |
| OTA updates via MyCloud CDN               | Public repository    | MyCloud CDN               |

See [MyCloud](https://github.com/ccnfac/MyCloud) for more info.

---

<!--
## Key Features

- **Voice Interface**: Voice model enabling full voice control
- **Agent Architecture**: Action Agents handle diverse workflows
- **Multi-Mode Use**: Media center, conference station, and workstation all in one


## Hardware Stack

| Component                      | Description                               |
|--------------------------------|------------------------------------------ |
| **Camera**                     | Leopard Imaging LI-IMX179-USB-130H-CONN   |
| **Processor**                  | Raspberry Pi                              |
| **Audio Input**                | Yeti Nano Dual USB / Rode Wireless ME     |
| **Keyboard Options**           | Man & Machine keyboard / Rii wireless     |
| **Displays**                   | Sharp / NEC Display                       |


## Software Stack

- **OS**: Linux
- **Security**:
  - Secure Web Browser
- **Core Apps**:
  - TV
  - Media
  - Retro Games
  - Conference
  - Browser
  - Settings
- **Utilities**:
  - Built-in tuner
  - Parsec

## Apps Overview

### 📺 TV
- Core app for watching live television via tuner or IPTV streams.   
- Optimized for large-screen navigation with voice or remote control.  


### 🎵 Media
- Local and network-based media playback (music, videos, photos).  
- Unified media library with intelligent voice search.  


### 👾 Retro Games
- Emulator hub powered by RetroPie.  
- Preloaded with classics and supports adding custom ROMs.  
- Multiplayer support via Wi-Fi and mesh gaming.  


### 🎥 Conference
- Video conferencing solution.  
- Integrates with camera and microphone hardware.  
- Voice-controlled meeting join/start.  


### 🌐 Browser
- Secure, privacy-focused web browser.  
- Designed to make streaming services feel like native TV apps.  
- Full-screen optimized for remote and voice input.  


### ⚙️ Settings
- Control over Wi-Fi, display, sound, user accounts, and system updates.  
- Voice-enabled for accessibility.  
- Includes profile controls and privacy options.  


## Voice Workflow Orchestration

All system functions are accessible by voice through our agentic system:

```plaintext
[ You: ] "Open Retro Games"  
[ Agent: ] "Launching RetroPie Emulator..."  
[ You: ] "Switch to video conference"  
[ Agent: ] "Starting Meet session with camera and mic..."  
[ You: ] "Stream screen to living room TV"  
[ Agent: ] "Broadcasting via VNC to connected Sharp 4K display"
```

## Compatible Models

M Series Large Format Displays:

M431-2
M501-2
M551-2
M651-2
M751
M751-AVT3
M861
M861-AVT3
M981
M981-AVT3

MA Series Large Format Displays:

MA431
MA431-IR
MA431-MPi4E
MA431-PT
MA551
MA551-IR
MA551-MPi4E
MA551-PT

P Series Large Format Displays:

P435
P435-IR
P435-MPi4E
P435-PT
P495
P495-IR
P495-MPi4E
P495-PT
P555
P555-IR
P555-MPi4E
P555-PT
-->
