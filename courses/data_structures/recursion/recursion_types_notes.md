# Types of Recursion

## Overview
This video covers the different types of recursion in programming:
- **Tail Recursion**
- **Head Recursion** 
- **Tree Recursion**
- **Indirect Recursion**
- **Nested Recursion**

---

## 1. Tail Recursion

### Definition
**Tail Recursion** occurs when a recursive function calls itself as the **last statement** in the function, with no operations performed after the recursive call returns.

### Key Characteristics
- The recursive call is the final operation
- All processing happens during the **calling phase**
- No operations are performed during the **returning phase**
- Function doesn't need to wait for the recursive call to complete before finishing

### Example of Tail Recursion
```cpp
void fun(int n) {
    if (n > 0) {
        printf("%d ", n);
        fun(n - 1);  // Last statement - this makes it tail recursion
    }
}
```

### Non-Tail Recursion Example
```cpp
void fun(int n) {
    if (n > 0) {
        printf("%d ", n);
        fun(n - 1) + n;  // Operation after recursive call - NOT tail recursion
    }
}
```

**Why it's not tail recursion:** The addition operation (`+ n`) must be performed after the recursive call returns, meaning work is done during the returning phase.

---

## 2. Tail Recursion vs Loops

### Conversion to Loop
Every tail recursive function can be easily converted to a loop (and vice versa).

**Tail Recursive Version:**
```cpp
void fun(int n) {
    if (n > 0) {
        printf("%d ", n);
        fun(n - 1);
    }
}
```

**Equivalent Loop Version:**
```cpp
void fun(int n) {
    while (n > 0) {
        printf("%d ", n);
        n--;
    }
}
```

### Performance Comparison

| Aspect | Tail Recursion | Loop |
|--------|----------------|------|
| **Time Complexity** | O(n) | O(n) |
| **Space Complexity** | O(n) | O(1) |
| **Activation Records** | n+1 records | 1 record |

### Space Analysis
- **Tail Recursion:** Creates multiple activation records on the stack (one for each recursive call)
  - For `fun(3)`: Creates 4 activation records (n=3, n=2, n=1, n=0)
  - Space complexity: **O(n)**

- **Loop:** Uses only one activation record throughout execution
  - Space complexity: **O(1)** (constant)

### Efficiency Conclusion
- **Time:** Both approaches have the same time complexity
- **Space:** Loops are more space-efficient for tail recursion scenarios
- **Recommendation:** Convert tail recursion to loops for better space efficiency

---

## 3. Compiler Optimization

### Tail Call Optimization
- Modern compilers can automatically optimize tail recursive functions
- They convert tail recursion into loop-like code during compilation
- This reduces space complexity from O(n) to O(1)
- The optimization happens at the object code level

### Benefits
- Automatic space optimization
- Maintains recursive code readability
- Achieves loop-like efficiency

---

## Key Takeaways

1. **Tail recursion** = recursive call is the last statement
2. **All processing** happens during the calling phase in tail recursion
3. **Loops are more space-efficient** than tail recursion
4. **Easy conversion** between tail recursion and loops
5. **Compiler optimization** can automatically improve tail recursive functions
6. **Time complexity** remains the same, but **space complexity** differs significantly

---

## 4. Head Recursion

### Definition
**Head Recursion** occurs when a recursive function calls itself as the **first statement** in the function, with all processing/operations performed **after** the recursive call returns.

### Key Characteristics
- The recursive call is the **first operation**
- **No processing** happens during the calling phase
- **All operations** are performed during the **returning phase**
- Function must wait for recursive calls to complete before doing any work

### Structure of Head Recursion
```cpp
void fun(int n) {
    if (n > 0) {
        fun(n - 1);        // First statement - recursive call
        printf("%d ", n);  // Processing after recursive call
    }
}
```

### Example Analysis
For `fun(3)`, the execution flow:
1. **Calling Phase:** `fun(3)` → `fun(2)` → `fun(1)` → `fun(0)`
2. **Returning Phase:** Print 1 → Print 2 → Print 3
3. **Output:** `1 2 3`

