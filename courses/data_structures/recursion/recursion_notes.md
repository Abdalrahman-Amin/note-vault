# Recursion Fundamentals

## 🎯 Learning Objectives
- Understand what recursion is and how it works
- Learn to trace recursive functions step by step
- Understand the two phases of recursion (calling and returning)
- Master the concept of base conditions
- Learn to analyze time complexity of recursive functions

---

## 📖 What is Recursion?

**Definition**: A function that calls itself is called a **recursive function**.

### Key Components of Recursion:
1. **Self-calling**: The function calls itself with modified parameters
2. **Base condition**: A condition that stops the recursion (prevents infinite calls)
3. **Two phases**: Calling phase (going deeper) and returning phase (coming back)

### General Structure:
```c
function recursiveFunc(parameter) {
    if (base_condition) {
        // Stop recursion
        return;
    }
    
    // Do something (optional)
    recursiveFunc(modified_parameter);  // Recursive call
    // Do something else (optional)
}
```

---

## 🔍 Example 1: Print Numbers (Descending Order)

### Code:
```c
void fun1(int n) {
    if (n > 0) {
        printf("%d ", n);    // Print first
        fun1(n - 1);         // Then recurse
    }
}

// Call: fun1(3)
```

### Execution Trace:
```
fun1(3) → print 3 → fun1(2) → print 2 → fun1(1) → print 1 → fun1(0) → stop
```

### Tree Representation:
```
fun1(3) - prints 3
├── fun1(2) - prints 2  
    ├── fun1(1) - prints 1
        ├── fun1(0) - does nothing (base case)
```

**Output**: `3 2 1`

### Key Insight:
- **Print happens DURING calling phase**
- Operations before recursive call execute while "going down"

---

## 🔍 Example 2: Print Numbers (Ascending Order)

### Code:
```c
void fun2(int n) {
    if (n > 0) {
        fun2(n - 1);         // Recurse first
        printf("%d ", n);    // Then print
    }
}

// Call: fun2(3)
```

### Execution Trace:
```
fun2(3) → fun2(2) → fun2(1) → fun2(0) → return → print 1 → print 2 → print 3
```

**Output**: `1 2 3`

### Key Insight:
- **Print happens DURING returning phase**
- Operations after recursive call execute while "coming back up"

---

## 🏠 Real-World Analogy: The Room Example

### Scenario 1: "Switch on bulb, then go to next room"
```
Room 1: Switch ON bulb 1 → Go to Room 2
Room 2: Switch ON bulb 2 → Go to Room 3  
Room 3: Switch ON bulb 3 → No more rooms (stop)
```
**Result**: Bulbs switched on in order: 1, 2, 3 (during calling phase)

### Scenario 2: "Go to next room, then switch on bulb"
```
Room 1: Go to Room 2 (don't switch bulb yet)
Room 2: Go to Room 3 (don't switch bulb yet)
Room 3: No more rooms → Switch ON bulb 3 → Return to Room 2
Room 2: Switch ON bulb 2 → Return to Room 1
Room 1: Switch ON bulb 1
```
**Result**: Bulbs switched on in order: 3, 2, 1 (during returning phase)

---

## 🎯 The Two Phases of Recursion

### 1. Calling Phase (Ascending/Going Down)
- Function calls itself repeatedly
- Like stretching a rubber band
- Stack builds up with function calls
- Parameters typically decrease toward base case

### 2. Returning Phase (Descending/Coming Back)
- Functions return to their callers
- Like releasing a stretched rubber band
- Stack unwinds
- Pending operations after recursive calls execute

### Visual Representation:
```
Calling Phase:    main() → fun(3) → fun(2) → fun(1) → fun(0)
                     ↓        ↓        ↓        ↓        ↓
                  Going deeper into recursion...

Returning Phase:  main() ← fun(3) ← fun(2) ← fun(1) ← fun(0)
                     ↑        ↑        ↑        ↑        ↑
                  Coming back from recursion...
```

---

