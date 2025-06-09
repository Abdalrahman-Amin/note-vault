# Factorial Function - Recursive Implementation

## Overview
The factorial function calculates n! (n factorial), which is the product of all positive integers from 1 to n. This video demonstrates how to implement factorial using recursion and draws parallels with the sum of natural numbers problem.

## Mathematical Definition
- **n!** = 1 × 2 × 3 × ... × n (for n > 0)
- **0!** = 1 (special case)
- **1!** = 1

### Examples
```
5! = 1 × 2 × 3 × 4 × 5 = 120
3! = 1 × 2 × 3 = 6
2! = 1 × 2 = 2
1! = 1
0! = 1
```

## Recursive Definition
The key insight is that factorial can be broken down recursively:
- **n!** = (n-1)! × n (when n > 0)
- **0!** = 1 (base case)

### Mathematical Breakdown
```
n! = 1 × 2 × 3 × ... × (n-1) × n
   = [1 × 2 × 3 × ... × (n-1)] × n
   = (n-1)! × n
```

## Recursive Implementation

### Algorithm Logic
```
factorial(n) = factorial(n-1) × n  when n > 0
factorial(0) = 1                   when n = 0
```

### Code Implementation
```cpp
int factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return factorial(n - 1) * n;
}
```

### Trace Example: factorial(5)
```
factorial(5) = factorial(4) × 5
factorial(4) = factorial(3) × 4
factorial(3) = factorial(2) × 3
factorial(2) = factorial(1) × 2
factorial(1) = factorial(0) × 1
factorial(0) = 1

Working backwards:
factorial(1) = 1 × 1 = 1
factorial(2) = 1 × 2 = 2
factorial(3) = 2 × 3 = 6
factorial(4) = 6 × 4 = 24
factorial(5) = 24 × 5 = 120
```

### Call Stack Visualization
```
factorial(5)
  └── factorial(4) × 5
      └── factorial(3) × 4
          └── factorial(2) × 3
              └── factorial(1) × 2
                  └── factorial(0) × 1
                      └── return 1
```

## Complexity Analysis

### Time Complexity
- **O(n)** - Makes n+1 recursive calls (from n down to 0)
- Each call performs one multiplication operation

### Space Complexity
- **O(n)** - Stack depth is n+1 activation records
- Each recursive call creates a new stack frame

### Memory Usage Example
For `factorial(5)`:
- **Number of calls**: 6 (factorial(5) through factorial(0))
- **Stack height**: 6 activation records
- **Memory consumption**: O(n) where n = 5

## Iterative Implementation (Exercise)

While not covered in the video, here's the iterative approach for comparison:

```cpp
int factorial_iterative(int n) {
    int result = 1;
    for (int i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

### Iterative vs Recursive Comparison
| Aspect | Recursive | Iterative |
|--------|-----------|-----------|
| Time Complexity | O(n) | O(n) |
| Space Complexity | O(n) | O(1) |
| Code Simplicity | Very Simple | Simple |
| Stack Usage | High | Low |

## Comparison with Sum of Natural Numbers

The factorial function is very similar to the sum of natural numbers function:

```cpp
// Sum of natural numbers
int sum(int n) {
    if (n == 0) {
        return 0;    // Addition identity
    }
    return sum(n - 1) + n;  // Addition operation
}

// Factorial
int factorial(int n) {
    if (n == 0) {
        return 1;    // Multiplication identity
    }
    return factorial(n - 1) * n;  // Multiplication operation
}
```

### Key Differences
- **Base case**: Sum returns 0, factorial returns 1
- **Operation**: Sum uses addition (+), factorial uses multiplication (×)
- **Identity element**: 0 for addition, 1 for multiplication

## Important Notes

### Special Cases
- **0!** = 1 by mathematical convention
- **1!** = 1
- Factorial is undefined for negative numbers

### Practical Considerations
- **Integer overflow**: Factorial values grow very quickly
  - 13! = 6,227,020,800 (exceeds 32-bit integer range)
  - Consider using `long long` or arbitrary precision libraries for large values
- **Stack overflow**: Deep recursion can cause stack overflow for large n

### Example of Rapid Growth
```
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
10! = 3,628,800
12! = 479,001,600
13! = 6,227,020,800 (overflow in 32-bit int)
```

## Applications
- **Combinatorics**: Permutations and combinations (nPr, nCr)
- **Probability**: Calculating probabilities in discrete events
- **Series expansions**: Taylor/Maclaurin series (e^x, sin x, cos x)
- **Algorithm analysis**: Counting operations in certain algorithms

## Key Takeaways
1. **Direct translation**: Mathematical recursive definitions translate easily to code
2. **Base case importance**: Always handle the terminating condition (n = 0)
3. **Similarity patterns**: Many recursive problems follow similar structures
4. **Space-time tradeoff**: Recursive solutions are elegant but use more memory
5. **Practical limits**: Consider overflow and stack limitations for large inputs

## Practice Exercises
1. Implement the iterative version of factorial
2. Write a function to calculate double factorial (n!! = n × (n-2) × (n-4) × ...)
3. Implement factorial using tail recursion optimization
4. Create a memoized version to avoid recalculating the same values