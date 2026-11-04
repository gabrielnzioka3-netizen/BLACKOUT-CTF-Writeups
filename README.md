# BLACKOUT CTF — Web Exploitation Writeups

A collection of web exploitation challenges solved during BLACKOUT CTF.

## Competition Overview

- **Category:** Web Exploitation
- **Challenges Solved:** 4
- **Total Points:** 750
- **Author:** Gabriel Nzioka
- **Focus Areas:** Web security, reconnaissance, authentication, and application logic

## Table of Contents

1. [1024 Redirects](#1024-redirects)
2. [CASE Money Transfer](#case-money-transfer)
3. [Cookie](#cookie)
4. [Source of All Problems](#source-of-all-problems)

---

## 1024 Redirects

**Points:** 150  
**Author:** 0x4m4  
**Flag:** `NEX{th1s_captcha_c0uldnt_tcha}`

### Challenge Overview

The challenge requires solving a CAPTCHA 1024 times consecutively before the server reveals the flag.

### Vulnerability

The server returns the next CAPTCHA as plain text in the HTML response. This makes it possible to automate the challenge by extracting each CAPTCHA and submitting it in the next request.

### Solution

The solution uses Python and the Requests library to automate the sequence.

### Key Takeaway

Always inspect HTTP responses and application behavior before assuming that a CAPTCHA requires OCR or manual interaction.

---

## CASE Money Transfer

**Points:** 300  
**Author:** 0x4m4  
**Flag:** `NEX{g1ve_m3_my_mon3y_b4ck}`

### Challenge Overview

A mock banking application requires a balance of 1000 before the flag can be accessed.

### Vulnerabilities

- Stored Cross-Site Scripting (XSS)
- Inadequate protection against same-origin malicious actions

### Key Takeaway

CSRF tokens do not protect an application against malicious JavaScript executing within the application's trusted origin.

---

## Cookie

**Points:** 200  
**Author:** 0x4m4  
**Flag:** `SSCIT{hat3l_di_l1k3s_c00ki3s}`

### Challenge Overview

The application uses a client-controlled cookie to determine whether a user is an administrator.

### Vulnerability

The application trusts the `admin` cookie without cryptographic verification or server-side authorization.

### Key Takeaway

Authorization decisions must not rely on untrusted client-controlled values.

---

## Source of All Problems

**Points:** 100  
**Author:** 0x4m4  
**Flag:** `NEX{i_w0uld_1ov3_t0_ch4ng3_th3_w0r1d_but_th3y_w0nt_giv3_m3_th3_50urc3_c0d3}`

### Challenge Overview

The challenge hides fragments of a flag throughout publicly accessible website resources.

### Vulnerability

Sensitive information is exposed through HTML comments, JavaScript comments, CSS comments, and publicly accessible files.

### Key Takeaway

Publicly accessible resources can expose sensitive information. Proper reconnaissance and source inspection are essential during web security assessments.

---

## Disclaimer

These writeups document challenges completed in an authorized CTF environment. The techniques are intended for cybersecurity education and authorized security testing.
