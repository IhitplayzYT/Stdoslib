# Stdoslib

A functional paradigm based library for C containing many convenient macros and functions for systems programming, data manipulation, and general-purpose utilities.

## Why Stdoslib?

Stdoslib was created to provide C developers with a modern, functional-style library that simplifies common programming tasks. It offers:

- **Type-safe macros** using C11 `_Generic` for type dispatch
- **Memory management utilities** with custom allocators and helpers
- **Data structures** (Vector, Iterator, Tuple) with functional APIs
- **String manipulation** functions for common operations
- **Bit manipulation** utilities for low-level programming
- **Network utilities** for IP address and port handling
- **Time functions** for performance measurement and time tracking
- **Sorting algorithms** with type-specific implementations
- **Functional programming patterns** like map, filter, reduce (via macros)

## Dependencies

Stdoslib has no external dependencies. It only requires:
- A C compiler supporting C11 or later (GCC, Clang, etc.)
- Standard C library (`stdio.h`, `stdlib.h`, `unistd.h`)
- GNU C extensions (for attributes like `__attribute__((visibility))`)

## Installation

### From Source

1. Clone the repository:
```bash
git clone https://github.com/IhitplayzYT/Stdoslib.git
cd Stdoslib
```

2. Build the library:
```bash
make
```

This will generate:
- `libstdoslib.a` - Static library
- `libstdoslib.so` - Shared library

3. Install system-wide (optional):
```bash
sudo make install
```

This installs to:
- Header: `/usr/local/include/stdoslib.h`
- Libraries: `/usr/local/lib/`

### Uninstall

To uninstall from system:
```bash
cd Stdoslib
sudo make uninstall
```

## Usage

### Basic Example

```c
#include <stdoslib.h>

int main() {
    // String manipulation
    i8 *str = (i8*)"Hello";
    i8 *upper = touppr(str);
    printf("Uppercase: %s\n", upper);
    
    // Memory operations
    i8 buffer[256];
    fill(buffer, 256, 0);
    
    // Type conversions
    i32 num = stoi("12345");
    printf("Number: %d\n", num);
    
    // Sorting
    i32 arr[] = {5, 2, 8, 1, 9};
    sort(arr, 5, 1);  // Ascending sort
    
    // Vector usage
    Vector *vec = new(Vector, (void*)42, sizeof(i32));
    vec->append(vec, (void*)100);
    
    return 0;
}
```

### Compilation

Compile your program with Stdoslib:

```bash
# Using static library
gcc -o myprogram myprogram.c -L. -lstdoslib

# Using shared library
gcc -o myprogram myprogram.c -L. -lstdoslib

# With library in standard path
gcc -o myprogram myprogram.c -lstdoslib
```

## Features & API Overview

### Type Definitions

Stdoslib provides consistent type aliases:
- `i8, i16, i32, i64` - Unsigned integers (8, 16, 32, 64 bit)
- `u8, u16, u32, u64` - Unsigned integers (alternative naming)
- `s8, s16, s32, s64` - Signed integers (8, 16, 32, 64 bit)
- `f32, f64` - Floating point (32, 64 bit)
- `byte` - Unsigned byte
- `boolean` - Boolean type

### Memory Management

```c
// Allocation
void *ptr = alloc(size);
dealloc(ptr);

// Memory operations
fill(dest, len, byte);      // Fill memory with byte
copy(dest, src, len);       // Copy memory
memcopy(dest, src, len);    // Memory copy
zero(src, len);             // Zero memory
memcomp(mem1, mem2, len);   // Compare memory
```

### String Operations

```c
// String manipulation
strcopy(dest, src);         // Copy string
strncopy(dest, src, len);  // Copy n chars
concat(str1, str2);         // Concatenate strings
len(str);                   // String length
strcomp(s1, s2);            // String comparison

// Case conversion
touppr(str);                // To uppercase
tolwr(str);                 // To lowercase
toupprn(str, len);          // To uppercase (n chars)
tolwrn(str, len);           // To lowercase (n chars)

// Search
strchar(str, ch);           // Find character
strcharidx(str, ch);        // Find character index
strstrs(haystack, needle);  // Find substring
strstrsidx(haystack, needle); // Find substring index

// Tokenization
tokenise(str, delimiter);   // Split string into tokens
```

### Data Structures

#### Vector
```c
Vector *vec = new(Vector, data, sizeof(type));
vec->append(vec, item);
vec->pop(vec);
Iterator *it = vec->iterator(vec);
```

#### Iterator
```c
Iterator *it = Iterator_init(vector);
void *item;
while ((item = it->next(it))) {
    // Process item
}
```

#### Tuple
```c
Tuple *tup = new(Tuple, item1, item2, item3, NULL);
tup->add(tup, item4);
```

### Sorting

```c
// Numeric arrays
i32 arr[] = {3, 1, 4, 1, 5};
sort(arr, 5, 1);  // 1 = ascending, 0 = descending

// String arrays
char *strs[] = {"banana", "apple", "cherry"};
sort(strs, 3, 1);
```