## ⚠️ Important Concepts

### Base Condition
- **Critical**: Without it, recursion becomes infinite
- **Purpose**: Provides termination condition
- **Example**: `if (n <= 0) return;` or `if (n == 0) return;`

### Stack Usage
- Each recursive call uses stack memory
- Stack stores: local variables, parameters, return addresses
- Deep recursion can cause stack overflow

### Function Call Mechanics
- When function is called: control transfers, parameters passed
- When function returns: control returns to caller, pending operations execute
- Any operations on the same line after function call wait for function to complete

---

## 💡 Key Takeaways

1. **Recursion = Self + Base Case**: Function calls itself with a stopping condition
2. **Two Phases**: Calling (going down) and Returning (coming back up)
3. **Order Matters**: Operations before recursive call happen during calling phase; operations after happen during returning phase
4. **Stack-based**: Uses system stack to manage function calls
5. **Like a Rubber Band**: Stretch (call) and release (return)

---

## 🔢 Quick Comparison

| Aspect | Print-Then-Call | Call-Then-Print |
|--------|----------------|-----------------|
| **Code** | `print(n); recurse(n-1);` | `recurse(n-1); print(n);` |
| **When prints** | Calling phase | Returning phase |
| **Output for fun(3)** | 3, 2, 1 | 1, 2, 3 |
| **Analogy** | Switch bulb while going | Switch bulb while returning |

---

## 🔄 Recursion vs Loops: Key Comparison

### Similarities:
- Both involve **repetition**
- Both can solve similar problems
- Both need termination conditions

### Key Differences:

| Aspect | Loops | Recursion |
|--------|-------|-----------|
| **Phases** | Only **Ascending** (one direction) | **Ascending + Descending** (two phases) |
| **Execution** | Sequential repetition | Function calls with stack |
| **Memory** | Constant space | Stack space grows |
| **Power** | Limited to forward processing | Can process both ways |

### Visual Comparison:
```
Loop:      Start → Repeat → Repeat → Repeat → End
           ────────────────────────────────────→

Recursion: Start → Call → Call → Call → Base Case
           ────────────────────────────────────→  (Ascending)
           ←────────────────────────────────────  (Descending)
           Return ← Return ← Return ← Return
```

---

## 📐 The Universal Recursion Formula

### General Pattern:
```c
function recursive(parameters) {
    if (base_condition) {
        return; // Stop recursion
    }
    
    // STATEMENT 1: Executes at CALLING time (Ascending phase)
    statement_before_call();
    
    recursive(modified_parameters); // RECURSIVE CALL
    
    // STATEMENT 2: Executes at RETURNING time (Descending phase)  
    statement_after_call();
    
    // Any computations here also execute at RETURNING time
}
```

### 🎯 **The Golden Rule**:
- **Before recursive call** = **Calling time** = **Ascending phase**
- **After recursive call** = **Returning time** = **Descending phase**

---

## 💪 The Power of Recursion

### Why Recursion is Powerful:
1. **Two-Phase Processing**: Can handle operations both going down and coming back up
2. **Natural Problem Solving**: Many problems have recursive structure
3. **Elegant Solutions**: Often shorter and more intuitive than iterative solutions
4. **Descending Phase**: Unique capability that loops don't have

### When to Use Recursion:
- Problems that can be broken into smaller subproblems
- When you need to process data in reverse order naturally
- Tree/graph traversals
- Mathematical sequences (factorial, fibonacci)
- Divide and conquer algorithms

---

## 📝 Practice Questions
1. What would `fun1(5)` output?
2. What would `fun2(4)` output?
3. What happens if we forget the base condition?
4. How many function calls are made for `fun1(n)`?
5. **New**: Can you solve the same problem that `fun2(3)` solves using a loop? What's missing?

---

## 🔗 Next Topics
- How recursion uses the stack internally
- Time complexity analysis of recursive functions
- Space complexity and stack usage
- Common recursive patterns (factorial, fibonacci, etc.)
- Tail recursion optimization