# Securities-Market-TechSprint
# SecuAuth AI - Continuous Zero-Trust Authentication Layer
> **SEBI TechSprint Submission** | Problem Statement 1: AI-Driven Detection of Synthetic Media and Phishing Attacks

---

## 📌 Project Overview
SecuAuth AI is an invisible, continuous biometric and cryptographic security layer designed for securities markets (Trading Platforms & Broker Portals). Even if a cybercriminal successfully steals a retail investor's password and OTP through advanced AI phishing or social engineering, SecuAuth AI stops the takeover by silently verifying the physical presence and liveness of the true investor at the edge.

---

## 🛠️ System Architecture & Workflow (How it Works)

This project bridges the Frontend (Client-side Edge Processing) and Backend (Cryptographic Verification Server) seamlessly without changing the core legacy authentication databases.

---

## 💻 Frontend Mechanics (Client-Side Edge)

The Frontend operates implicitly within the user's browser (Web Extension) or mobile application (Security SDK). It has two primary responsibilities: **Biometric Landmark Extraction** and **Liveness Filtering**.

### 1. Account Enrolment Phase
* **Facial Mesh Mapping:** When a user registers, the frontend uses `MediaPipe Face Mesh` to calculate a high-density 468-point 3D landmark mesh of the investor's face.
* **Key Generation (WebCrypto API):** The system feeds this numerical map into a localized cryptographic generator. It creates a **Device Private Key** (securely locked in the device's Secure Enclave) and a **Public Key** (sent to the broker's server). **No actual face photo is ever sent to the server.**

### 2. The Silent Authentication Phase
* **The Background Hook:** When an anomalous login or high-value transaction (large fund withdrawal/abnormal trade) is detected, the frontend page shows a standard "Processing your request..." loader.
* **Invisible Stream:** Underneath the loader graphical UI layer, the frontend opens a silent data stream from the front-facing camera.
* **Liveness Detection (Anti-Deepfake):** The algorithm monitors micro-movements, eye-blink frequency, and skin-reflectance variances. If a hacker holds up a photo, a tablet playing a video, or uses an AI deepfake stream, the frontend flags it as `SYNTHETIC_MEDIA_SPOOF` and kills the session.
* **Token Signing:** If the liveness test passes, the frontend captures the temporary landmark coordinates, signs a server challenge using the local **Private Key**, and dispatches the signature to the backend.

---

## ⚙️ Backend Mechanics (Verification Server)

The Backend operates as a high-performance validation middleware that acts on zero-trust principles.

### 1. Decentralized Signature Verification
* **Decryption & Match:** The backend server receives the validation token and signature from the frontend. It fetches the investor's registered **Public Key** from the broker's secure DB.
* **Cryptographic Verification:** The server uses standard asymmetric cryptography (ECDSA/RSA-4096) to verify the signature. Since the server only validates signatures, a server-side leak will never compromise user biometrics.

### 2. Advanced Threat Response Engine (Decision Matrix)
* **Condition: MATCH**
  * The backend validates the signature, authorizes the session token, and instructs the frontend to route the legitimate user to their trading dashboard or approve the fund transfer instantly.
* **Condition: MISMATCH / DEEPFAKE DETECTED**
  * **Session Termination:** The backend immediately invalidates the stolen OTP session, blocking account entry.
  * **Intruder Capture:** The backend signals the frontend to silently snapshot the intruder’s face from the camera stream.
  * **Investor Mobilization Pipeline:** The intruder's photo is sent to an independent out-of-band security log. Instantly, an urgent Push Notification/SMS alert is dispatched to the true investor's primary trusted device, showing the hacker's photo with a single-tap option to permanently freeze the entire trading profile.

---

## 🚀 Key Technical Highlights for Intermediaries
* **Zero Friction UX:** Legitimate users perform no extra authentication steps; security runs entirely in the background.
* **Phishing Immunity:** Stolen OTPs become useless because the adversary cannot replicate the localized hardware key and physical biometric signature combination.
* **Regulatory Compliance:** Fully satisfies strict data privacy mandates since sensitive biometric images are never centralized on cloud databases.