### What Makes It Head Recursion?
- **✅ Valid Head Recursion:** No statements before the recursive call
- **❌ Not Head Recursion:** Any operation before the recursive call
```cpp
// NOT Head Recursion
void fun(int n) {
    if (n > 0) {
        printf("Before call\n");  // Operation before recursive call
        fun(n - 1);
        printf("%d ", n);
    }
}
```

---

## 5. Head Recursion vs Loops

### Conversion Difficulty
Head recursion **cannot be easily converted** to loops in a straightforward manner.

### Direct Conversion Attempt (Doesn't Work)
```cpp
// Head Recursive Version
void fun(int n) {
    if (n > 0) {
        fun(n - 1);
        printf("%d ", n);
    }
}
// Output: 1 2 3

// Attempted Direct Loop Conversion
void fun(int n) {
    while (n > 0) {
        printf("%d ", n);  // This prints 3 2 1, not 1 2 3
        n--;
    }
}
// Output: 3 2 1 (Wrong!)
```

### Correct Loop Equivalent
To achieve the same output (`1 2 3`), we need a different approach:
```cpp
void fun(int n) {
    for (int i = 1; i <= n; i++) {
        printf("%d ", i);
    }
}
// Output: 1 2 3 (Correct!)
```

### Key Insight
- The loop version looks **completely different** from the recursive structure
- **Direct structural conversion** is not possible
- We need to **redesign the logic** to achieve the same result

---

## 6. Comparison: Tail vs Head Recursion

| Aspect | Tail Recursion | Head Recursion |
|--------|----------------|----------------|
| **Recursive Call Position** | Last statement | First statement |
| **Processing Phase** | During calling | During returning |
| **Loop Conversion** | Easy & direct | Difficult & indirect |
| **Structure Similarity** | Maintains similar structure | Requires complete redesign |
| **Execution Order** | Forward (3→2→1) | Reverse (1→2→3) |

---

## Key Takeaways

1. **Head recursion** = recursive call is the **first statement**
2. **All processing** happens during the **returning phase**
3. **Cannot be easily converted** to loops (unlike tail recursion)
4. **Loop equivalents** require completely different logic structure
5. **Execution pattern** creates reverse order output compared to input
6. **Structural conversion** is not straightforward - requires algorithmic redesign

---

## 7. Tree Recursion

### Definition
**Tree Recursion** occurs when a recursive function calls itself **more than one time** within the same function call.

### Linear vs Tree Recursion

| **Linear Recursion** | **Tree Recursion** |
|---------------------|-------------------|
| Function calls itself **only once** | Function calls itself **multiple times** |
| Creates a linear call sequence | Creates a tree-like call structure |
| Examples: Tail, Head recursion | Example: Fibonacci, binary tree traversals |

### Structure Comparison
```cpp
// Linear Recursion (calls itself once)
void linearRecursion(int n) {
    if (n > 0) {
        // Some processing
        linearRecursion(n - 1);  // Single recursive call
        // Some processing
    }
}

// Tree Recursion (calls itself multiple times)
void treeRecursion(int n) {
    if (n > 0) {
        printf("%d ", n);
        treeRecursion(n - 1);    // First recursive call
        treeRecursion(n - 1);    // Second recursive call
    }
}
```

---

## 8. Tree Recursion Example Analysis

### Function Tracing
For `treeRecursion(3)`, let's trace the execution:

```
Call Tree Structure:
                    fun(3)
                   /      \
               fun(2)      fun(2)
              /     \     /     \
          fun(1)  fun(1) fun(1) fun(1)
          /   \   /   \  /   \  /   \
      fun(0) fun(0) fun(0) fun(0) fun(0) fun(0) fun(0) fun(0)
```

### Execution Order and Output
The calls are made in this order:
1. `fun(3)` → prints **3**
2. `fun(2)` → prints **2** 
3. `fun(1)` → prints **1**
4. `fun(0)` → terminates
5. `fun(0)` → terminates
6. `fun(1)` → prints **1**
7. `fun(0)` → terminates
8. `fun(0)` → terminates
9. `fun(2)` → prints **2**
10. `fun(1)` → prints **1**
11. `fun(0)` → terminates
12. `fun(0)` → terminates
13. `fun(1)` → prints **1**
14. `fun(0)` → terminates
15. `fun(0)` → terminates

