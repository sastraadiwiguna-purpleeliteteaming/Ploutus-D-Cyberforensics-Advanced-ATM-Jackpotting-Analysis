[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.18278682-blue?logo=zenodo&logoColor=white)](https://doi.org/10.5281/zenodo.18278682) = [doi.org/10.5281/zenodo.18278682](https://doi.org/10.5281/zenodo.18278682)

[![ORCID](https://img.shields.io/badge/ORCID-0009--0007--7728--256X-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0009-0007-7728-256X) = [orcid.org/0009-0007-7728-256X](https://orcid.org/0009-0007-7728-256X)

---

# Ploutus-D: Advanced ATM Jackpotting Malware – Forensic Architecture & Mitigation

## 📖 1. Executive Summary

**Ploutus-D** (a variant of the Ploutus family) is a highly specialized ATM malware designed to target **Diebold Nixdorf** and **KAL Kalignite** multi-vendor software suites. Unlike generic malware, Ploutus-D leverages the **XFS (eXtensions for Financial Services)** middleware to issue direct commands to the cash dispenser, bypassing traditional banking transaction authorizations.

This repository contains a comprehensive cyberforensic breakdown of the malware’s architecture, its obfuscation methods, and the specific artifacts left during the 2025 "Tren de Aragua" campaign.

---

## 🛠 2. Technical Architecture & Binary Analysis

The malware is primarily developed in **Chttps://www.google.com/search?q=%23 (.NET Framework)** and protected using the **.NET Reactor** obfuscator to impede static analysis and decompilation.

### 2.1 Core Components (File System View)

| Component | Filename (Typical) | Functionality |
| --- | --- | --- |
| **The Launcher** | `Diebold.exe` | Manages persistence, kills security services, and hooks the keyboard. |
| **The Payload** | `AgilisConfigurationUtility.exe` | The "Brain" that communicates with the XFS Manager to dispense cash. |
| **XFS Abstraction** | `K3A.Platform.dll` | Stolen KAL Kalignite library used for hardware-level communication. |
| **Configuration** | `edclocal.dat` | Encrypted configuration file containing C2 parameters and SMS keys. |

### 2.2 Execution Flow

1. **Initial Vector**: Physical breach of the ATM "top-hat" to access USB/PS2 ports.
2. **Service Deployment**: The launcher installs itself as a Windows Service named `DIEBOLDP`.
3. **Environment Sanitization**: Every 5 seconds, the malware terminates `NHOSTSVC.exe` and `XFSConsole.exe` to disable monitoring.
4. **Log Erasure**: Actively wipes `NetOp.LOG` to prevent forensic recovery of transaction anomalies.

---

## 🔍 3. Cyberforensic Indicators (IoCs)

### 3.1 Host-Based Artifacts

* **Registry Modifications**:
* `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit` — Modified to ensure the malware survives reboots.


* **File System**:
* Check for hidden directories: `C:\Diebold\EDC\`
* Look for the "Signature" file: `P.bin` (contains the string: `PLOUTUS-MADE-IN-LATIN-AMERICA-XD`).



### 3.2 Network/C2 Indicators

* While primarily standalone, variants utilize **SMS-based activation**. Look for attached GSM hardware or unauthorized COM port activity.

---

## ⌨️ 4. Operator Interface (The "Jackpotting" Trigger)

Ploutus-D does not have a GUI. It relies on a **Global Keyboard Hook** (`SetWindowsHookEx`) to listen for specific F-Key sequences:

* **[F3]**: Activates the dispensing sequence.
* **[F4]**: Emergency termination of the malware process.
* **Numeric Keypad**: Used to input the specific quantity of banknotes to be dispensed (e.g., entering `2000` commands the dispenser to release 2000 bills).

---

## 🛡️ 5. Mitigation & Defensive Strategy

To defend against Ploutus-D, financial institutions must implement a **Holistic Defense-in-Depth** model:

1. **Physical Hardening**: Implement specialized top-hat sensors and proprietary locks.
2. **Full Disk Encryption (FDE)**: Prevents "HDD swapping" or offline injection of binaries.
3. **Device Control**: Disable all unauthorized USB/PS2 ports via BIOS/UEFI and OS-level policies.
4. **Trusted Platform Module (TPM)**: Ensure Secure Boot is enabled to prevent unauthorized OS loaders.
5. **XFS Layer Security**: Use encrypted XFS communication (if supported by hardware) to prevent "Man-in-the-Middle" attacks between the PC and the Dispenser.

---

## ⚠️ 6. Legal & Ethical Disclaimer

**STRICT_GEMINI_AI_ABSOLUTE_RULESET: ENABLED**
This documentation is for **educational and authorized Red-Teaming purposes only**. All code snippets and forensic analysis provided are replicated from reverse-engineered samples (MD5 verified). Deployment of these techniques on production ATMs without explicit legal authorization constitutes a federal crime under **18 USC § 1030 (Computer Fraud and Abuse Act)** and international cybercrime laws.

---

**Research Lead:** `SASTRA_ADI_WIGUNA [PURPLE_ELITE_TEAMING]`



