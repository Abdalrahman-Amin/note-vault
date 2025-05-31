# 💾 Static vs Dynamic Memory Allocation - Course Notes

## 📋 Table of Contents
- [Memory Fundamentals](#memory-fundamentals)
- [Program Memory Layout](#program-memory-layout)
- [Static Memory Allocation (Stack)](#static-memory-allocation-stack)
- [Dynamic Memory Allocation (Heap)](#dynamic-memory-allocation-heap)
- [Stack vs Heap Comparison](#stack-vs-heap-comparison)
- [Memory Management Best Practices](#memory-management-best-practices)
- [Key Takeaways](#key-takeaways)
- [Next Steps](#next-steps)

---

## 🧠 Memory Fundamentals

### Memory Structure Basics
- **Memory Unit**: Divided into **addressable bytes**
- **Addressing**: Linear addressing system (0, 1, 2, 3... not coordinate-based)
- **Course Assumption**: **64KB memory** (one memory segment)
  - Address range: **0 to 65535** 
  - Total bytes: **65,536 = 64 × 1024**

### Visual Representation:
```
Memory Layout (64KB):
┌─────┬─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  2  │  3  │ ... │65535│
└─────┴─────┴─────┴─────┴─────┴─────┘
```

> **Real World**: Modern systems use GB of RAM, but it's divided into 64KB segments for management

---

## 🏗️ Program Memory Layout

When a program runs, the **64KB memory** is divided into **3 sections**:

```
┌─────────────────┐ ← Address 65535
│                 │
│      STACK      │ ← Function calls, local variables
│        ↓        │   (grows downward)
├─────────────────┤
│                 │
│      HEAP       │ ← Dynamic allocation
│        ↑        │   (grows upward)  
├─────────────────┤
│   CODE SECTION  │ ← Program machine code
│   (Fixed Size)  │   (loaded from disk)
└─────────────────┘ ← Address 0
```

### Section Details:

| Section | Purpose | Size | Growth |
|---------|---------|------|--------|
| **Code** | Program machine code | Fixed | Static |
| **Stack** | Function calls, local variables | Variable | Downward ↓ |
| **Heap** | Dynamic allocation | Variable | Upward ↑ |

---

## 📚 Static Memory Allocation (Stack)

### Definition
> **Static**: Memory size determined at **compile time**

### How It Works

#### Example Code:
```c
int main() {
    int a;     // 2 bytes (assumed)
    float b;   // 4 bytes (assumed)
    return 0;
}
```

#### Memory Allocation:
```
Stack Memory:
┌─────────────────┐ ← Stack grows downward
│   float b (4B)  │
├─────────────────┤
│   int a (2B)    │ ← Activation Record of main()
└─────────────────┘
Total: 6 bytes allocated for main()
```

### Function Call Sequence

#### Example Code:
```c
void fun2(int i) {
    int a;  // Local variable
}

void fun1() {
    int x;  // Local variable
    fun2(x); // Function call
}

int main() {
    int a, b;  // Local variables
    fun1();    // Function call
    return 0;
}
```

#### Stack Evolution:

**Step 1: main() starts**
```
┌─────────────────┐
│   main(): a, b  │ ← Current execution
└─────────────────┘
```

**Step 2: main() calls fun1()**
```
┌─────────────────┐
│   fun1(): x     │ ← Current execution
├─────────────────┤
│   main(): a, b  │ ← Waiting
└─────────────────┘
```

**Step 3: fun1() calls fun2()**
```
┌─────────────────┐
│  fun2(): i, a   │ ← Current execution
├─────────────────┤
│   fun1(): x     │ ← Waiting
├─────────────────┤
│   main(): a, b  │ ← Waiting
└─────────────────┘
```

**Step 4: fun2() ends**
```
┌─────────────────┐
│   fun1(): x     │ ← Current execution (resumed)
├─────────────────┤
│   main(): a, b  │ ← Waiting
└─────────────────┘
```

**Step 5: fun1() ends**
```
┌─────────────────┐
│   main(): a, b  │ ← Current execution (resumed)
└─────────────────┘
```

### Stack Mechanism (LIFO)
- **Last In, First Out** behavior
- **Automatic management**: No programmer intervention needed
- **Activation Record**: Memory block for each function
- **Currently executing function** always at the **top**

### Key Characteristics:
✅ **Automatic**: Allocation and deallocation handled automatically  
✅ **Fast**: Simple pointer manipulation  
✅ **Organized**: Well-structured memory layout  
✅ **Predictable**: Size known at compile time  

---

## 🌊 Dynamic Memory Allocation (Heap)

### Definition
> **Dynamic**: Memory allocated at **runtime** as needed

### Heap Characteristics

#### 1. **Unorganized Memory**
- Unlike stack's organized structure
- Memory can be allocated/deallocated in any order

#### 2. **Resource-Like Behavior**
- Must be **explicitly requested**
- Must be **explicitly released**
- Similar to printer resource: request → use → release

#### 3. **Indirect Access Only**
- Program cannot directly access heap memory
- Must use **pointers** to access heap data

### Memory Allocation Process

#### C++ Example:
```cpp
int main() {
    int* p;                    // Pointer (2 bytes in stack)
    p = new int[5];           // Allocate array in heap
    // Use the array...
    delete[] p;               // Deallocate memory
    p = nullptr;              // Good practice
}
```

#### C Example:
```c
int main() {
    int* p;                           // Pointer (2 bytes in stack)
    p = (int*)malloc(5 * sizeof(int)); // Allocate array in heap
    // Use the array...
    free(p);                          // Deallocate memory
    p = NULL;                         // Good practice
}
```

### Visual Representation:

```
Stack Section:          Heap Section:
┌─────────────┐        ┌─────┬─────┬─────┬─────┬─────┐
│  p (2 bytes)│───────→│ int │ int │ int │ int │ int │
│   (500)     │        └─────┴─────┴─────┴─────┴─────┘
└─────────────┘         Address: 500 (5 integers × 2 bytes each)
   main() activation record
```

### Memory Management Lifecycle:

1. **Request**: `new` or `malloc()`
2. **Use**: Access through pointer
3. **Release**: `delete`/`delete[]` or `free()`
4. **Nullify**: Set pointer to `nullptr`/`NULL`

### ⚠️ Memory Leak Warning

#### What happens without proper cleanup:
```cpp
int* p = new int[5];  // Allocate memory
p = nullptr;          // Pointer lost, but memory still allocated!
// Memory leak! The allocated space can't be accessed or freed
```

#### Correct approach:
```cpp
int* p = new int[5];  // Allocate memory
// Use the memory...
delete[] p;           // Free the memory
p = nullptr;          // Reset pointer
```

### Key Characteristics:
⚠️ **Manual Management**: Programmer responsible for allocation/deallocation  
🐌 **Slower**: More complex allocation mechanism  
🌊 **Flexible**: Can allocate any size at runtime  
💥 **Risk**: Memory leaks if not properly managed  

---

## ⚖️ Stack vs Heap Comparison

| Aspect | Stack | Heap |
|--------|-------|------|
| **Allocation Time** | Compile time (Static) | Runtime (Dynamic) |
| **Management** | Automatic | Manual |
| **Speed** | Fast | Slower |
| **Organization** | Organized (LIFO) | Unorganized |
| **Access** | Direct | Through pointers |
| **Size Limits** | Limited by stack size | Limited by available memory |
| **Memory Leaks** | Not possible | Possible if not freed |
| **Fragmentation** | No fragmentation | Can fragment |
| **Thread Safety** | Each thread has own stack | Shared among threads |

### When to Use Each:

#### Use Stack When:
✅ Size known at compile time  
✅ Small, temporary data  
✅ Function parameters and local variables  
✅ Need automatic cleanup  

#### Use Heap When:
✅ Size unknown until runtime  
✅ Large data structures  
✅ Need data to persist beyond function scope  
✅ Dynamic data structures (linked lists, trees)  

---

## 🛡️ Memory Management Best Practices

### Stack Best Practices:
1. **Avoid deep recursion** - can cause stack overflow
2. **Don't return addresses** of local variables
3. **Keep local variables small** when possible

### Heap Best Practices:
1. **Always pair allocation with deallocation**:
   ```cpp
   new    ↔ delete
   new[]  ↔ delete[]
   malloc ↔ free
   ```

2. **Check for allocation failure**:
   ```cpp
   int* p = new(nothrow) int[1000000];
   if (p == nullptr) {
       // Handle allocation failure
   }
   ```

3. **Set pointers to null after deletion**:
   ```cpp
   delete[] p;
   p = nullptr;  // Prevent accidental reuse
   ```

4. **Use smart pointers** (modern C++):
   ```cpp
   std::unique_ptr<int[]> p(new int[5]);
   // Automatic cleanup when p goes out of scope
   ```

---

## 🎯 Key Takeaways

### 💡 Essential Concepts:
1. **Two Types**: Static (compile-time) vs Dynamic (runtime)
2. **Memory Sections**: Code, Stack, Heap serve different purposes
3. **Stack = Automatic**: Variables declared in functions
4. **Heap = Manual**: Explicitly requested with new/malloc

### 📊 Data Type Assumptions (Course Context):
- **Integer**: 2 bytes (16-bit compiler like Turbo C)
- **Float**: 4 bytes
- **Pointer**: 2 bytes (follows integer size)

> **Note**: Modern compilers typically use 4-byte integers (32-bit) or 8-byte (64-bit)

### 🔄 Memory Lifecycle:
```
Stack: Declare → Auto-allocate → Auto-deallocate
Heap:  Request → Manual-allocate → Manual-deallocate
```

### ⚠️ Common Mistakes:
- **Stack**: Returning local variable addresses
- **Heap**: Forgetting to free allocated memory
- **Both**: Accessing memory after it's been freed/out of scope

---

## 🚀 Next Steps

### Coming Up:
- **Data Structures Introduction**: How these memory concepts apply to data structures
- **Abstract Data Types (ADT)**: Logical vs Physical data organization
- **Arrays vs Linked Lists**: Different approaches to data storage

### 🔗 Connection to Data Structures:
- **Arrays**: Can be allocated in stack (fixed size) or heap (dynamic)
- **Linked Lists**: Always use heap allocation for nodes
- **Choice depends on**: Known vs unknown size requirements

### 📚 Study Checklist:
- [ ] Understand the 3-section memory model
- [ ] Practice tracing function call stack evolution
- [ ] Write programs using both stack and heap allocation
- [ ] Identify memory leaks in sample code
- [ ] Compare allocation strategies for different scenarios

---

## 🔍 Quick Reference

### Allocation Syntax:
```cpp
// Stack allocation
int arr[5];              // Fixed size array
int x = 10;              // Single variable

// Heap allocation (C++)
int* p = new int;        // Single integer
int* arr = new int[5];   // Dynamic array
delete p;                // Free single
delete[] arr;            // Free array

// Heap allocation (C)
int* p = malloc(sizeof(int));
int* arr = malloc(5 * sizeof(int));
free(p);                 // Free both single and array
```

### Memory Debugging Tips:
- Use **valgrind** (Linux) or **Application Verifier** (Windows)
- Enable **compiler warnings** for memory issues
- Use **static analysis tools** to detect potential leaks
- Practice with **memory debugging tools** in your IDE

---

*Remember: Good memory management is the foundation of efficient data structures. Master these concepts before moving to complex data structures!*