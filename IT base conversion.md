---
tags:
- IT
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]]

# Convert Decimal to any base
- First step: Get the modulus of the decimal number by the other base
- Second step: Get the floor division result of the decimal number over the other base
- Repeat
- Remember that every iteration's modulus goes into a new position starting from 0 and incrementing by 1
### Decimal to binary example

| Number | Modulus       | Floor division    | Position |
| ------ | ------------- | ----------------- | -------- |
| 175    | 175mod(2) = 1 | floor(175/2) = 87 | 0        |
| 87     | 87mod(2) = 1  | floor(87/2) = 43  | 1        |
| 43     | 43mod(2) = 1  | floor(43/2) = 21  | 2        |
| 21     | 21mod(2) = 1  | floor(21/2) = 10  | 3        |
| 10     | 10mod(2) = 0  | floor(10/2) = 5   | 4        |
| 5      | 5mod(2) = 1   | floor(5/2) = 2    | 5        |
| 2      | 2mod(2) = 0   | floor(2/2) = 1    | 6        |
| 1      | 1mod(2) = 1   | floor(1/2) = 0    | 7        |
Final answer: 10101111
# Decimal to hexadecimal example

| Number | Modulus         | Floor division     | Position |
| ------ | --------------- | ------------------ | -------- |
| 175    | 175mod(16) = 15 | floor(175/16) = 10 | 0        |
| 10     | 10mod(16) = 10  | floor(10/16) = 0   | 1        |
Final answer: AF
# Convert any base to Decimal
- For every position in the base, multiply the number by the base power its position
- Sum up all the results

### Example from Hexadecimal to Decimal
Number: AF and A = 10, F = 15 in Hex

10 x 16^1 + 15 x 16^0 = 160 + 15 
=
# 175
### Example from Binary to Decimal
number: 10101111

1 x 2^7 + 0 x 2^6 + 1 x 2^5 + 0 x 2^4 + 1 x 2 ^3 + 1 x 2^2 + 1 x 2 ^1 + 1 + 2^0 
= 

128 + 0 + 32 + 0 + 8 + 4 + 2 + 1
=
# 175

### Example from Decimal to Decimal (yes, really)
Number: 175

1 x 10^2 + 7 x 10^1 + 5 x 10^0 
=
100 + 70 + 5
=
# 175
# Convert Hexadecimal to binary and back

This is a very straightforward conversion as Hexadecimal was created to be an easier way of representing binary to begin with

All you need to do is:
- Split your binary number into nibbles (4 bits) starting from position 0
- If the last group isn't 4 bits, that's fine, just place them as is
- Convert each nibble into decimal
- Sum em up
### Example
Number: 10101111

1010 = 1 x 2^3 + 0 x 2^2 + 1 x 2^1 + 0 x 2^0 = 8 + 2 = 10 (A in Hex)
1111 = 1 x 2^3 + 1 x 2^2 + 1 x 2^1 + 1 x 2^0 = 8 + 4 + 2 + 1 = 15 (F in Hex)

Answer: AF