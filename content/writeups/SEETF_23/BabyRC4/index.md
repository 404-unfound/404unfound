---
title: "[SEETF'23] BabyRC4"
date: 2023-06-26T10:24:19+08:00
author: "Kairos"
tags:
- Python
- crypto
ctfs:
- SEETF'23
---

## SEETF'23: BabyRC4

This was one of the "easier" crypto challenge in SEETF, using the ARC4 (an implementation of RC4 cipher, a symmetric stream cipher).

## The Challenge

```py
from Crypto.Cipher import ARC4
from os import urandom
key = urandom(16)
flag = b'SEE{?????????????????????????????????}'[::-1]

def enc(ptxt):
    cipher = ARC4.new(key)
    return cipher.encrypt(ptxt)

print(f"c0 = bytes.fromhex('{enc(flag).hex()}')")
print(f"c1 = bytes.fromhex('{enc(b'a'*36).hex()}')")

"""
c0 = bytes.fromhex('b99665ef4329b168cc1d672dd51081b719e640286e1b0fb124403cb59ddb3cc74bda4fd85dfc')
c1 = bytes.fromhex('a5c237b6102db668ce467579c702d5af4bec7e7d4c0831e3707438a6a3c818d019d555fc')
"""
```


## Explanation

Looking at the code, the challenge involves requiring to decrypt the flag value by reversing the encryption process. It gives two ciphertexts: `c0` and `c1`. The objective was to decrypt the flag, represented as `b'SEE{?????????????????????????????????}'[::-1]`.

In essense, the ARC4 XORs each byte of the plaintext with a corresponding byte from a pseudorandom key (generated from the `key` variable). XOR is a binary operation which compares two  binary values (plaintext and key) to produce an encrypted result. Read more [here](https://www.101computing.net/xor-encryption-algorithm/).

But lets look closer at the code.
1. First, the code generates a random 16-byte (128-bit) key using the `urandom` function.
2. The `enc` function is defined to encrypt the plaintext with the ARC4 algorithm with the generated key, taking the plaintext as the input, initialises an ARC4 cipher with the key, then encrypts the plaintext.
3. Two ciphertexts, c0 (result of the flag being encrytped) and c1 (result of 36 'a's being encrypted), are provided in hexadecimal format, which are the results of encrypting different plaintexts **using the same key**.


## Solution

1. Knowing that the two ciphertexts are encrypted with the same key, the first step was to XOR  `c0` and `c1` byte by byte to obtain the keystream. 

```py
c0 = bytes.fromhex('b99665ef4329b168cc1d672dd51081b719e640286e1b0fb124403cb59ddb3cc74bda4fd85dfc')
c1 = bytes.fromhex('a5c237b6102db668ce467579c702d5af4bec7e7d4c0831e3707438a6a3c818d019d555fc')

# XOR c0 and c1 
keystream = bytes(c0_byte ^ c1_byte for c0_byte, c1_byte in zip(c0, c1))
```

2. The next step was to recover the key. With the keystream in hand, and knowing that `c1` was XOR-ed using a sequence of 'a's, we could XOR the keystream with 'a's to cancel out the keystream. This gave us our key!
```py
key = bytes(keystream_byte ^ ord('a') for keystream_byte in keystream)
```

3. Now that we have our key, I converted it to hexadecimal, printed it out to take a look at it, and then assigned a variable `recovered_key_hex` 
```py
print("Recovered key:", key.hex())
recovered_key_hex = '7d35333832656661633a733573733579336b5f3443725f33355565725f724576336e7b45'
```
4. With the recovered key in hexadecimal, we could now recover the key in ascii and reverse it to retrieve the key. I also had to append the `SE` as the prefix to the flag, assuming that it wasn't added during encryption.
```py
import binascii
recovered_key = recovered_key[::-1]  # Reverse the key
flag = "SE" + recovered_key.decode('ascii')

print("Flag:", flag)

```

And the flag I got was `SEE{n3vEr_reU53_rC4_k3y5ss5s:cafe2835}`.