### Bit Manipulation

```c
// Single bit operations
getbit(mem, n);       // Get nth bit
setbit(mem, n);       // Set nth bit
unsetbit(mem, n);     // Unset nth bit
flipbit(mem, n);      // Flip nth bit

// Byte operations
flip_byte(ch);        // Flip all bits in byte
invert_bits(str, len); // Invert bits in buffer
```

### Network Utilities

```c
// IP address conversion
i32 addr = ipaddr("192.168.1.1");
i8 *str = ipstr(addr);

// Port conversion
i16 port = net_port(8080);

// Endian conversion
i16 val16 = endian16(x);
i32 val32 = endian32(x);
i64 val64 = endian64(x);
```

### Time Functions

```c
// Performance measurement
i64 ticks = ticks_elapsed();
i64 freq = tick_freq();
i64 seconds = seconds_elapsed();

// Time formatting
Time *t = curr_time();
i8 *formatted = fmttime(t);
```

### Type Conversion

```c
// String to number
i8 n8 = stoi8("123");
i16 n16 = stoi16("12345");
i32 n32 = stoi32("123456789");
i64 n64 = stoi64("1234567890123");
int n = stoi("123");

// Number to string
i8 *hex = ascii2hex(byte);
i8 byte = hex2ascii("FF");
```

### Mathematical Operations

```c
// Power function
double result = pow(base, exponent);

// Division
i16 result = ceil_div(a, b);  // Ceiling division
i16 result = floor_div(a, b);  // Floor division

// Precision
double rounded = precision(value, decimals);
```

### Validation Functions

```c
// Type checking
is_alphabetic(str);     // Check if alphabetic
is_numeric(str);        // Check if numeric
is_alphanumeric(str);  // Check if alphanumeric

// Type assertion
Type t = assert_type(str);  // Returns: t_char, t_int, t_float, t_bool, t_charptr
```

### Utility Macros

```c
// Free multiple pointers
FREE(ptr1, ptr2, ptr3);

// Print arrays
printarr(arr, length);

// Min/Max operations
min(x, y, z, ...);
max(x, y, z, ...);

// Arithmetic operations
sum(x, y, z, ...);
sub(x, y, z, ...);
mul(x, y, z, ...);
div(x, y, z, ...);
```

## Examples

### Example 1: String Processing

```c
#include <stdoslib.h>

int main() {
    i8 *text = (i8*)"Hello World";
    i8 *upper = touppr(text);
    i8 *lower = tolwr(text);
    
    printf("Original: %s\n", text);
    printf("Upper: %s\n", upper);
    printf("Lower: %s\n", lower);
    
    // Tokenize
    Tokens *toks = tokenise(text, ' ');
    print_s_Tok_ret(toks);
    
    return 0;
}
```

### Example 2: Sorting Arrays

```c
#include <stdoslib.h>

int main() {
    i32 numbers[] = {64, 34, 25, 12, 22, 11, 90};
    i16 len = 7;
    
    printf("Before sort: ");
    printarr(numbers, len);
    
    sort(numbers, len, 1);  // Ascending
    
    printf("After sort: ");
    printarr(numbers, len);
    
    return 0;
}
```

### Example 3: Vector Operations

```c
#include <stdoslib.h>

int main() {
    Vector *vec = new(Vector, (void*)10, sizeof(i32));
    
    vec->append(vec, (void*)20);
    vec->append(vec, (void*)30);
    vec->append(vec, (void*)40);
    
    Iterator *it = vec->iterator(vec);
    void *item;
    while ((item = it->next(it))) {
        printf("Item: %d\n", (i32)item);
    }
    
    return 0;
}
```

### Example 4: Bit Manipulation

```c
#include <stdoslib.h>

int main() {
    i8 byte = 0b10101010;
    
    printf("Original: ");
    print_bytes(&byte, 1);
    
    setbit(&byte, 0);
    printf("After set bit 0: ");
    print_bytes(&byte, 1);
    
    flipbit(&byte, 1);
    printf("After flip bit 1: ");
    print_bytes(&byte, 1);
    
    return 0;
}
```

## Project Structure

```
Stdoslib/
├── include/
│   └── stdoslib.h       # Public header file
├── src/
│   └── stdoslib.c      # Implementation
├── Makefile            # Build configuration
└── README.md           # This file
```

## Building from Source

```bash
# Clean build artifacts
make clean

# Build static and shared libraries
make

# Install system-wide
sudo make install

# Uninstall
sudo make uninstall
```

## License

This project is provided as-is for educational and commercial use.

## Contributing

Contributions are welcome! Please ensure:
- Code follows the existing style
- Functions are documented
- Changes don't break existing functionality
- Tests are added for new features

## Notes

- The library uses GCC-specific attributes for visibility control
- Some functions use static buffers (e.g., `concat`) - be aware of reentrancy
- The library is designed for systems programming and may not be suitable for all use cases
- Warnings during compilation are acceptable as noted in requirements
