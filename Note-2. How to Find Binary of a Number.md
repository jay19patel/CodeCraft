# How to Find Binary of a Number

Binary representation means expressing a number using only 0 and 1. Each digit in binary is a power of 2.

## Steps to Find Binary of Any Number
1. Find the largest power of 2 less than or equal to the number.
2. Subtract that power from the number and mark it as `1` in the binary form.
3. Repeat with the remaining number until it becomes 0.
4. If a power of 2 is not used, mark it as `0`.

## Example: Find Binary of 19
We find powers of 2:

| Power of 2 | Value  | Used? (1 = Yes, 0 = No) |
|------------|--------|------------------------|
| 2⁴        | 16     | 1                      |
| 2³        | 8      | 0                      |
| 2²        | 4      | 0                      |
| 2¹        | 2      | 1                      |
| 2⁰        | 1      | 1                      |

### Calculation:
- **19 = 16 + 2 + 1**
- **Marking used powers as 1 and others as 0:**
  - 2⁴ → `1`
  - 2³ → `0`
  - 2² → `0`
  - 2¹ → `1`
  - 2⁰ → `1`

### Binary of 19:
`10011`

## Binary Representation in Graph Form
```
  2⁴  2³  2²  2¹  2⁰
  -------------------
   1    0    0    1    1
```
This means 19 in binary is **10011**.

---

## Example: Find Binary of 155
We find powers of 2:

| Power of 2 | Value  | Used? (1 = Yes, 0 = No) |
|------------|--------|------------------------|
| 2⁷        | 128    | 1                      |
| 2⁶        | 64     | 0                      |
| 2⁵        | 32     | 1                      |
| 2⁴        | 16     | 1                      |
| 2³        | 8      | 1                      |
| 2²        | 4      | 0                      |
| 2¹        | 2      | 1                      |
| 2⁰        | 1      | 1                      |

### Calculation:
- **155 = 128 + 32 + 16 + 8 + 2 + 1**
- **Marking used powers as 1 and others as 0:**
  - 2⁷ → `1`
  - 2⁶ → `0`
  - 2⁵ → `1`
  - 2⁴ → `1`
  - 2³ → `1`
  - 2² → `0`
  - 2¹ → `1`
  - 2⁰ → `1`

### Binary of 155:
`10011011`

## Binary Representation in Graph Form
```
  2⁷  2⁶  2⁵  2⁴  2³  2²  2¹  2⁰
  ---------------------------------
   1    0    1    1    1    0    1    1
```
This means 155 in binary is **10011011**.

---

## Example: Find Binary of "JAY"
Each letter is converted to its ASCII value and then to binary.

| Letter | ASCII Value | Binary Representation |
|--------|------------|----------------------|
| J      | 74         | 1001010              |
| A      | 65         | 1000001              |
| Y      | 89         | 1011001              |

### Combined Binary Representation of "JAY":
**`1001010 1000001 1011001`**

