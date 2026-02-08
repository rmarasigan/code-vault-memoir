---
Tags:
  - Notes
  - Basics
Created: 23-November-2025 16:28:36
Modified: 08-February-2026 10:20:30
---
## Conversion
- A conversion **changes the type of an expression** to the type specified by the conversion
- It may appear literally in the source, or it may be implied by the context in which an expression appears
- An explicit conversion is an expression of the form `T(x)` where `T` is a _type_ and `x` is an expression that can be converted to type `T`

## Expressions
- Specifies the computation of a value by applying operators and functions to operands
- **Expression vs Statement**
    - An expression represents a value (returns a value)
    - A statement represents an operation, an imperative command to _do something_, but itself does not have a value

## Constants
- A constant is just a simple, **unchanging value**
- They are created at compile time, even when defined as locals in functions, and can only be numbers, characters (runes), strings, or booleans
- It may be given a type explicitly by a constant declaration or conversion, or implicitly when used in a variable or an assignment statement or as an operand in an expression
- Go is statically typed language that does not permit operations that mix numeric types
    - If you want to add `int` and `uint`, you must be explicit about what you want the result to be
        ```go
        var u uint
        var i int
        
        // You can write either:
        uint(i)+u
        i+int(u)
        ```
    - This strictness eliminates a common cause of bugs and other failures, but it has a cost: it sometimes requires programmers to decorate their code with clumsy numeric conversions to express their meaning clearly
- **Untyped Constant**
    - Has a default type which is the type to which the constant is implicitly converted in contexts where a typed value is required
    - The **default type** of an untyped constant is `bool`, `rune`, `int`, `float64`, `complex128`, or `string` respectively, depending on whether it is a boolean, rune, integer, floating-point, complex, or string constant