**Final Output:** `3 2 1 1 2 1 1`

### Stack Analysis
- **Maximum stack height:** 4 activation records (for path: fun(3) → fun(2) → fun(1) → fun(0))
- **Total function calls:** 15 calls
- **Stack reuse:** Same memory locations are reused as functions return

---

## 9. Complexity Analysis

### Time Complexity Calculation

**Level-wise call count for `n = 3`:**
- Level 0: 1 call (`fun(3)`)
- Level 1: 2 calls (`fun(2)`, `fun(2)`)
- Level 2: 4 calls (`fun(1)` × 4)
- Level 3: 8 calls (`fun(0)` × 8)

**Total calls:** `1 + 2 + 4 + 8 = 15`

**Mathematical representation:**
```
Total calls = 2⁰ + 2¹ + 2² + 2³ + ... + 2ⁿ
            = 2^(n+1) - 1
            = O(2ⁿ)
```

**For `n = 3`:** `2^(3+1) - 1 = 2⁴ - 1 = 16 - 1 = 15` ✓

### Space Complexity Analysis
- **Space required:** Equal to the **maximum height** of the recursion tree
- **Maximum height:** `n + 1` levels (from `fun(n)` to `fun(0)`)
- **Space complexity:** **O(n)**

### Complexity Summary
| Aspect | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | **O(2ⁿ)** | Exponential due to repeated subproblems |
| **Space** | **O(n)** | Linear based on maximum recursion depth |

---

## 10. Tree Recursion Characteristics

### Key Properties
1. **Exponential time complexity** - Very inefficient for large inputs
2. **Linear space complexity** - Stack depth equals input size
3. **Repeated computations** - Same subproblems solved multiple times
4. **Tree-like structure** - Creates branching call patterns

### Visual Representation
```
Tree Structure for treeRecursion(3):

Level 0:           [3]
                  /   \
Level 1:        [2]   [2]
               /  \   /  \
Level 2:     [1] [1] [1] [1]
            / |  | \/ |  | \
Level 3:  [0][0][0][0][0][0][0][0]
```

### Memory Usage Pattern
- **Activation records created:** 15 total
- **Maximum simultaneous records:** 4
- **Stack reuse:** High (same memory locations reused)

---

## 11. Important Notes

### Performance Implications
- **Exponential growth:** For `n = 10`, approximately 1,024 function calls
- **Inefficiency:** Many redundant calculations
- **Use cases:** Educational purposes, understanding recursion patterns

### Optimization Opportunities
- **Memoization:** Store results of subproblems
- **Dynamic Programming:** Bottom-up approach
- **Iterative solutions:** Convert to loops where possible

### Real-world Applications
- **Fibonacci sequence** (classic tree recursion example)
- **Binary tree traversals**
- **Divide and conquer algorithms**
- **Backtracking problems**

---

## 12. Indirect Recursion

### Definition
**Indirect Recursion** occurs when multiple functions call each other in a **circular fashion**, creating a cycle where functions don't call themselves directly but call each other to form a recursive pattern.

### Key Characteristics
- **Multiple functions** involved (2 or more)
- **Circular calling pattern:** A → B → C → A (forms a cycle)
- **No direct self-calling:** Functions call other functions, not themselves
- **Mutual recursion:** Functions are mutually dependent

### Basic Structure
```cpp
// Simple two-function indirect recursion
void functionA(int n) {
    if (condition) {
        // Some processing
        functionB(modified_n);  // Calls function B
    }
}

void functionB(int n) {
    if (condition) {
        // Some processing
        functionA(modified_n);  // Calls function A back
    }
}
```

### Circular Call Pattern
```
Function A → Function B → Function C → Function A
    ↑                                        ↓
    ←←←←←←←←← (Cycle repeats) ←←←←←←←←←←←←←←←
```

