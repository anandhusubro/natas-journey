# Natas 08

## Objective

Analyze the application's source code to understand how a secret value is generated and determine how to recover the original value.

## Skills Practiced

* Source Code Analysis
* Data Encoding Analysis
* Reverse Engineering
* Logic Tracing

## Tools Used

* View Source
* Browser Developer Tools
* PHP Function Analysis

## Challenge Description

The application validated user input against a secret value. Rather than storing the secret directly, the source code revealed that it had been transformed through multiple encoding operations before being stored.

## Methodology

1. Inspected the application's source code.
2. Identified the sequence of functions used to transform the secret value.
3. Analyzed the purpose of each transformation.
4. Reversed the operations in the opposite order.
5. Recovered the original secret and used it to satisfy the application's validation logic.

## Key Observation

The source code disclosed the exact algorithm used to encode the secret. While the value appeared hidden, the transformation process was completely reversible.

## Lessons Learned

* Encoding is not the same as encryption.
* Obfuscated data can often be recovered when the transformation logic is known.
* Source code disclosure can expose sensitive implementation details.
* Security should not rely on hiding algorithms from users.

## Concepts Introduced

* Base64 Encoding
* String Reversal
* Hexadecimal Encoding
* Reverse Engineering
* Information Disclosure

## Screenshots
The source code revealed a sequence of transformations used to obscure a secret value. By tracing the encoding process and reversing each operation in the correct order, the original value could be recovered. This demonstrates the difference between data encoding and true cryptographic protection.

<img width="1145" height="651" alt="image" src="https://github.com/user-attachments/assets/979f9d8e-9f33-452a-ba98-5427ba9bba56" />

<img width="1657" height="541" alt="image" src="https://github.com/user-attachments/assets/75c1daf5-cb38-48da-a636-a386cc839926" />