### String
```go
// **Untyped** String Constant
const hello = "Hello, 世界"

// **Typed** String Constant
const greeting string = "Hello, 世界"
```
- Encloses some text between double quotes (Go also has raw string literals, enclosed by back-quotes, 

> [!TIP]
> A `string` is represented in memory as a **two-word** structure containing:
> - A pointer to the string
> - The length
> 
> A `string` is immutable and multiple strings can share the same storage. It should be noted, that slicing a string does not allocate or copy the underlying data.
> - Slicing creates a new two-word structure that points to the same byte sequence, and corresponding length.
> - This also means the entire original underlying data is kept even if the slice only takes a subset.
> 
> The "_word_" is a technical term for 8 bytes (on 32-bit hardware for 4 bytes). The actual hardware works (often) with word-sized things like integers or pointers. So the "two-words" means 16 bytes. The first 8 is for the length, and the second 8 is for the address of the actual string content.

### Booleans
- The values `true` and `false` are untyped boolean constants that can be assigned to any boolean variable, but once given a type, boolean variables cannot be mixed

### Floats
- There are **two floating-point** types in Go: `float32` and `float64`
- The default type for a floating-point constant is `float64`, although an untyped floating-point constant can be assigned to a `float32` value just fine

### Complex Numbers
- Complex constants behave a lot like floating-point constants
- The default type of a complex number is `complex128`, the larger-precision version composed of two `float64` values

### Integers
- This have more moving parts — many sizes, signed or unsigned, and more — but they play by the same rules
- Some of the integer types are:
    - `byte` — alias for `uint8`
    - `rune` — alias for `int32`
    - **Signed Integers**
        - `int8` — 8-bit, ranging from -128 to 127 (constant values outside of that range can never be assigned)
        - `int16` — 16-bit, ranging from -32, 768 to 32, 767
        - `int32` — 32-bit, ranging from -2, 147, 483, 648 to 2, 147, 483, 647
        - `int64` — 64-bit, ranging from -9, 223, 372, 036, 854, 775, 808 to 9, 223, 372, 036, 854, 775, 807
    - **Unsigned Integers**
        - `uint8` — also known as `byte`, 8-bit, ranging from 0 through 255
        - `uint16` — 16-bit, ranging from 0 to 65, 535
        - `uint32` — 32-bit, ranging from 0 to 4, 294, 967, 295
        - `uint64` — 64-bit, ranging from 0 to 18, 446, 744, 073, 709, 551, 615
        - `uintptr` — large enough to contain a bit pattern of any pointer

### A String and A []Byte
- `string`
    - It represents a sequence of characters. It is an immutable type, which means you cannot modify individual characters within a string.
    - String values are always interpreted as UTF-8 encoded Unicode text.
- `[]byte`
    - A slice of bytes, where each element represents a single byte. It is a mutable type, so you can modify individual bytes within a byte slice.
    - It can be used to represent binary data or text in various encoding.
- `string` to `[]byte`
    - You can convert a string to a byte slice using the `[]byte()` type conversion.
    - **Example**
        ```go
        str := "Hello"
        bytes := []byte(str)
        ```
- `[]byte` to `string`
    - You can convert a byte slice to a string using the `string()` type conversion.
    - **Example**
        ```go
        bytes := []byte{72, 101, 108, 108, 111}
        str := string(bytes)
        ```
- **Byte Buffer**
    - A **region of memory** used to **temporarily store a sequence of bytes**. It provides a data structure for efficient manipulation of byte data.
    - Allows you to read and write bytes to and from the buffer, making it useful for tasks like data serialization, network communication, file I/O, and efficient string manipulation.
    - The purpose of a byte buffer is to **provide a flexible and efficient way to work with sequences of bytes**. It typically offers methods or functions for operations such as appending data, reading data, resizing the buffer, and converting the buffer’s contents to different types.
    - In Go, `bytes.Buffer` is a type that provides a way to manipulate in-memory buffers. It is part of the standard library packages called `bytes`. You can perform various operations on it. Some common operations include:
        - `Write` or `WriteString`: Appending data to the buffer
        - `Read` or `ReadString`: Reading data from the buffer
        - `Bytes`: Getting the contents of the buffer as a byte slice
        - `String`: Getting the contents of the buffer as a string
        - `Reset`: Resetting the buffer to its initial state

## Built-in Types
- **Built-in String Type**
    - `string`
- **Built-in Boolean Type**
    - `bool`
- **Built-in Numeric Types**
    - `int8`
    - `int16`
    - `int32` (`rune`)
    - `int64`
    - `uint8` (`byte`)
    - `uint16`
    - `uint32`
    - `uint64`
    - `uintptr`
    - `float32`
    - `float64`
    - `complex64`
    - `complex128`

## Aggregate Types
- Types that **group multiple values together** in a fixed, structured, and contiguous way in memory
- **Struct Types**
    - Consist of a **collection of named fields** (member variables), each with its own type
        ```go
        type Book struct {
        	author string
        	title  string
        	pages  int
        }
        ```
    - The act of combining multiple fields into a `struct` is known as **composition**
    - Useful for **grouping values of different types** into a single logical unit
    - A **field is exported** (i.e., accessible from other packages) if its **name starts with a capital letter**
- **Container Types**
    - **Array**
        - A **fixed-length sequence** of zero or more elements of a particular type (e.g. `[N]T`)
        - Rarely used directly due to fixed length — **slices are preferred** for most use cases
        - Elements are **automatically initialized** to the **zero value** of their type
        - If an ellipsis `...` is used for the length, the length is inferred from the number of initial values
            ```go
            array := [...]int{1, 2, 3}
            fmt.Printf("%T\\n", array)
            ```

> [!NOTE]
> The size must be a constant expression, whose value can be computed as the program is being compiled.

## Composite Types
- Types that are **constructed from**, **reference**, or **wrap other types**
- These types often offer **more flexibility and dynamic behavior** (data structures that can grow or change in size)
- **Pointer Types** (`*T`):
    - Point to a value of another type `T`
    - The `T` is called the **base type** of the pointer
- **Function Types**
    - The signature of a function type is composed of the input parameter definition list and the output result definition list of the function (e.g. `func (int) (bool, string)`)
- **Container Types**
    - **Slice Types**
        - It looks like an array type without a size
        - **Dynamic-length and dynamic-capacity** container types (e.g. `[]T`)
        - Represent **variable-length sequences** whose elements all have the same type
        - Internally consist of three components
            - **Pointer**: **Points to the first element of the array** that is reachable through the slice, which is not necessarily the array’s first element
            - **Length**: The number of slice elements, and **can’t exceed the capacity**
            - **Capacity**: The **number of elements** between the start of the slice and the end of the underlying array
    - **Map Types**
        - Maps are associated arrays (or dictionaries)
            ```go
            // K is the type of its keys
            // V is the type of its values
            map[K]V
            ```
        - An **unordered collection of key/value pairs** in which all the **keys are distinct** (also known as a hash table or dictionary)
        - The value associated with a given key can be retrieved, updated, or removed using a constant number of key comparisons on the average

> [!NOTE]
> - The keys need not be of the same type as the values.
> - All of the keys in a given map are of the same type, and all of the values are of the same type.

- **Channel Types**
    - Used for **communication and synchronization** between goroutines (e.g. `chan T`)
    - It can be viewed as **synchronized First-In-First-Out (FIFO) queues**
    - **Three Channel Types**
        - **Bidirectional (default)**: Both sendable and receivable
        - **Send-Only**: Only sendable and are denoted by the `chan<- T` literal
        - **Receive-Only**: Only receivable and are denoted by the `<-chan T` literal
- **Interface Types**
	```go
    interface {
    	Method0(string) int
    	Method1() (int, bool)
    }
    ```
    - Define a set of method signatures
    - Allow for **polymorphism** — any type that implements the interface's methods satisfies the interface
    - Used heavily in **reflection**, **abstraction**, and **dependency injection**

## 🔖 Extras
- [Conversions](https://go.dev/ref/spec#Conversions)
- [Expressions](https://go.dev/ref/spec#Expressions)
- [Constants — Rob Pike](https://go.dev/blog/constants)
- [Go Type System Overview](https://go101.org/article/type-system-overview.html)
- [Chapter 4. Composite Types](https://notes.shichao.io/gopl/ch4/)
- [Expressions, Statements and Simple Statements](https://go101.org/article/expressions-and-statements.html)
- [Statements are statements, and expressions are expressions (in Go)](https://clipperhouse.com/statements-are-statements-and-expressions-are-expressions-in-go/)
- [What's the difference among Expression,Statements and Declaration from the view of compiler?](https://stackoverflow.com/questions/26910493/whats-the-difference-among-expression-statements-and-declaration-from-the-view)