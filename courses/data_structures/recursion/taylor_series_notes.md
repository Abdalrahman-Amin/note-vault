# Taylor Series Recursion

## Overview
Taylor series for e^x is an infinite series that approximates the exponential function:
```
e^x = 1 + x/1! + x²/2! + x³/3! + x⁴/4! + ... + xⁿ/n!
```

## Problem Analysis
The Taylor series involves **three main operations**:
1. **Summation** of terms
2. **Power calculation** (x raised to increasing powers)
3. **Factorial calculation** (denominators: 1!, 2!, 3!, ...)

### Previous Recursive Solutions
We've already seen recursive functions for individual operations:

```cpp
// Sum of first n natural numbers
int sum(int n) {
    if (n == 0) return 0;
    return sum(n-1) + n;
}

// Factorial of n
int factorial(int n) {
    if (n == 0) return 1;
    return factorial(n-1) * n;
}

// Power function (x^n)
int power(int x, int n) {
    if (n == 0) return 1;
    return power(x, n-1) * x;
}
```

## Challenge: Multiple Operations in One Function
- A function can only **return one value**
- Taylor series needs to track **power, factorial, and sum simultaneously**
- **Solution**: Use static variables to maintain state across recursive calls

## Method 1: Using Static Variables

### Key Concept
- Static variables maintain their values across function calls
- Use static variables to track:
  - `p`: Current power of x
  - `f`: Current factorial value

### Algorithm Trace
For `e(x, 4)` (5 terms total):
```
Call: e(x, 4) → e(x, 3) → e(x, 2) → e(x, 1) → e(x, 0)

Base case: e(x, 0) returns 1

Returning phase:
- e(x, 1): p = x¹, f = 1! → returns 1 + x/1
- e(x, 2): p = x², f = 2! → returns previous + x²/2
- e(x, 3): p = x³, f = 3! → returns previous + x³/6
- e(x, 4): p = x⁴, f = 4! → returns previous + x⁴/24
```

### Implementation

```cpp
#include <iostream>
using namespace std;

double taylorSeries(double x, int n) {
    static double p = 1;  // Power term (starts with x⁰ = 1)
    static double f = 1;  // Factorial term (starts with 0! = 1)
    double result;
    
    // Base case
    if (n == 0) {
        return 1;
    }
    
    // Recursive call
    result = taylorSeries(x, n - 1);
    
    // Update static variables for current term
    p = p * x;    // Increment power: x¹, x², x³, ...
    f = f * n;    // Increment factorial: 1!, 2!, 3!, ...
    
    // Return previous result + current term
    return result + (p / f);
}

// Helper function to reset static variables
void resetStatics() {
    static double p = 1;
    static double f = 1;
    p = 1;
    f = 1;
}
```

### Example Usage
```cpp
int main() {
    double x = 2.0;
    int terms = 10;
    
    double result = taylorSeries(x, terms);
    cout << "e^" << x << " ≈ " << result << endl;
    
    // Reset for next calculation
    resetStatics();
    
    return 0;
}
```

## Step-by-Step Execution

### For `taylorSeries(2, 4)`:

| Call | n | Action | p value | f value | Term added | Running sum |
|------|---|--------|---------|---------|------------|-------------|
| 1 | 4 | Call(3) | 1 | 1 | - | - |
| 2 | 3 | Call(2) | 1 | 1 | - | - |
| 3 | 2 | Call(1) | 1 | 1 | - | - |
| 4 | 1 | Call(0) | 1 | 1 | - | - |
| 5 | 0 | Return 1 | 1 | 1 | 1 | 1 |
| Return | 1 | p*=x, f*=1 | 2 | 1 | 2/1 = 2 | 1 + 2 = 3 |
| Return | 2 | p*=x, f*=2 | 4 | 2 | 4/2 = 2 | 3 + 2 = 5 |
| Return | 3 | p*=x, f*=3 | 8 | 6 | 8/6 = 1.33 | 5 + 1.33 = 6.33 |
| Return | 4 | p*=x, f*=4 | 16 | 24 | 16/24 = 0.67 | 6.33 + 0.67 = 7 |

## Important Notes

### Advantages
- **Elegant solution** combining multiple recursive operations
- **Memory efficient** - uses static variables instead of passing multiple parameters
- **Clear logic flow** - mirrors the mathematical definition

### Disadvantages
- **Static variables persist** between function calls
- **Not thread-safe** - static variables are shared
- **Side effects** - function behavior depends on previous calls

### Best Practices
- Reset static variables between different calculations
- Consider thread safety in multi-threaded applications
- Document the use of static variables clearly

## Time & Space Complexity
- **Time Complexity**: O(n) - single recursive call chain
- **Space Complexity**: O(n) - recursive call stack depth
- **Auxiliary Space**: O(1) - only static variables used

## Alternative Approach Preview
The instructor mentions a **faster method** with fewer multiplications will be covered next, which likely refers to:
- Horner's method for polynomial evaluation
- Iterative approach with better computational efficiency

## Key Takeaways
1. **Static variables** enable complex recursive operations with multiple state tracking
2. **Returning phase** is where the actual computation and accumulation happens
3. **Mathematical series** can be elegantly implemented using recursion
4. **Trade-offs exist** between code elegance and potential side effects

---
*Next: Optimized Taylor Series implementation with reduced multiplications*