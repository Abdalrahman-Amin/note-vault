# Exponent Function - Recursive Implementation & Optimization

## Overview
The exponent function calculates m^n (m raised to the power n), which means multiplying m by itself n times. This video covers two approaches:
1. **Basic Recursive Approach** - Simple but inefficient
2. **Optimized Recursive Approach** - Uses exponentiation by squaring

## Mathematical Definition
- **m^n** = m × m × m × ... (n times)
- **m^0** = 1 (base case)
- **Recursive relation**: m^n = m^(n-1) × m

## Basic Recursive Implementation

### Algorithm Logic
```
power(m, n) = power(m, n-1) × m  when n > 0
power(m, 0) = 1                  when n = 0
```

### Code Implementation
```cpp
int power(int m, int n) {
    if (n == 0) {
        return 1;
    }
    return power(m, n-1) * m;
}
```

### Trace Example: power(2, 9)
```
power(2, 9) = power(2, 8) × 2
power(2, 8) = power(2, 7) × 2
power(2, 7) = power(2, 6) × 2
power(2, 6) = power(2, 5) × 2
power(2, 5) = power(2, 4) × 2
power(2, 4) = power(2, 3) × 2
power(2, 3) = power(2, 2) × 2
power(2, 2) = power(2, 1) × 2
power(2, 1) = power(2, 0) × 2
power(2, 0) = 1

Result: 1 × 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 = 2^9
```

### Complexity Analysis - Basic Approach
- **Time Complexity**: O(n) - Makes n+1 recursive calls
- **Space Complexity**: O(n) - Stack depth is n+1
- **Number of Multiplications**: n

## Optimized Recursive Implementation (Exponentiation by Squaring)

### Key Insight
Instead of reducing power by 1 each time, we can reduce it by half:
- If n is **even**: m^n = (m²)^(n/2)
- If n is **odd**: m^n = m × (m²)^((n-1)/2)

### Algorithm Logic
```
power(m, n) = 1                           when n = 0
power(m, n) = power(m², n/2)              when n is even
power(m, n) = m × power(m², (n-1)/2)      when n is odd
```

### Code Implementation
```cpp
int power(int m, int n) {
    if (n == 0) {
        return 1;
    }
    
    if (n % 2 == 0) {
        // n is even
        return power(m * m, n / 2);
    } else {
        // n is odd
        return m * power(m * m, (n - 1) / 2);
    }
}
```

### Trace Example: power(2, 9)
```
power(2, 9)     [n=9, odd]  = 2 × power(2², 4)
power(4, 4)     [n=4, even] = power(4², 2)
power(16, 2)    [n=2, even] = power(16², 1)
power(256, 1)   [n=1, odd]  = 256 × power(256², 0)
power(65536, 0) [n=0]       = 1

Working backwards:
power(256, 1) = 256 × 1 = 256
power(16, 2) = 256
power(4, 4) = 256
power(2, 9) = 2 × 256 = 512 = 2^9
```

### Multiplication Count in Optimized Version
1. `m * m` in first call (2 × 2 = 4)
2. `m * m` in second call (4 × 4 = 16) 
3. `m * m` in third call (16 × 16 = 256)
4. `m * m` in fourth call (256 × 256 = 65536)
5. Final multiplication: `2 × 256 = 512`

**Total: 5 multiplications** (compared to 9 in basic approach)

### Complexity Analysis - Optimized Approach
- **Time Complexity**: O(log n) - Power reduces by half each time
- **Space Complexity**: O(log n) - Stack depth is log₂(n)
- **Number of Multiplications**: O(log n)

## Comparison Summary

| Aspect | Basic Recursive | Optimized Recursive |
|--------|----------------|-------------------|
| Time Complexity | O(n) | O(log n) |
| Space Complexity | O(n) | O(log n) |
| Multiplications for 2^9 | 9 | 5 |
| Function Calls for 2^9 | 10 | 5 |

## Key Takeaways
1. **Basic recursion** is simple to understand but inefficient for large powers
2. **Exponentiation by squaring** dramatically improves performance
3. The optimized approach reduces both time and space complexity from linear to logarithmic
4. This technique is widely used in cryptography and competitive programming

## Practice Exercise
Implement the iterative version of both basic and optimized exponentiation functions using loops instead of recursion.

## Applications
- **Cryptography**: RSA encryption uses modular exponentiation
- **Computer Graphics**: Matrix transformations
- **Mathematical Computing**: Large number calculations
- **Competitive Programming**: Problems involving large powers