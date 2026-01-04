# 6. Zigzag Conversion

### **[Try on LeetCode](https://leetcode.com/problems/zigzag-conversion/)**

| | |
|---|---|
| **Status** | ✅ Solved |
| **Difficulty** | 🟡 Medium |
| **Topics** | String, Mathematics |

---

## Description

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this:

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```cpp
string convert(string s, int numRows);
```

---

## Examples

### Example 1
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

### Example 2
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```

### Example 3
```
Input: s = "A", numRows = 1
Output: "A"
```

---

## Constraints

- `1 <= s.length <= 1000`
- `s` consists of English letters (lower-case and upper-case), `','` and `'.'`
- `1 <= numRows <= 1000`

---

<details>
<summary>💡 Hint - Click to view</summary>

Think about the zigzag pattern as a cycle:
- Characters cycle through rows with a repeating pattern
- The cycle length is `2 * (numRows - 1)`
- For each row, identify which indices fall into that row
- Track direction: going down (row increases) or going up (row decreases)
- Use the modulo operation to determine which row a character belongs to

**Tip**: Try simulating the zigzag pattern by tracking the current row and direction as you iterate through the string.

</details>

---

<details>
<summary>📝 Approach & Solution</summary>

## Approaches

### Approach 1: Simulation (String Concatenation)

**Idea**: Simulate the zigzag pattern by tracking direction and row position, then concatenate rows.

**Advantage**: Easy to understand and visualize the zigzag pattern  
**Disadvantage**: String concatenation (`+=`) creates new string objects each iteration, causing **hidden O(n²) overhead** due to string immutability in Python

**Steps**:
1. Handle edge case: if `numRows == 1`, return string as is
2. Create a list to store characters for each row
3. Iterate through the string:
   - Add character to current row
   - Change direction when reaching top (row 0) or bottom (row numRows-1)
   - Move to next row based on direction
4. Join all rows to get result

**Stated Time Complexity**: O(n) (but O(n²) in practice due to string concatenation)  
**Space Complexity**: O(n)

```python
def convert(s: str, numRows: int) -> str:
    if numRows == 1:
        return s
    
    rows = ['' for _ in range(numRows)]
    current_row = 0
    going_down = True
    
    for char in s:
        rows[current_row] += char
        
        # Change direction at top or bottom
        if current_row == 0:
            going_down = True
        elif current_row == numRows - 1:
            going_down = False
        
        # Move to next row
        current_row += 1 if going_down else -1
    
    return ''.join(rows)
```

---

### Approach 2: Direct Index Calculation (List Append)

**Idea**: Use math to directly determine which indices belong to each row, then collect them using list append.

**Advantage**: Avoids string concatenation - uses list append which is O(1) amortized. **True O(n) time complexity**  
**Disadvantage**: Less intuitive - requires understanding the mathematical pattern

**Pattern Analysis**:
- Cycle length = `2 * (numRows - 1)`
- For first and last rows: characters appear at intervals of cycle length
- For middle rows: characters appear in a diagonal pattern within each cycle

**Steps**:
1. Handle edge case: if `numRows == 1`, return string
2. Calculate cycle length
3. For each row, calculate which indices fall in that row using the pattern
4. Append characters at those indices to a list and join

**Time Complexity**: O(n)  
**Space Complexity**: O(n)

```python
def convert(s: str, numRows: int) -> str:
    if numRows == 1:
        return s
    
    cycle = 2 * (numRows - 1)
    result = []
    
    for row in range(numRows):
        if row == 0 or row == numRows - 1:
            # First and last row: simple arithmetic progression
            for i in range(row, len(s), cycle):
                result.append(s[i])
        else:
            # Middle rows: two characters per cycle
            for i in range(row, len(s), cycle):
                result.append(s[i])
                # Diagonal character in the same cycle
                diagonal = i + cycle - 2 * row
                if diagonal < len(s):
                    result.append(s[diagonal])
    
    return ''.join(result)
```

**Example of Pattern**:
- For numRows = 3, cycle = 4
- Row 0: indices 0, 4, 8, 12, ... (step by 4)
- Row 1: indices (1, 3), (5, 7), (9, 11), (13, ...) (pairs in cycle)
- Row 2: indices 2, 6, 10, 14, ... (step by 4)

</details>

---

<details>
<summary>🔍 Detailed Walkthrough</summary>

### Example: s = "PAYPALISHIRING", numRows = 3

**Step 1**: Initialize
- rows = ["", "", ""]
- current_row = 0
- going_down = True

**Step 2**: Iterate through string
```
Index 0, char 'P': rows[0] = "P", going_down = True, next_row = 1
Index 1, char 'A': rows[1] = "A", going_down = True, next_row = 2
Index 2, char 'Y': rows[2] = "Y", going_down = False (at bottom), next_row = 1
Index 3, char 'P': rows[1] = "AP", going_down = False, next_row = 0
Index 4, char 'A': rows[0] = "PA", going_down = True (at top), next_row = 1
Index 5, char 'L': rows[1] = "APL", going_down = True, next_row = 2
Index 6, char 'I': rows[2] = "YI", going_down = False (at bottom), next_row = 1
Index 7, char 'S': rows[1] = "APLS", going_down = False, next_row = 0
Index 8, char 'H': rows[0] = "PAH", going_down = True (at top), next_row = 1
Index 9, char 'I': rows[1] = "APLSI", going_down = True, next_row = 2
Index 10, char 'R': rows[2] = "YIR", going_down = False (at bottom), next_row = 1
Index 11, char 'I': rows[1] = "APLSII", going_down = False, next_row = 0
Index 12, char 'N': rows[0] = "PAHN", going_down = True (at top), next_row = 1
Index 13, char 'G': rows[1] = "APLSIIG", going_down = True, next_row = 2
```

**Step 3**: Concatenate
- rows[0] = "PAHN"
- rows[1] = "APLSIIG"
- rows[2] = "YIR"
- Result = "PAHNAPLSIIGYIR" ✅

</details>

