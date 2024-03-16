---
title: "[Cyber Apoc '24] Dynastic"
date: 2024-03-16T19:49:17+08:00
authors:
  - Kairos
tags:
  - Crypto
ctfs:
  - CyberApoc'24
---

# [Cyber Apoc '24] Dynastic

This was a crypto challenge where we are given a Python script (encryption algo) and the output text file (cipher). The
given files can be found [here](crypto_dynastic.zip).

## Description

> You find yourself trapped inside a sealed gas chamber, and suddenly, the air is pierced by the sound of a distorted
> voice played through a pre-recorded tape. Through this eerie transmission, you discover that within the next 15
> minutes,
> this very chamber will be inundated with lethal hydrogen cyanide. As the tape’s message concludes, a sudden mechanical
> whirring fills the chamber, followed by the ominous ticking of a clock. You realise that each beat is one step closer
> to
> death. Darkness envelops you, your right hand restrained by handcuffs, and the exit door is locked. Your situation
> deteriorates as you realise that both the door and the handcuffs demand the same passcode to unlock. Panic is a luxury
> you cannot afford; swift action is imperative. As you explore your surroundings, your trembling fingers encounter a
> torch. Instantly, upon flipping the switch, the chamber is bathed in a dim glow, unveiling cryptic letters etched into
> the walls and a disturbing image of a Roman emperor drawn in blood. Decrypting the letters will provide you the key
> required to unlock the locks. Use the torch wisely as its battery is almost drained out!

## Analysis

The given Python script is as below:

```python
from secret import FLAG
from random import randint

def to_identity_map(a):
    return ord(a) - 0x41

def from_identity_map(a):
    return chr(a % 26 + 0x41)

def encrypt(m):
    c = ''
    for i in range(len(m)):
        ch = m[i]
        if not ch.isalpha():
            ech = ch
        else:
            chi = to_identity_map(ch)
            ech = from_identity_map(chi + i)
        c += ech
    return c

with open('output.txt', 'w') as f:
    f.write('Make sure you wrap the decrypted text with the HTB flag format :-]\n')
    f.write(encrypt(FLAG))
```

Looking at the script, we can see that the function operates on each character of the flag. Let's break down the
algorithm first. The `to_identity_map` function takes in a character and returns its position in the alphabet (A -> 0,
B -> 1, ..., Z -> 25). The `from_identity_map` function does the reverse, taking in a number and returning the character
at that position in the alphabet. So essentially, the steps during encryption are:

1. Character is converted to a number using `to_identity_map`
2. The number is incremented by its position in the flag if it is an alphabet
3. The result is converted back to a character using `from_identity_map`
4. The result is appended to the ciphertext

This is repeated for each character in the flag. Note that the `randint` function is not used in the encryption process!

## Getting the Flag

Reversing the algorithm is fairly simple; we literally just need to do the reverse of each step. My solve script is as
follows:

```python
def to_identity_map(a):
    return ord(a) - 0x41

def from_identity_map(a):
    return chr(a % 26 + 0x41)

def decrypt(ciphertext):
    m = ''
    for i in range(len(ciphertext)):
        ch = ciphertext[i]
        if not ch.isalpha():
            m += ch
        else:
            chi = to_identity_map(ch)
            m += from_identity_map(chi - i)
    return m

encrypted_flag = "DJF_CTA_SWYH_NPDKK_MBZ_QPHTIGPMZY_KRZSQE?!_ZL_CN_PGLIMCU_YU_KJODME_RYGZXL"

decrypted_flag = decrypt(encrypted_flag)

print(decrypted_flag)
```

This iterates through each character of the ciphertext and adds its position in the flag rather than subtracting it. The output is `DID_YOU_KNOW_ABOUT_THE_TRITHEMIUS_CIPHER?!_IT_IS_SIMILAR_TO_CAESAR_CIPHER`; wrapping it in `HTB{}` gives us the flag :D