# SecuAuth AI - Complete Project Explainer

## 📌 Project Title
**SecuAuth AI** — Continuous Zero-Trust Authentication Layer  
**Problem Statement:** AI-Driven Detection of Synthetic Media and Phishing Attacks (SEBI TechSprint)

---

## 1. Executive Summary

SecuAuth AI is an **invisible, continuous biometric + cryptographic security layer** designed for securities trading platforms and broker portals.

Even if a hacker steals the user's password + OTP through phishing or social engineering, **SecuAuth AI silently verifies** that the real investor is physically present using their device's camera — without any extra steps for the user.

---

## 2. The Problem

- 90%+ account takeovers happen using stolen credentials (Password + OTP)
- Hackers use real-time phishing kits and man-in-the-middle attacks
- Current 2FA fails once the OTP is entered on the legitimate site
- Existing biometric solutions (Face ID, fingerprint) are visible and can be bypassed

---

## 3. The Solution: SecuAuth AI

A **silent background verification system** that activates after traditional login.

### Core Idea:
After the user enters Password + OTP, while the page shows "Processing...", the system silently:
1. Scans the user's face using the front camera
2. Generates a cryptographic signature
3. Verifies identity without sending any photo to the server

---

## 4. How It Works (Complete Working Flow)

The system works in two modes:

### A. For Websites → Chrome Extension
### B. For Mobile Apps → Security SDK

Below is the **complete architecture diagram** showing the full flow:

![Architecture Flow Diagram](images/flow-diagram.png)

---

## 5. Chrome Extension (For Web Portals)

### What it does:
- Runs in the background on broker websites (Zerodha, Groww, Upstox, etc.)
- Detects login or high-value transaction attempts
- Silently activates camera only when needed
- Signs the request using device private key

### How to Build (High-level):

1. **Manifest V3** Chrome Extension
2. Uses **MediaPipe Face Mesh** (runs fully in browser)
3. Uses **WebCrypto API** for key generation & signing
4. Injects into login pages using content scripts
5. Communicates with backend only using signed tokens

### Key Benefits:
- Minimal changes required on broker websites
- Works on any existing platform

---

## 6. Mobile SDK (For Broker Apps)

### What it does:
- Lightweight SDK that developers can integrate into their Android/iOS apps
- Uses native camera + secure enclave
- Handles liveness detection and cryptographic signing

### How to Build:
- **Android**: Kotlin + MediaPipe + Android Keystore
- **iOS**: Swift + Vision Framework + Secure Enclave
- Exposes simple APIs like:
  - `startSilentAuth()`
  - `verifyLiveness()`
  - `signChallenge()`

---

## 7. Backend Decision Matrix

| Condition       | Action                                      | Result                              |
|----------------|---------------------------------------------|-------------------------------------|
| **MATCH**      | Signature verified + Liveness passed        | Access Granted                      |
| **MISMATCH**   | Face doesn't match or Deepfake detected     | Session terminated + Intruder photo captured |
| **No Camera**  | User blocks camera                          | Access denied + Alert sent to owner |

---

## 8. Privacy & Security Highlights

- **No face image ever sent** to server
- Only **cryptographic signatures** are transmitted
- Keys are stored in **device Secure Enclave**
- Fully compliant with data privacy regulations

---

## 9. MVP Development Plan (Next Steps)

1. Build Chrome Extension (MVP)
2. Create simple demo website (mock broker login)
3. Implement face mesh + WebCrypto in extension
4. Build Node.js backend for signature verification
5. Demo two scenarios:
   - Legitimate user → Success
   - Different person → Intruder alert

---

## 10. Future Scope

- Add voice + typing behavior biometrics
- Integrate with UPI and banking apps
- Support for WebAuthn / Passkeys standard
- Regulatory sandbox testing with SEBI

---

**This document + the HTML explainer website together give a complete picture** of the project for judges, mentors, and future developers.