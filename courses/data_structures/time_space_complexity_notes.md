# Time and Space Complexity - Course Notes

## 📚 Topic Overview
**Complexity Analysis** - Understanding how algorithms perform in terms of time (execution speed) and space (memory usage) as input size grows. Essential for writing efficient code and choosing optimal solutions.

---

## 🕒 Time Complexity Fundamentals

### What is Time Complexity?
- **Definition**: Measure of how much time an algorithm takes based on input size
- **Real-world analogy**: Like asking "How long does it take to complete a task?"
- **Key insight**: Time depends on the **procedure/algorithm** you choose, not just the problem
- **Representation**: We use **Order notation** - Order(n), where n = input size

### Why It Matters
- **Machine efficiency**: Computers should be faster than manual work
- **Scalability**: Algorithm that works for 10 items should work for 10,000,000 items
- **Resource planning**: Knowing time helps choose best approach

---

## 📊 Core Time Complexity Patterns

### **Pattern 1: Linear Processing - Order(n)**
```
Array: [8, 2, 9, 4, 5, 10, 12, 15, 6, 3]
       └─────── n elements ──────────┘
```

**Operations that take Order(n) time:**
- **Adding all elements** - visit each once
- **Searching for element** - might check all elements
- **Finding maximum/minimum** - compare each element

**Code Pattern:**
```c
for(i = 0; i < n; i++) {
    // Process each element once
    // Add, search, compare, etc.
}
```

**Key Rule**: One loop through n elements = Order(n)

### **Pattern 2: Nested Processing - Order(n²)**
```
For each element: [8, 2, 9, 4, 5...]
  Compare with:   [8, 2, 9, 4, 5...] ← All elements again
```

**Operations that take Order(n²) time:**
- **Bubble sort** - compare each with every other
- **Nested comparisons** - checking pairs of elements
- **Matrix operations** - processing 2D arrays

**Code Pattern:**
```c
for(i = 0; i < n; i++) {        // n iterations
    for(j = 0; j < n; j++) {    // n iterations each
        // Process pair (i,j)
    }
}
```

**Formula**: n × n = n² operations

### **Pattern 3: Triangular Processing - Still Order(n²)**
```
Element 1: Compare with [2, 9, 4, 5, 10, 12, 15, 6, 3] → 9 comparisons
Element 2: Compare with [9, 4, 5, 10, 12, 15, 6, 3]    → 8 comparisons
Element 3: Compare with [4, 5, 10, 12, 15, 6, 3]       → 7 comparisons
...
```

**Mathematical breakdown:**
- Total comparisons: (n-1) + (n-2) + (n-3) + ... + 1
- Formula: `(n × (n-1))/2 = (n² - n)/2`
- **Degree**: Still n² → Order(n²)

**Code Pattern:**
```c
for(i = 0; i < n; i++) {
    for(j = i+1; j < n; j++) {  // Start from i+1, not 0
        // Process unique pairs
    }
}
```

### **Pattern 4: Divide and Conquer - Order(log n)**
```
Array: [8, 2, 9, 4, 5, 10, 12, 15, 6, 3]
Step 1: Process middle element (5)
Step 2: Process middle of chosen half
Step 3: Process middle of chosen quarter
...until 1 element remains
```

**Operations that take Order(log n) time:**
- **Binary search** in sorted array
- **Tree traversal** along height
- **Divide by 2** repeatedly

**Code Pattern:**
```c
for(i = n; i > 1; i = i/2) {
    // Process current element
    // Each iteration halves the problem
}

// Alternative with while loop
i = n;
while(i > 1) {
    // Process
    i = i/2;
}
```

**Key insight**: Dividing by 2 repeatedly until reaching 1 = log₂(n) steps

---

## 🎯 Visual Time Complexity Comparison

| Input Size (n) | Order(1) | Order(log n) | Order(n) | Order(n²) | Order(n³) |
|----------------|----------|--------------|-----------|-----------|-----------|
| 10             | 1        | 3            | 10        | 100       | 1,000     |
| 100            | 1        | 7            | 100       | 10,000    | 1,000,000 |
| 1,000          | 1        | 10           | 1,000     | 1,000,000 | 1,000,000,000 |

**Performance ranking**: Order(1) > Order(log n) > Order(n) > Order(n²) > Order(n³)

---

## 🔍 Data Structure Complexity Analysis

### **Arrays/Lists**
```
[8, 2, 9, 4, 5, 10, 12, 15, 6, 3] ← n elements
```
- **Access all elements**: Order(n)
- **Search element**: Order(n) worst case
- **Sort elements**: Order(n²) basic algorithms

### **Linked Lists**
```
8 → 2 → 9 → 4 → 5 → 10 → 12 → 15 → 6 → 3 → NULL
```
- **Same as arrays** for traversal operations
- **Time complexity**: Order(n) for most operations

