# Abstract Data Types (ADT) - Course Notes

## 📚 Topic Overview
**Abstract Data Type (ADT)** - A fundamental concept in computer science that combines data representation with operations, while hiding internal implementation details.

---

## 🔍 Key Definitions

### Data Type
A data type consists of two essential components:
1. **Representation of data** - How data is stored in memory
2. **Operations on data** - What actions can be performed on the data

### Abstract
- **Meaning**: Hiding internal details
- **Purpose**: Allow usage without knowing implementation specifics
- **Benefit**: Simplifies programming by focusing on what operations do, not how they work

---

## 💡 Real-World Example: Integer Data Type

### Representation
- **Memory allocation**: 2 bytes (16 bits) in C/C++
- **Structure**:
  - 1 bit → Sign bit (positive/negative)
  - 15 bits → Actual number storage

### Operations Available
- **Arithmetic**: `+`, `-`, `*`, `/`, `%`
- **Relational**: `<`, `>`, `<=`, `>=`, `==`, `!=`
- **Increment/Decrement**: `++`, `--` (pre and post)

### Why It's "Abstract"
- We use these operations without understanding binary implementation
- Internal details are hidden from the programmer
- Focus is on functionality, not implementation

---

## 🎯 Connection to Object-Oriented Programming

ADT concept became prominent with **Object-Oriented Programming** because:
- Classes allow creation of custom data types
- Encapsulation hides internal details
- Users interact through defined interfaces (methods)

---

## 📋 Practical Example: List ADT

### Sample List
```
Elements: [8, 2, 9, 4, 5, 10, 12]
Indices:   0  1  2  3  4   5   6
```

### Data Representation Requirements
1. **Storage space** for elements
2. **Capacity** (maximum size)
3. **Size** (current number of elements)

### Implementation Options
- **Array-based** implementation
- **Linked List** implementation
- *Note: Choice of implementation doesn't affect the interface*

---

## 🛠️ List ADT Operations

### Core Operations

#### 1. **Add/Append**
- **Purpose**: Add element to end of list
- **Example**: Add `15` → `[8, 2, 9, 4, 5, 10, 12, 15]`

#### 2. **Insert at Index**
- **Purpose**: Add element at specific position
- **Process**: Shift existing elements to make space
- **Example**: Insert `20` at index 6
  - Before: `[8, 2, 9, 4, 5, 10, 12]`
  - After: `[8, 2, 9, 4, 5, 10, 20, 12]`

#### 3. **Remove**
- **Purpose**: Delete element at given index
- **Process**: Shift remaining elements to fill gap
- **Example**: Remove index 6 → shifts elements left

#### 4. **Set/Replace**
- **Purpose**: Change element at specific index
- **Example**: Set index 3 to `25` → `[8, 2, 9, 25, 5, 10, 12]`

#### 5. **Get**
- **Purpose**: Retrieve element at given index
- **Example**: Get index 5 → returns `10`

#### 6. **Search/Contains**
- **Purpose**: Find if element exists and return its index
- **Example**: Search for `9` → returns index `2`
- **Alternative**: Check if list contains specific element

#### 7. **Sort**
- **Purpose**: Arrange elements in specific order
- **Result**: Organized list (ascending/descending)

### Additional Operations
- **Reverse**: Flip element order
- **Merge**: Combine multiple lists
- **Split**: Divide list into parts

---

## 🏗️ ADT Implementation in Programming

### C++ Class Structure
```cpp
class ListADT {
private:
    // Data representation (hidden)
    // - Array or linked list
    // - Capacity and size variables
    
public:
    // Public operations (interface)
    // - add(), insert(), remove()
    // - get(), set(), search()
    // - sort(), etc.
};
```

### Key Principles
- **Encapsulation**: Internal details are private
- **Interface**: Public methods define available operations
- **Abstraction**: Users don't need to know implementation

---

## 🎓 Important Takeaways

1. **ADT = Data + Operations** combined with abstraction
2. **Implementation independence**: Same interface, different internal structures
3. **Common in OOP**: Classes naturally represent ADTs
4. **Foundation for data structures**: Stack, Queue, Tree, Graph, Hash Table, etc.
5. **Focus on behavior**: What operations do, not how they're implemented

---

## 🔗 Course Connection
This ADT concept will be fundamental for understanding:
- **Arrays and Linked Lists** (implementation choices)
- **Stacks and Queues** (specialized list operations)
- **Trees and Graphs** (hierarchical and network structures)
- **Hash Tables** (key-value storage)

---

## 📝 Quick Review Questions
1. What are the two components of any data type?
2. Why is the term "abstract" important in ADT?
3. How does a List ADT differ from a simple array?
4. What happens when you insert an element in the middle of a list?
5. Why can the same ADT have different implementations?

---

*Next Topic: [Next lesson title here]*