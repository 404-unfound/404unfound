---
title: "[UofTCTF'24] repeat"
date: 2024-01-15T14:21:56+08:00
authors:
  - Kairos
tags:
  - crypto
ctfs:
  - UofTCTF'24
---

## UofTCTF'24: repeat
This was a standard XOR challenge, where we are given the ciphertext and the encryption algorithm. We then need to get the plaintext, i.e. the flag.

## The Challenge

```
I'm a known repeat offender when it comes to bad encryption habits. But the secrets module is secure, so you'll never be able to guess my key!
```

We are given two files: `gen.py` and `flag.enc`.

`gen.py`:
```python
import os
import secrets

flag = "REDACATED"
xor_key = secrets.token_bytes(8)

def xor(message, key):
    return bytes([message[i] ^ key[i % len(key)] for i in range(len(message))])

encrypted_flag = xor(flag.encode(), xor_key).hex()

with open("flag.enc", "w") as f:
    f.write("Flag: "+encrypted_flag)
```

`flag.enc`:

```
Flag: 982a9290d6d4bf88957586bbdcda8681de33c796c691bb9fde1a83d582c886988375838aead0e8c7dc2bc3d7cd97a4
```

## The Analysis
We know a few things:
1. The flag is encrypted using XOR.
    - XOR can be reversed by XORing the ciphertext with the key to get the plaintext.
2. The key is a random 8-byte string.
3. The flag likely starts with `uoftctf{`. 

## The Solution
To solve this challenge, we need to:
1. Get the key. 
    Knowing that the starting plaintext of the flag, and that the key is 8 bytes long, we can get all 8 bytes of the key by XORing the first 8 bytes of the ciphertext with `uoftctf{`.
2. Decrypt the ciphertext.
    With the key, we can XOR the ciphertext with the key to get the plaintext.

To simplify the process, I wrote a python script to do the above steps.

```python
def xor_key(known, encrypted_part):
    return bytes([known[i] ^ encrypted_part[i] for i in range(len(known))])

def decrypt(cipher, xor_key):
    encrypted_bytes = bytes.fromhex(cipher)
    decrypted_bytes = bytes([encrypted_bytes[i] ^ xor_key[i % len(xor_key)] for i in range(len(encrypted_bytes))])
    return decrypted_bytes.decode()

known = "uoftctf{"

with open("flag.enc", "r") as f:
    cipher = f.read().split(" ")[1]

key_length = len(known)

known_encrypted_part = cipher[:key_length * 2]

xor_key = xor_key(known.encode(), bytes.fromhex(known_encrypted_part))

pt = decrypt(cipher, xor_key)

print(pt)
```

While not the most elegant solution, it can nicely solve the challenge.

```
uoftctf{x0r_iz_r3v3rs1bl3_w17h_kn0wn_p141n73x7}
```