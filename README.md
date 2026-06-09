<div align="center">

<h1>🔐 SecretSnap</h1>

<p><strong>Hide messages in plain sight — AES-256 encrypted image steganography from the terminal.</strong></p>

[![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![AES-256](https://img.shields.io/badge/Encryption-AES--256--CBC-4CAF50?style=for-the-badge&logo=gnuprivacyguard&logoColor=white)]()
[![Steganography](https://img.shields.io/badge/Technique-LSB%20Steganography-7B2D8B?style=for-the-badge)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Cross--Platform-lightgrey?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()

<br/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=16&pause=1000&color=00C8FF&center=true&vCenter=true&width=500&lines=Encrypt+a+message.+Hide+it+in+a+photo.+Share+freely." alt="Typing SVG" />

</div>

---

## 📌 Overview

**SecretSnap** is a command-line steganography tool that conceals text messages inside ordinary PNG images. It works in two layers: first, the message is optionally encrypted with **AES-256-CBC**; then it is invisibly embedded into the pixel data of an image using **Least Significant Bit (LSB)** steganography.

The output image is visually indistinguishable from the original. Only someone using SecretSnap with the correct password can retrieve and read the hidden message.

---

## ✨ Features

- **LSB Steganography** — Encodes message bits into the least significant channel of each RGB pixel value, producing no visible change to the image
- **AES-256-CBC Encryption** — Optionally encrypts the message before embedding; key is derived from a user password using SHA-256
- **Integrity Verification** — A header token is prepended to the payload to detect tampered images and incorrect passwords cleanly
- **Automatic RGB Conversion** — RGBA images (transparent PNGs) are automatically converted to RGB before encoding
- **Non-Destructive Output** — Saves the encoded image as a new file (`<name>-enc.png`); the original is never modified
- **Secure Password Input** — Uses `getpass` so the password is never echoed to the terminal
- **Rich Terminal UI** — Color-coded output, status spinners, and a styled ASCII banner via `rich`, `colorama`, and `pyfiglet`

---

## 🔬 How It Works

### Encoding

```
Message + Password
       │
       ▼
[AES-256-CBC Encryption]  ← optional, skipped if no password
       │
       ▼
Prepend Header Token  →  "M6nMjy5THr2J" + payload
       │
       ▼
For each character in payload:
  → Convert to 8-bit binary
  → Read 3 consecutive pixels (9 RGB channel values)
  → Force LSB of channels 1–8 to match each binary digit
  → Set channel 9 as stop-flag: odd = end, even = continue
  → Write pixels back to image
       │
       ▼
Save as <filename>-enc.png
```

### Decoding

```
Load encoded image
       │
       ▼
Read LSBs of 3-pixel groups → reconstruct characters → stop at sentinel
       │
       ▼
Validate outer header token
       │
       ▼
[AES-256-CBC Decrypt]  ← if password provided
       │
       ▼
Validate inner header token  →  confirm correct password
       │
       ▼
Output hidden message
```

---

## 🛠 Technology Stack

| Component        | Library / Tool                        |
|------------------|---------------------------------------|
| Language         | Python 3.7+                           |
| Image Processing | Pillow (PIL)                          |
| Cryptography     | PyCryptodome (AES-256-CBC, SHA-256)   |
| IV Generation    | `Crypto.Random` (cryptographic random)|
| Encoding         | `base64` (ciphertext transport)       |
| Terminal UI      | Rich, Colorama, Termcolor, Pyfiglet   |
| Password Input   | `getpass` (masked input)              |

---

## ⚙️ Installation

### Prerequisites

- Python 3.7+
- pip

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/tejasumantht/SecretSnap.git
cd SecretSnap

# 2. Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt
```

---

## 🚀 Usage

```bash
python secretsnap.py
```

You will be prompted to choose between encoding and decoding.

### Encode a Message

```
Choose one:
1. Encode
2. Decode
>> 1

Image path (with extension):
>> photo.png

Message to be hidden:
>> The documents are in the blue folder.

Password to encrypt (leave empty for no password):
>> ••••••••

Re-enter Password:
>> ••••••••
```

✅ Output: `photo-enc.png` — visually identical to `photo.png`, with the message hidden inside.

---

### Decode a Message

```
Choose one:
1. Encode
2. Decode
>> 2

Image path (with extension):
>> photo-enc.png

Enter password (leave empty if no password):
>> ••••••••
```

✅ Output:
```
Decoded Text:
The documents are in the blue folder.
```

---

## 💡 Example Workflow

```bash
# Alice encodes a message for Bob
python secretsnap.py
→ Encode
→ Image: landscape.png
→ Message: "Package delivered. Locker 42."
→ Password: [shared secret]
→ Output: landscape-enc.png

# Alice shares landscape-enc.png through any channel
# It looks like an ordinary landscape photo.

# Bob decodes it
python secretsnap.py
→ Decode
→ Image: landscape-enc.png
→ Password: [shared secret]
→ Output: "Package delivered. Locker 42."
```

> 💡 **Capacity:** Each character requires 3 pixels. A 1920×1080 image supports up to ~691,200 characters.

---

## 📁 Project Structure

```
SecretSnap/
├── secretsnap.py          # Core application — encoding, decoding, encryption, CLI
├── requirements.txt       # Python dependencies
├── README.md              # Project documentation
├── LICENSE                # MIT License
└── sample/
    ├── original.png       # Sample input image
    └── original-enc.png   # Sample encoded output
```

---

## 🚧 Future Improvements

- [ ] **Stronger Key Derivation** — Integrate PBKDF2-HMAC-SHA256 or Argon2 for password-based key generation
- [ ] **Randomized Pixel Selection** — Use a seeded PRNG to scatter encoded pixels non-sequentially for added concealment
- [ ] **Message Authentication** — Add HMAC-SHA256 to verify payload integrity on decode
- [ ] **GUI Interface** — Build a Tkinter or web-based front-end for non-CLI users
- [ ] **Multi-Format Support** — Extend to BMP and TIFF (other lossless formats)
- [ ] **Large Message Splitting** — Distribute payloads across multiple images for long messages
- [ ] **Pipable CLI** — Package as a pip-installable tool with `secretsnap encode --image X --message Y` syntax
- [ ] **Batch Processing** — Encode/decode multiple images in a single command

---

## 👤 Author

**Teja Sumanth**

Incoming M.Sc. Cybersecurity Student · Paderborn University, Germany

[![GitHub](https://img.shields.io/badge/GitHub-tejasumantht-181717?style=flat-square&logo=github)](https://github.com/tejasumantht)
[![Email](https://img.shields.io/badge/Email-tejasumanth.t%40gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:tejasumanth.t@gmail.com)

---

<div align="center">

*Built with Python, pixels, and a passion for privacy.*

</div>
