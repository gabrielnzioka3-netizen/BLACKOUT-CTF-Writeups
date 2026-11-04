# BLACKOUT CTF — Digital Forensics Writeups

A collection of digital forensics challenges solved during BLACKOUT CTF, documenting the investigation process, tools used, technical findings, and lessons learned.

## Competition Overview

- **Competition:** BLACKOUT CTF
- **Category:** Digital Forensics
- **Challenges Solved:** 4
- **Total Points:** 600
- **Author:** Gabriel Nzioka
- **Focus Areas:** File analysis, steganography, metadata analysis, network forensics, and data reconstruction

## Challenges

| Challenge | Points | Technique |
|---|---:|---|
| Bitwise Mystery | 100 | Bitwise transformation |
| Flag (Blacked-Out Image) | 100 | JPEG metadata analysis |
| Osama's Obscured Secrets | 150 | DOCX metadata analysis |
| Fragmented | 250 | PCAP analysis and protocol reconstruction |

## Table of Contents

1. [Bitwise Mystery](#1-bitwise-mystery)
2. [Flag — Blacked-Out Image](#2-flag-blacked-out-image)
3. [Osama's Obscured Secrets](#3-osamas-obscured-secrets)
4. [Fragmented](#4-fragmented)

---

## 1. Bitwise Mystery

**Points:** 100

### Challenge Overview

The challenge provides a file containing seemingly unreadable text. The objective is to identify the transformation applied to the bytes and recover the original message.

### Investigation

The analysis focuses on identifying repeated byte patterns and determining whether a bitwise transformation was applied.

### Tools

- Python 3
- Linux command-line utilities

### Key Takeaway

Understanding byte-level transformations and character encoding is important when investigating unusual files.

---

## 2. Flag — Blacked-Out Image

**Points:** 100

### Challenge Overview

A JPEG image contains a partially obscured flag. The objective is to investigate the image and determine whether the hidden information can be recovered.

### Investigation

The analysis examines the image's metadata and embedded information.

### Tools

- ExifTool
- Python
- Pillow

### Key Takeaway

Image metadata may contain information that is not visible in the rendered image.

---

## 3. Osama's Obscured Secrets

**Points:** 150

### Challenge Overview

A DOCX document contains a short message, but the flag is hidden elsewhere within the document.

### Investigation

The document is examined as a ZIP archive, and its XML metadata is inspected.

### Tools

- unzip
- Python
- XML inspection

### Key Takeaway

Office documents contain multiple internal components, including metadata that may not appear in the document's main body.

---

## 4. Fragmented

**Points:** 250

### Challenge Overview

A PCAP file contains several fragments of a flag distributed across different network protocols.

### Investigation

The network capture is analyzed to identify HTTP, DNS, and TLS traffic and reconstruct the available information.

### Tools

- Wireshark
- TShark
- Scapy
- Python

### Key Takeaway

Network forensics requires understanding protocols, packet contents, encryption, and the limitations of available evidence.

---

## Disclaimer

These writeups document challenges completed in an authorized Capture The Flag environment. The techniques are intended for cybersecurity education and authorized forensic investigations.
