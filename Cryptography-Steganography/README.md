# BLACKOUT CTF — Cryptography & Steganography

A collection of cryptography and steganography challenges solved during BLACKOUT CTF.

This write-up documents the investigation process, tools, techniques, and lessons learned from four challenges.

## Competition Overview

- **Competition:** BLACKOUT CTF
- **Category:** Cryptography & Steganography
- **Challenges Solved:** 4
- **Total Points:** 430
- **Author:** Gabriel Nzioka
- **Focus Areas:** Binary encoding, JPEG steganography, audio analysis, Morse code, and data decoding

## Challenges

| Challenge | Points | Technique |
|---|---:|---|
| Burning Inside | 100 | JPEG steganography |
| Binary Shadows | 200 | Binary decoding |
| Encrypted Echoes | 50 | Audio analysis and Morse code |
| Hidden Message | 80 | Steganography and string reversal |

## Table of Contents

1. [Burning Inside](#1-burning-inside)
2. [Binary Shadows](#2-binary-shadows)
3. [Encrypted Echoes](#3-encrypted-echoes)
4. [Hidden Message](#4-hidden-message)

---

## 1. Burning Inside

**Points:** 100  
**Technique:** JPEG steganography using Steghide

## 2. Binary Shadows

**Points:** 200  
**Technique:** Binary-to-ASCII decoding using Python

## 3. Encrypted Echoes

**Points:** 50  
**Technique:** Audio signal analysis and Morse code decoding

## 4. Hidden Message

**Points:** 80  
**Technique:** Steganography and reverse-string decoding

---

## Tools Used

- Kali Linux
- Steghide
- Binwalk
- ExifTool
- Python 3
- NumPy
- Audacity
- SoX

## Key Takeaways

- Inspect file metadata before attempting complex analysis.
- Understand binary encoding and character representations.
- Analyze audio signals to identify hidden communication.
- Use steganography tools to investigate embedded data.
- Document findings and verify recovered information.

## Disclaimer

These writeups document challenges completed in an authorized Capture The Flag environment. The techniques are intended for cybersecurity education and authorized investigations.
