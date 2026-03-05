# ASCII Encoding and Decoding Example in Python

This Python program demonstrates how to encode and decode text using ASCII encoding.

## Source Code

```python
# Sample text
text = "Cyber123"

print("Original Text:")
print(text)
print("-" * 40)

# ASCII Encoding
ascii_encoded = text.encode("ascii")
print("ASCII Encoded (bytes):", ascii_encoded)

ascii_numbers = list(ascii_encoded)
print("ASCII Values:", ascii_numbers)

print("-" * 40)

# ASCII Decoding
ascii_decoded = ascii_encoded.decode("ascii")
print("ASCII Decoded:", ascii_decoded)
```

## Explanation

- The variable `text` stores the original string `"Cyber123"`.
- The `encode("ascii")` method converts the string into ASCII byte format.
- `list(ascii_encoded)` converts the byte values into a list of ASCII numerical values.
- The `decode("ascii")` method converts the ASCII bytes back into the original string.

## Expected Output

```
Original Text:
Cyber123
----------------------------------------
ASCII Encoded (bytes): b'Cyber123'
ASCII Values: [67, 121, 98, 101, 114, 49, 50, 51]
----------------------------------------
ASCII Decoded: Cyber123
```
