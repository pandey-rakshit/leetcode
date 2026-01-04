# 🧩 Zigzag Conversion – LeetCode #6

[![LeetCode](https://img.shields.io/badge/LeetCode-Solved-success)](https://leetcode.com/problems/zigzag-conversion/)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow)
![Language](https://img.shields.io/badge/Language-Python-blue)

---

## 📌 Problem Overview

The **Zigzag Conversion** problem asks us to rearrange a string written in a zigzag pattern across a given number of rows, then read it row by row.

### Example (numRows = 3)

```
P   A   H   N
A P L S I I G
Y   I   R
```

Output:

```
PAHNAPLSIIGYIR
```

---

## 🧠 Problem Statement

Given a string `s` and an integer `numRows`, write the characters of `s` in a zigzag pattern across `numRows` rows, then read the result line by line.

### Function Signature

```cpp
string convert(string s, int numRows);
```

---

## 🧪 Examples

### Example 1

```
Input:  s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

### Example 2

```
Input:  s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
```

### Example 3

```
Input:  s = "A", numRows = 1
Output: "A"
```

---

## 📋 Constraints

* `1 <= s.length <= 1000`
* `s` contains English letters (upper & lower case), `','` and `'.'`
* `1 <= numRows <= 1000`

---

<details>
<summary><b>💡 Key Insight (Click to expand)</b></summary>

* Zigzag follows a **repeating cycle**
* Cycle length = `2 × (numRows − 1)`
* Characters move:

  * straight down
  * then diagonally up
* First & last rows follow a simple pattern
* Middle rows receive **two characters per cycle**

</details>

---

<details>
<summary><b>📝 Approach & Solution</b></summary>

---

### 🔹 Approach 1: Simulation (Row Traversal)

**Idea**
Simulate the zigzag movement by tracking:

* current row
* direction (down / up)

Characters are appended row-wise and combined at the end.

**Pros**

* Very intuitive
* Easy to visualize

**Cons**

* Uses string concatenation (`+=`)
* Causes hidden **O(n²)** cost in Python

**Steps**

* Handle edge case (`numRows == 1`)
* Create one container per row
* Traverse characters while switching direction
* Join all rows

**Complexity**

* Time: O(n) *(O(n²) in practice)*
* Space: O(n)

```python
def convert(s: str, numRows: int) -> str:
    if numRows == 1:
        return s

    rows = ['' for _ in range(numRows)]
    row, step = 0, 1

    for ch in s:
        rows[row] += ch
        if row == 0:
            step = 1
        elif row == numRows - 1:
            step = -1
        row += step

    return ''.join(rows)
```

---

### 🔹 Approach 2: Direct Index Calculation (Optimized)

**Idea**
Instead of simulating movement, directly compute which indices belong to each row using math.

**Key Insight**

```
cycle = 2 × (numRows − 1)
```

**Row Rules**

* First & last rows → fixed step
* Middle rows → two characters per cycle

**Pros**

* No string concatenation
* True **O(n)** time complexity

**Cons**

* Less intuitive than simulation

**Complexity**

* Time: O(n)
* Space: O(n)

```python
def convert(s: str, numRows: int) -> str:
    if numRows == 1:
        return s

    cycle = 2 * (numRows - 1)
    res = []

    for row in range(numRows):
        for i in range(row, len(s), cycle):
            res.append(s[i])
            diag = i + cycle - 2 * row
            if row not in (0, numRows - 1) and diag < len(s):
                res.append(s[diag])

    return ''.join(res)
```

---

### 📌 Pattern Example (`numRows = 3`)

* Cycle = 4
* Row 0 → `0, 4, 8, 12`
* Row 1 → `(1, 3), (5, 7), (9, 11)`
* Row 2 → `2, 6, 10, 14`

---

</details>

---

<details>
<summary><b>🔍 Step-by-Step Walkthrough</b></summary>

### Input

```
s = "PAYPALISHIRING"
numRows = 3
```

### Row Construction

```
Row 0 → PAHN
Row 1 → APLSIIG
Row 2 → YIR
```

### Final Output

```
PAHNAPLSIIGYIR
```

✅ Correct result

</details>

---

## 🧠 Takeaways

* Simulation is **great for understanding**
* Index-based approach is **best for performance**
* Recognizing cycles simplifies many string problems
* Always watch out for hidden string immutability costs in Python

---

## 🔗 References

* 📘 LeetCode Problem:
  [https://leetcode.com/problems/zigzag-conversion/](https://leetcode.com/problems/zigzag-conversion/)
* 🧠 Topic Tags:
  `String`, `Math`, `Pattern Simulation`

---

## ⭐ Final Notes

If you found this explanation helpful:

* ⭐ Star the repository
* 🧩 Try solving it using another language
* 🧠 Practice similar pattern problems

Happy coding 🚀