---

## 13. Indirect Recursion Example

### Code Implementation
```cpp
void funA(int n) {
    if (n > 0) {
        printf("%d ", n);
        funB(n - 1);  // Calls function B with n-1
    }
}

void funB(int n) {
    if (n > 1) {
        printf("%d ", n);
        funA(n / 2);  // Calls function A with n/2
    }
}
```

### Execution Trace for `funA(20)`

| Step | Function | Input | Condition | Action | Output |
|------|----------|-------|-----------|---------|--------|
| 1 | `funA` | 20 | 20 > 0 ✓ | Print 20, call `funB(19)` | **20** |
| 2 | `funB` | 19 | 19 > 1 ✓ | Print 19, call `funA(9)` | **19** |
| 3 | `funA` | 9 | 9 > 0 ✓ | Print 9, call `funB(8)` | **9** |
| 4 | `funB` | 8 | 8 > 1 ✓ | Print 8, call `funA(4)` | **8** |
| 5 | `funA` | 4 | 4 > 0 ✓ | Print 4, call `funB(3)` | **4** |
| 6 | `funB` | 3 | 3 > 1 ✓ | Print 3, call `funA(1)` | **3** |
| 7 | `funA` | 1 | 1 > 0 ✓ | Print 1, call `funB(0)` | **1** |
| 8 | `funB` | 0 | 0 > 1 ✗ | **Terminate** | - |

**Final Output:** `20 19 9 8 4 3 1`

### Call Stack Visualization
```
funA(20) → funB(19) → funA(9) → funB(8) → funA(4) → funB(3) → funA(1) → funB(0)
    ↑                                                                        ↓
    ←←←←←←←←←←←←←←← (Returns back through the chain) ←←←←←←←←←←←←←←←←←←←←←←←←←
```

---

## 14. Indirect Recursion Analysis

### Pattern Recognition
The example shows an interesting pattern:
- **Function A:** Reduces by 1 (n → n-1)
- **Function B:** Reduces by half (n → n/2) 
- **Different reduction rates** create unique sequence patterns

### Termination Conditions
- **Function A:** Stops when `n ≤ 0`
- **Function B:** Stops when `n ≤ 1`
- **Both conditions must be met** for complete termination

### Advantages
- **Problem decomposition:** Different functions handle different aspects
- **Code organization:** Separates logic into distinct functions
- **Mutual dependency:** Functions work together to solve complex problems

### Disadvantages
- **Complexity:** Harder to trace and debug
- **Stack overhead:** Multiple function calls increase memory usage
- **Interdependency:** Functions are tightly coupled

---

## 15. Indirect vs Direct Recursion

| Aspect | Direct Recursion | Indirect Recursion |
|--------|------------------|-------------------|
| **Function calls** | Function calls itself | Functions call each other |
| **Structure** | Single function | Multiple functions |
| **Cycle** | Self-referential | Circular chain |
| **Tracing** | Easier to follow | More complex to trace |
| **Use cases** | Single problem | Multi-aspect problems |

---

## 16. Applications of Indirect Recursion

### Common Use Cases
1. **State machines** - Different states call different functions
2. **Parsing algorithms** - Different grammar rules call each other
3. **Game development** - Different game states interact
4. **Compiler design** - Different parsing phases call each other

### Example Applications
```cpp
// State machine example
void stateA() {
    // Process state A
    if (condition) stateB();
}

void stateB() {
    // Process state B  
    if (condition) stateA();
}

// Parser example
void parseExpression() {
    // Parse expression
    parseTerm();
}

void parseTerm() {
    // Parse term
    if (hasOperator) parseExpression();
}
```

---

## Key Takeaways

1. **Indirect recursion** involves **multiple functions** calling each other cyclically
2. **Circular pattern** creates the recursive behavior without direct self-calls
3. **Complex tracing** due to function switching makes debugging challenging
4. **Useful for problems** requiring different processing logic at different stages
5. **Termination requires** careful condition checking in all involved functions

---

## Next Topics
- Nested Recursion