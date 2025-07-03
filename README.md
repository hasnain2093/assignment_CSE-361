<div align="center">

# Hajee Mohammad Danesh Science and Technology University  
**Dinajpur-5200**

---

##  Course Information  
**Course Title:** Mathematical Analysis for Computer Science  
**Course Code:** CSE 361

---

##  Algorithm Name  
### BitTwistX Cipher  
*(Bitwise XOR with Rotational Twist & Key-based Shift)*

---

##  Submitted By  
**Name:** Md Alhasnain Bin Arif  
**Student ID:** *2102038*  
**Level:** 3  
**Semester:** II  
**Department:** Computer Science and Engineering

---

##  Submitted To  
**Name:** Pankaj Bhowmik  
**Designation:** Lecturer  
**Department:** Computer Science and Engineering

---

</div>

---

##  Algorithm Description  
**BitTwistX Cipher** is a custom-designed symmetric cryptographic algorithm based on:

- ASCII value manipulation  
- Bitwise XOR operation with a text key  
- Key-controlled **bitwise rotation** (left in encryption, right in decryption)  
- Dynamic behavior per character using modular arithmetic (`mod 8`)  
- Reversible and lightweight design, ideal for education or embedded systems

---

##  Table of Contents
-  Encryption Algorithm  
-  Decryption Algorithm  
-  Example Test Case  
-  Flowcharts  
-  C++ Source Code

---

##  Encryption Algorithm

Let:  
- `K[i]` = ASCII value of the i-th character of the **Key**  
- `P[i]` = ASCII value of the i-th character of the **Plaintext**  
- `C[i]` = ASCII value of the resulting **Ciphertext**

**Steps:**  
1. Match key length to plaintext by repeating/truncating as necessary.  
2. For each character at position `i`:  
   - Perform XOR: `temp = P[i] XOR K[i]`  
   - Left rotate `temp` by `(K[i] % 8)` bits  
   - Final `C[i]` is the rotated result

---

##  Decryption Algorithm

Let:  
- `K[i]` = ASCII value of the i-th character of the **Key**  
- `C[i]` = ASCII value of the i-th character of the **Ciphertext**  
- `D[i]` = Decrypted character (original plaintext)

**Steps:**  
1. Match key length to ciphertext by repeating/truncating as needed  
2. For each character at position `i`:  
   - Right rotate `C[i]` by `(K[i] % 8)` bits  
   - XOR the result with `K[i]`  
   - Final character is `D[i]`

---

##  Example Test Case

**Plaintext:** `Hi`  
**Key:** `XY`

### Encryption Table:

| Pos | Plain Char | ASCII | Key Char | ASCII | XOR Result | Rotate Left | Cipher Byte |
|-----|------------|--------|----------|--------|-------------|---------------|---------------|
|  0  | H          | 72     | X        | 88     | 72 XOR 88 = 16 | (88 % 8 = 0) → 16 | `16` |
|  1  | i          | 105    | Y        | 89     | 105 XOR 89 = 48 | (89 % 8 = 1) → 96 | `96` |

**Ciphertext (ASCII):** `[16, 96]`  
**Ciphertext (char):** `\x10` and `` ` ``

### Decryption Table:

| Pos | Cipher Byte | ASCII | Rotate Right | XOR with Key | Recovered ASCII | Char |
|-----|--------------|--------|----------------|----------------|------------------|------|
|  0  | 16           | 16     | (0 bits) → 16   | 16 XOR 88 = 72 | 72               | H    |
|  1  | 96           | 96     | (1 bit) → 48    | 48 XOR 89 = 105 | 105              | i    |

**Decrypted Text:** `Hi`

---


##  C++ Source Code

```cpp
#include <iostream>
#include <string>
using namespace std;

int rotateLeft(int value, int shift) {
    return ((value << shift) | (value >> (8 - shift))) & 0xFF;
}

int rotateRight(int value, int shift) {
    return ((value >> shift) | (value << (8 - shift))) & 0xFF;
}

string encrypt(string plaintext, string key) {
    string ciphertext = "";
    int keyLen = key.length();

    for (size_t i = 0; i < plaintext.length(); i++) {
        char p = plaintext[i];
        char k = key[i % keyLen];
        int xorVal = p ^ k;
        int shift = k % 8;
        int rotated = rotateLeft(xorVal, shift);
        ciphertext += static_cast<char>(rotated);
    }

    return ciphertext;
}

string decrypt(string ciphertext, string key) {
    string plaintext = "";
    int keyLen = key.length();

    for (size_t i = 0; i < ciphertext.length(); i++) {
        char c = ciphertext[i];
        char k = key[i % keyLen];
        int shift = k % 8;
        int unrotated = rotateRight(c, shift);
        int original = unrotated ^ k;
        plaintext += static_cast<char>(original);
    }

    return plaintext;
}

int main() {
    string text = "Hi";
    string key = "XY";

    string encrypted = encrypt(text, key);
    string decrypted = decrypt(encrypted, key);

    cout << "Original Text: " << text << endl;
    cout << "Encrypted Text: ";
    for (char ch : encrypted) cout << (int)(unsigned char)ch << " ";
    cout << endl;

    cout << "Decrypted Text: " << decrypted << endl;

    return 0;
}