### **Matrices (2D Arrays)**
```
4×4 Matrix = 16 elements = n² elements (if n×n matrix)
```
- **Process all elements**: Order(n²)
- **Process one row**: Order(n)
- **Process one column**: Order(n)

**Code Pattern:**
```c
for(i = 0; i < n; i++) {        // Rows
    for(j = 0; j < n; j++) {    // Columns
        // Process matrix[i][j]
    }
}
```

### **Trees**
```
      Root
     /    \
   Node   Node
   / \     / \
  □   □   □   □
```
- **Process along height**: Order(log n) - following one path
- **Process all nodes**: Order(n) - visiting every node
- **Height of balanced tree**: log₂(n) levels

---

## 💾 Space Complexity Fundamentals

### What is Space Complexity?
- **Definition**: Amount of memory consumed during program execution
- **Focus**: How space grows with input size, not exact bytes
- **Goal**: Understand memory requirements for scalability

### Space Analysis Examples

#### **Basic Data Structures**
```c
int array[n];           // Space: Order(n)
```

#### **Linked List**
```c
struct Node {
    int data;           // Data storage
    struct Node* next;  // Pointer storage
};
```
- **Space**: Order(n) for data + Order(n) for pointers = Order(2n) = **Order(n)**

#### **Matrix**
```c
int matrix[n][n];       // Space: Order(n²)
```

#### **Tree**
```c
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};
```
- **Space**: Order(n) - one node per element

---

## 🎯 Analysis Methodology

### **From Code Analysis**
1. **Count the loops**:
   - 1 loop = Order(n)
   - 2 nested loops = Order(n²)
   - 3 nested loops = Order(n³)

2. **Check loop behavior**:
   - `i++` = linear growth
   - `i = i/2` = logarithmic growth
   - `i = i*2` = exponential growth

3. **Focus on dominant term**:
   - `n² + n + 1` → Order(n²)
   - `n log n + n` → Order(n log n)

### **From Process Analysis**
1. **Understand the algorithm**
2. **Count operations on input size**
3. **Express as mathematical function**
4. **Find the dominant term (highest degree)**

---

## 🚀 Quick Recognition Rules

### **Instant Pattern Recognition**
- **Single for loop (0 to n)** → Order(n)
- **Two nested for loops** → Order(n²)
- **Dividing by 2 repeatedly** → Order(log n)
- **Processing all elements once** → Order(n)
- **Comparing every pair** → Order(n²)

### **Common Mistakes to Avoid**
❌ **Wrong**: Assuming all for loops are Order(n)
✅ **Right**: Check what the loop variable does

❌ **Wrong**: Counting lines of code
✅ **Right**: Count operations based on input size

❌ **Wrong**: Including constants in final answer
✅ **Right**: Focus on dominant term only

---

## 💡 Practical Examples

### **Example 1: Finding Maximum**
```c
int max = arr[0];
for(i = 1; i < n; i++) {
    if(arr[i] > max) {
        max = arr[i];
    }
}
```
**Analysis**: Visit each element once → **Order(n)**

### **Example 2: Bubble Sort**
```c
for(i = 0; i < n-1; i++) {
    for(j = 0; j < n-i-1; j++) {
        if(arr[j] > arr[j+1]) {
            // swap
        }
    }
}
```
**Analysis**: Nested loops, comparing pairs → **Order(n²)**

### **Example 3: Binary Search**
```c
while(low <= high) {
    mid = (low + high) / 2;
    if(arr[mid] == target) return mid;
    else if(arr[mid] < target) low = mid + 1;
    else high = mid - 1;
}
```
**Analysis**: Halving search space each time → **Order(log n)**

---

## 🎓 Key Takeaways

1. **Time complexity depends on your algorithm**, not the problem itself
2. **Focus on input size growth**, not exact time measurements
3. **Analyze the procedure first**, then verify with code
4. **Space complexity follows similar patterns** as time complexity
5. **Look for dominant terms** - ignore constants and lower-order terms
6. **Practice pattern recognition** - most algorithms follow common patterns

---

## 📝 Quick Review Questions

1. What's the time complexity of searching an unsorted array?
2. Why is nested loop processing Order(n²) and not Order(2n)?
3. How do you recognize Order(log n) complexity in code?
4. What's the space complexity of a binary tree with n nodes?
5. If an algorithm does (n²-n)/2 operations, what's its time complexity?

**Answers**: 1) Order(n), 2) Inner loop runs n times for each of n outer iterations, 3) Loop variable divides by constant each iteration, 4) Order(n), 5) Order(n²)

---

*Next Topic: Analyzing specific algorithms and code examples*