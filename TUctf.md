# TU CTF

Hosted by the [University of Tulsa CTF club](https://tuctf.com/) on January 24th - January 26th.

This is my first CTF of the year and I competed alone. While it was open for 48hr, I mainly worked Friday evening and a bit on Saturday afternoon. This was a good start to the year for me, as I was able to complete 7 challenges and ended up in 165th of the 485 people who completed a challenge. 

![CTF_placement](https://github.com/user-attachments/assets/b81852af-0dde-4875-a02b-f2d436f47d61)

- Polyglot files: [Mystery Presentation](#Mystery-Presentation)
- Pcap file: [Packet Detective](#Packet-Detective) 
- 802.11 Cap file: [Security Rocks](#Security-Rocks)
- XOR: [My First Encryption](#My-First-Encryption)
- EVTX file: [Files in the Web](#Files-in-the-Web)
- XOR: [XOR-Dinary](#XOR-Dinary)
- Ghidra RE: [Mystery Box](#Mystery-Box)

## Forensics

### Mystery Presentation

![mysteryPresentation](https://github.com/user-attachments/assets/821415d1-1ce8-4890-a195-e09ea217f785)


While given a .pptx file type, clicking on the file to open it opened a folder of many documents. 

Using `file` we could see that it was a zip archive. 

Exploring the zip archive exposed a file named `flag.txt` which contained the flag. 

<details>
<summary>Flag</summary>
<br>
`TUCTF{p01yg10+_fi1e5_hiddin9_in_p1@in_5i9h+}`
</details>

### Packet Detective

![packetDetective](https://github.com/user-attachments/assets/38ed532d-41bb-4f35-9782-ad83724923a1)

Given a pcap file, I analyzed it in Wireshark. 

There were only 100 packets captures, so I started by simply scrolling through and looking at the data of each packet to see if any strings popped out. This exposed the flag in plaintext on the 100th packet. 

<details>
<summary>Flag</summary>
<br>
`TUCTF{N3tw0rk_M4st3r}`
</details>

### Security Rocks

![securityRocks](https://github.com/user-attachments/assets/5f1181cc-a223-4604-942f-228b1863f892)

Big thanks to a 2021 writeup on a pcap file with protocol 802.11 by [Phasetw0](https://phasetw0.com/writeups/uiuctf2021_ceo/). 

[Wireshark docs](https://wiki.wireshark.org/HowToDecrypt802.11)

Decode `1KZTi2ZV7tO6yNxslvQbjRGL54BsPVyskwv4QaR29UMKj` on [Dcode]() as --- to get our flag. 

<details>
<summary>Flag</summary>
<br>
`TUCTF{w1f1_15_d3f1n173ly_53cure3}`
</details>

## Cryptography

### My First Encryption

![firstEncryption](https://github.com/user-attachments/assets/3a28bc99-f9fc-4da6-8762-60d28f982a2a)

The challenge gave a `flag.jpeg` file and the hint that xor was used. When the image was opened, it gave the warning that this was not a jpeg file as it starts with `0xCF 0xE8`, instead of `0xFF 0xD8`.

Using an [online XOR calculator](https://xor.pw/#), the difference is `0x30`, so I wrote a simple python script to read the file and xor against the value. The output image contained the flag.


```python
def xor_file(input_file, output_file, xor_value):
    with open(input_file, "rb") as f:
        data = f.read()
    
    xor_data = bytearray([b ^ xor_value for b in data])
    
    with open(output_file, "wb") as f:
        f.write(xor_data)

xor_file("flag.jpeg", "output.jpg", 0x30)
```

<details>
<summary>Flag</summary>
<br>
`TUCTF{kn0wn_pl@1nt3xt_15_dang3r0us}`
</details>

### Files in the Web

![fileWeb](https://github.com/user-attachments/assets/40cb95bc-8202-4378-a057-f1ac2efcf1fe)

```python
from cryptography.fernet import Fernet

key = b"UIYfpIrqvzvTedjR1qFm66K1MYYlwNgUQlgpZPfLs3k="
ciphertext = b"gAAAAABnSr5MS-gH-RLqtV1ltw_hBuwujvt6S-Ku3pOdgSpAiby55EGOI3JMpv3JX6ptlhnC8cT4UdfqiIck6RDgobhASUKPJlZMkV0Js82Xx-kIHKywirHeGBqKQimJ672sPnbeWL1e"

fernet = Fernet(key)
decrypted_message = fernet.decrypt(ciphertext)

print(decrypted_message.decode("utf-8"))
```

<details>
<summary>Flag</summary>
<br>
`TUCTF{1T$__t0rn@d0$zn__nz$0d@nr0t__$T1}`
</details>

### XOR-Dinary

![xorDinary](https://github.com/user-attachments/assets/84075e16-9cef-4fc3-844a-7a8d11a38b56)

We were given a flag text and the information that the text was XOR'ed against a single character. 

We know that the flag starts with a capital `T` so we can XOR the first hex value `4E` with `54` the value for capital T. This left us with `1a`. Editing the xor script f

```
# Input hex string
hex_string = "4e4f594e5c617763457c2e6c2a282b2d29456d2a287e452b2f45772a2b2f2d67"

# Convert the hex string to bytes
byte_data = bytes.fromhex(hex_string)

# XOR each byte with 0x1a
xor_result = bytes([byte ^ 0x1a for byte in byte_data])

# Convert the result back to a hexadecimal string
xor_hex_result = xor_result.hex()

# Output the result
print("XOR result (hex):", xor_hex_result)
```

While I could've added the python to 

<details>
<summary>Flag</summary>
<br>
`TUCTF{my_f4v02173_w02d_15_m0157}`
</details>

## Reverse Engineering

### Mystery Box

![mysteryBox](https://github.com/user-attachments/assets/bb1d458b-1c11-44a2-95a1-30c53ccd17f0)

We are given an executable runnable only on Windows. Since I am running a Kali Linux environment and didn't want to set up a virtual machine with my limited time, I immediately opened the file in Ghidra. 

After finding the entrypoint that says **"Welcome to Mystery Box"** as an indicator I am in the right spot. THe code requests, a user input and if the key is correct proceeds to perform an XOR each byte of the data in location `140003000` with `0x5A`. 

Simply finding the bytes that start at address `140003000` through the next `00` byte which was ``. 

With the following python script, I got the flag 

<details>
<summary>Flag</summary>
<br>
`TUCTF{Banana_Socks}`
</details>

```python
#Code from ChatGPT because I'm lazy.
encrypted_flag = [ ]  # Replace this with actual data
decrypted_flag = ''.join(chr(byte ^ 0x5A) for byte in encrypted_flag)
print("Decrypted Flag:", decrypted_flag)
```
