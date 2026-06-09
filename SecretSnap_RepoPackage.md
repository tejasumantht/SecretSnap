# SecretSnap — GitHub Repository Package

---

## 2. REPOSITORY DESCRIPTIONS

### Option A — Feature-led
```
A Python CLI tool that hides AES-256 encrypted messages inside PNG images using Least Significant Bit (LSB) steganography.
```

### Option B — Concept-led
```
Covert message embedding with AES-256-CBC encryption and LSB steganography — hide text in any PNG image, invisibly.
```

### Option C — Audience-led
```
SecretSnap: encrypt and conceal text messages within image pixel data using AES-256 and LSB steganography. CLI-based, cross-platform, offline.
```

### Short "About" Section
```
🔐 SecretSnap — Hide encrypted messages inside images using AES-256 steganography. Python · Pillow · PyCryptodome · CLI
```

---

## 3. GITHUB TOPICS

```
steganography
lsb-steganography
image-steganography
aes-256
aes-cbc
cryptography
encryption
python
python3
pillow
pycryptodome
cli-tool
privacy-tool
covert-communication
data-hiding
cybersecurity
information-security
image-processing
security-tools
portfolio-project
```

---

## 4. GITHUB BADGES

Paste into the top of your README.md:

```markdown
[![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![AES-256](https://img.shields.io/badge/Encryption-AES--256--CBC-4CAF50?style=for-the-badge&logo=gnuprivacyguard&logoColor=white)]()
[![Steganography](https://img.shields.io/badge/Technique-LSB%20Steganography-7B2D8B?style=for-the-badge)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Cross--Platform-lightgrey?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge&logo=github)](CONTRIBUTING.md)
[![Made with Python](https://img.shields.io/badge/Made%20with-Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)]()
```

---

## 5. PROJECT STRUCTURE

```
SecretSnap/
├── secretsnap.py          # Core application — all encoding, decoding, encryption, and CLI logic
├── requirements.txt       # Python dependencies
├── README.md              # Project documentation
├── LICENSE                # MIT License
├── .gitignore             # Python + image file exclusions
└── sample/
    ├── original.png       # Sample input image for demonstration
    └── original-enc.png   # Sample encoded output for demonstration
```

### Recommended `.gitignore`
```gitignore
# Python
__pycache__/
*.py[cod]
*.pyo
*.pyd
.Python
*.egg-info/
dist/
build/
venv/
.env

# Encoded output images (avoid committing test files)
*-enc.png

# OS
.DS_Store
Thumbs.db
```

---

## 6. REQUIREMENTS.TXT

```
Pillow>=9.0.0
pycryptodome>=3.15.0
colorama>=0.4.6
termcolor>=2.0.0
pyfiglet>=0.8.post1
rich>=13.0.0
```

> **Note:** `pycryptodome` and `pycrypto` conflict — do not install both. If you have `pycrypto` installed, run `pip uninstall pycrypto` before installing `pycryptodome`.

---

## 7. LICENSE RECOMMENDATION

### Recommended: MIT License

**Why MIT:**

- It is the most widely recognized permissive open-source license, appropriate for portfolio and personal projects.
- Allows anyone to use, modify, and distribute the code — maximizing the project's reach and contribution potential.
- Requires only that the original copyright notice is retained, keeping attribution simple.
- Well understood by recruiters and hiring managers reviewing GitHub profiles.

**Full MIT License text:**

```
MIT License

Copyright (c) 2024 Teja Sumanth

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 8. FUTURE IMPROVEMENTS

1. **Stronger Key Derivation (PBKDF2 / Argon2)**
   Replace the current single-pass SHA-256 key derivation with PBKDF2-HMAC-SHA256 (or Argon2id), including a random salt. This significantly increases the cost of any brute-force attempt against the password.

2. **Randomized Pixel Selection via Seeded PRNG**
   Instead of encoding sequentially from pixel (0,0), use a PRNG seeded by the derived key to scatter encoded pixels across the image. This makes the encoding pattern far less predictable.

3. **HMAC Message Authentication**
   Add an HMAC-SHA256 tag over the ciphertext before embedding. On decode, the HMAC is verified first, giving cryptographic assurance that the payload has not been altered.

4. **Graphical User Interface (GUI)**
   Build a Tkinter desktop GUI or a lightweight Flask/FastAPI web front-end with drag-and-drop image upload for users who prefer not to use the command line.

5. **Pipable CLI with Arguments**
   Package as a pip-installable tool with an `argparse` or `click`-based interface:
   ```bash
   secretsnap encode --image photo.png --message "Hello" --password mypass
   secretsnap decode --image photo-enc.png --password mypass
   ```

6. **Support for Additional Lossless Formats**
   Extend encoding/decoding support to BMP and TIFF — other lossless formats where LSB data survives saving.

7. **Large Message Splitting Across Multiple Images**
   For messages that exceed a single image's capacity, automatically split the payload across a set of images and reassemble them on decode.

8. **Unit Test Suite**
   Add a `tests/` directory with `pytest`-based unit tests covering encode/decode round-trips, password validation, header integrity checks, and edge cases (empty message, maximum capacity, wrong password).
