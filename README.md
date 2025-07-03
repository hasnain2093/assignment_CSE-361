Concept Overview

TwistXOR Cipher is a newly proposed symmetric-key encryption algorithm designed to be both lightweight and secure. It combines the power of bitwise XOR operations with key-controlled bitwise rotation (twist), resulting in a simple yet effective encryption method. The algorithm is intended for educational and lightweight security applications, particularly where fast and reversible encryption is necessary.

TwistXOR uses a plain-text string and a secret key string of arbitrary length. The encryption and decryption processes are symmetric, meaning the same key is used for both. The key idea of the algorithm is to not only XOR the plain text with the key but also dynamically shift (twist) the result based on the ASCII value of each corresponding key character.

Key Design
In TwistXOR Cipher, the key can be of any length. If the key is shorter than the plaintext, it is cyclically repeated to match the length of the input. The ASCII value of each character in the key controls how much to rotate (shift) the result of the XOR operation. This adds a layer of complexity compared to traditional XOR-based encryption.

Each key character does two things:
1.It is XORed with a plaintext character.
2.Its ASCII value (modulo 8) determines the number of bits the result will be rotated (left in encryption, right in decryption).

Encryption Process
The encryption algorithm follows these steps:

Convert the plaintext and key into byte values (based on ASCII).

Adjust the key so that its length matches the length of the plaintext.

For each character in the plaintext:

XOR the character with the corresponding character in the key.

Left-rotate (bitwise) the result by (ASCII of key character % 8) bits.

Store the rotated value as part of the ciphertext.

The final ciphertext is a sequence of integers (or characters), which are not directly human-readable, thus ensuring confidentiality.

Decryption Process

The decryption process is the reverse of encryption:
      1.Convert the ciphertext and key into byte values.
      2.Adjust the key length to match the ciphertext.

For each byte in the ciphertext:
     1.Right-rotate the byte by (ASCII of key character % 8) bits.
     2.XOR the result with the corresponding key character.
     3.Convert the final result back to characters to retrieve the original plaintext.
     Because the operations are fully reversible, the exact original message is recovered,
     provided the correct key is used.

Example (Test Case)
Let’s consider a simple example where:
Plaintext = Hi
Key = XY

Step-by-step:
1.The ASCII values of 'H' and 'i' are 72 and 105 respectively.
2.The ASCII values of 'X' and 'Y' are 88 and 89 respectively.

First character:
      72 XOR 88 = 16
    Rotate left 16 by (88 % 8 = 0) bits → Result: 16

Second character:
     105 XOR 89 = 48
    Rotate left 48 by (89 % 8 = 1) bit → Result: 96

The ciphertext is therefore [16, 96] (as byte values or in hex format 0x10 0x60).

To decrypt:
      Rotate 16 right by 0 bits → 16
         16 XOR 88 = 72 → ‘H’

  Rotate 96 right by 1 bit → 48
    48 XOR 89 = 105 → ‘i’

So, the original message Hi is fully recovered.

Strengths of TwistXOR Cipher
1.Simple but effective: The algorithm is easy to understand and implement but non-trivial to break without the key.
2.Dynamic behavior: Every character in the key influences both the XOR and the twist operation.
3.Lightweight: Suitable for embedded systems, learning purposes, or lightweight encryption.
4.Fully reversible: Guarantees accurate decryption with the correct key.

Final Words
TwistXOR Cipher is an original symmetric encryption method that leverages both XOR and dynamic rotation. It demonstrates how logical operations and number theory (bitwise operations and modular arithmetic) can be used to design secure and efficient encryption systems. This project was developed as part of the "Mathematical Analysis for Computer Science" course, focusing on simplicity, reversibility, and innovation.


