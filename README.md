# Python ASCII and Hexadecimal Encoding & Decoding

This project demonstrates how **ASCII encoding/decoding** and **Hexadecimal encoding/decoding** work using Python.  
Encoding converts readable text into another representation for processing, storage, or transmission, while decoding restores the original data.

The program uses a sample string (`Cyber123`) to demonstrate ASCII encoding and provides a **menu-driven system** that allows users to encode or decode text using hexadecimal format.

---

# Overall Program Workflow

The program works in the following steps:

1. Define a sample text string.
2. Convert the string into ASCII bytes.
3. Display ASCII numeric values.
4. Decode ASCII bytes back into readable text.
5. Convert text into hexadecimal format.
6. Convert hexadecimal values back into text.
7. Allow the user to choose encoding or decoding using a menu system.

---

# Code Functions Overview

## 1. ASCII Encoding

The program converts text into ASCII byte format.

```python
ascii_encoded = text.encode("ascii")
```

Function Purpose

Converts characters into ASCII byte representation.

Each character is represented by a numeric ASCII value.

Example Output
```
ASCII Encoded (bytes): b'Cyber123'
```

2. Display ASCII Numeric Values

The ASCII bytes are converted into a list of numeric values.
```
ascii_numbers = list(ascii_encoded)
```

Example Output
```
ASCII Decoded: Cyber123
```

Hexadecimal Encoding and Decoding

The program also implements hexadecimal conversion using two main functions.
```
def string_to_hex(input_string):
    hex_output = input_string.encode('utf-8').hex()
    return hex_output
```

Function Purpose

Converts a string into hexadecimal representation.

Uses UTF-8 encoding before converting bytes to hex values.

Example
```
Cyber123 → 4379626572313233
```

5. Hexadecimal Decoding Function
```
def hex_to_string(hex_input):
    bytes_object = bytes.fromhex(hex_input)
    decoded_string = bytes_object.decode('utf-8')
    return decoded_string
```

Function Purpose
Converts hexadecimal values back into readable text.
Uses bytes.fromhex() to convert hex into bytes.
Uses decode() to restore the original string.

Example
```
4379626572313233 → Cyber123
```

Main Program Execution
The program includes a menu-driven interface that allows the user to interact with the system.

```
def main():
```

User Options
1 → Encode a string into hexadecimal
2 → Decode hexadecimal into text

Example Interaction

Encoding
```
Enter the string to encode: Cyber123
Hexadecimal: 4379626572313233
```

Decoding
```
Enter the hexadecimal string to decode: 4379626572313233
Decoded string: Cyber123
```

Summary

This project demonstrates:

ASCII encoding and decoding

Hexadecimal encoding and decoding

Python built-in encoding functions (encode, decode)

Conversion between text, bytes, and hexadecimal values

A simple menu-based user interaction system

