# Static Variables in Recursion

## Overview
This lesson explores how static variables behave differently from regular local variables in recursive functions, demonstrating their single-copy nature and impact on function results.

## Key Concepts

### Regular Variables in Recursion
- **Local variables** are created **separately** for each recursive call
- Each function call gets its **own copy** of local variables
- Variables are stored in the **stack** as activation records
- Each activation record maintains independent variable values

### Static Variables in Recursion
- **Static variables** have only **one copy** throughout all recursive calls
- Created **once** at program loading time, not for each function call
- Stored in the **code section** (or global/static section)
- **Shared** among all recursive function calls
- Maintain their value across all recursive calls

## Example 1: Regular Variables (Without Static)

```cpp
int fun(int n) {
    if (n > 0) {
        return fun(n-1) + n;  // Adding n at return time
    }
    return 0;
}

int main() {
    int result = fun(5);
    printf("Result: %d\n", result);  // Output: 15
    return 0;
}
```

### Execution Trace for fun(5):
```
fun(5) → fun(4) + 5
fun(4) → fun(3) + 4  
fun(3) → fun(2) + 3
fun(2) → fun(1) + 2
fun(1) → fun(0) + 1
fun(0) → 0

Return path:
0 + 1 = 1
1 + 2 = 3  
3 + 3 = 6
6 + 4 = 10
10 + 5 = 15
```

### Memory Layout (Stack):
```
[fun(5): n=5]
[fun(4): n=4]
[fun(3): n=3]
[fun(2): n=2]
[fun(1): n=1]
[fun(0): n=0]
```

## Example 2: With Static Variable

```cpp
int fun(int n) {
    static int x = 0;  // Static variable - single copy
    
    if (n > 0) {
        x++;               // Increment x before recursive call
        return fun(n-1) + x;  // Adding x at return time
    }
    return 0;
}

int main() {
    int result = fun(5);
    printf("Result: %d\n", result);  // Output: 25
    return 0;
}
```

### Execution Trace for fun(5) with Static Variable:

#### Forward Calls (x increments):
```
fun(5): x=0 → x=1, call fun(4)
fun(4): x=1 → x=2, call fun(3)  
fun(3): x=2 → x=3, call fun(2)
fun(2): x=3 → x=4, call fun(1)
fun(1): x=4 → x=5, call fun(0)
fun(0): returns 0
```

#### Return Path (x=5 for all additions):
```
0 + 5 = 5   (x is 5)
5 + 5 = 10  (x is still 5)
10 + 5 = 15 (x is still 5)
15 + 5 = 20 (x is still 5)
20 + 5 = 25 (x is still 5)
```

### Memory Layout:
```
Code Section: [static int x = 5]  // Single copy

Stack:
[fun(5): n=5]
[fun(4): n=4]
[fun(3): n=3]
[fun(2): n=2]
[fun(1): n=1]
[fun(0): n=0]
```

## Key Differences

| Aspect | Regular Variables | Static Variables |
|--------|------------------|------------------|
| **Copies** | Multiple (one per call) | Single copy |
| **Storage** | Stack | Code section |
| **Lifetime** | Function call duration | Program lifetime |
| **Initialization** | Every function call | Once at program start |
| **Value sharing** | Independent values | Shared across all calls |

## Important Points

### Static Variable Behavior
- **Single instance**: Only one copy exists regardless of recursive depth
- **Persistent state**: Value persists and accumulates across recursive calls
- **Shared access**: All recursive calls access the same memory location
- **Initialization timing**: Initialized once when program loads, not per call

### Tracing Guidelines
- **Don't show static variables** in each step of the recursion tree
- **Maintain separately**: Track static variables as external/global entities
- **Single value**: Use the current value of static variable for all operations

### Global Variables
- **Same behavior** as static variables in recursion context
- **Single copy** shared among all function calls
- **Same result** as static variables would produce
- **Code section storage** like static variables

## Use Cases for Static Variables in Recursion

- **Counting operations**: Track number of recursive calls
- **Accumulating results**: Build up results across calls
- **State preservation**: Maintain state between recursive calls
- **Optimization**: Avoid passing additional parameters

## Memory Organization

```
Program Memory Layout:
┌─────────────────┐
│   Code Section  │ ← Static & Global variables
├─────────────────┤
│      Heap       │
├─────────────────┤
│      Stack      │ ← Local variables & parameters
│  [Activation    │
│   Records]      │
└─────────────────┘
```

## Summary

Static variables in recursion fundamentally change the function's behavior by:
- Providing a **single shared state** across all recursive calls
- **Accumulating changes** that persist throughout the recursion
- **Eliminating independent variable copies** that regular local variables create
- Requiring **different tracing approaches** to understand execution flow

Understanding this distinction is crucial for debugging recursive functions and predicting their behavior when static variables are involved.