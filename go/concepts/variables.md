# Variables

- Provide the ability to read from and write to memory
- Access to memory is type-safe, this means that the compiler takes type seriously and won't allow us to use variables outside the scope of how they are declared
- Use the keyword `var` when variables are being declared to their zero value
    ```go
    var a int
    var b string
    var c float64
    var d bool
    ```
    
- Use the **short variable declaration** when variables are being declared and initialized
    ```go
    /* **Syntax**
     *  **Declaring Variable with an Initial Value**
     *  var varName dataType = initialValue
     *
     *  **Type Inference Syntax**
     *  It will automatically infer the type of the variable using the given value
     *  var varName = initialValue
     *
     *  **Short Hand Declaration Syntax**
     *  Do not use the short hand declaration outside a function
     *  It is only for local variables
     *  varName := "initialValue"
    */
    quantity := 10
    pie := 3.14159
    isActive := true
    greeting := "hello"
    ```
## Scope
### Local Variable
- Defined **within a block or a function level** and can only be accessed from within their block or function
- Once it is declared, it cannot be re-declared within the same block or function
- It will only live till the end of the block or a function in which they are declared, and after that, they are Garbage Collected

```go
func main() {
	var message = "Hello, World!"
	fmt.Println(message)
}

func local() {
	// The build will fail and return an error
	fmt.Println(message) // ./prog.go:11:14: undefined: message
}
```

### Global Variable (Package-Level)
- It will be global within a package if it is **declared at the top of a file** outside the scope of any function or block
- If the name starts with a **lowercase letter**, it can be accessed from within the same package
- If the name starts with an **uppercase letter**, it can be accessed outside of the package
- If you assign a new value to a global variable inside a function, it will be modified the global variable itself

```go
package main

import "fmt"

// Global variable with lowercase (**package-private**)
var message = "a global variable"

// Global variable with uppercase (**exported/public**)
var Version = "1.0"

func main() {
	fmt.Println(message) // a global variable
	
	// Declaring a new local variable that shadows the global one
	message := "a local variable"
	fmt.Println(message) // a local variable
}
```

**For more info: [Understanding Scope](understanding-scope.md)**
## Built-In Types
- It can be specific to a precision such as `int32` or `int64`
    - `uint8` = unsigned integer with 1 byte of allocation
    - `int32` = signed integer with 4 bytes of allocation

> [!TIP] 
> 1 Byte = 8 Bits
    
- If using a non-precision-based type (`uint`, `int`), the size of the value is based on the architecture used to build the program
    - 32-bit arch = int represents a signed int at 4 bytes of memory allocation
    - 64-bit arch = int represents a signed int at 8 bytes of memory allocation

## Word Size
- The **amount of memory allocation** required to store integers and pointers for a given architecture
- The size of the data structures (maps, channels, slices, interfaces, functions) will be based on the architecture being used to build the program

## Zero Value Concept
- Zero value is the setting of every bit in every byte to zero.

    | Type    | Zero Value |
    |---------|------------|
    | bool    | false      |
    | int     | 0          |
    | float   | 0          |
    | complex | 0i         |
    | string  | "" (empty) |
    | Pointer | nil        |

- `string` uses the UTF-8 character set and is a two-word internal data structure
    - First word = pointer to a backing array of bytes
    - Second word = length or the number of bytes in the backing array
- If the `string` is set to its zero value state, then the first word is `nil` and the second word is `0`

## Conversion vs Casting vs Assertion
- There is **no type casting** in Go
- **Type Conversion**
    - **Syntax**: `type(variable)`
    - You explicitly convert **one data type into another**, and it works between concrete (non-interface) types
    - Converting a value from one type to another **creates a new copy of the value** whilst the original value remains unchanged
    - It is checked at **compile time**, and if a conversion is invalid, the compiler will catch it before the program runs
    ```go
    var x uint16 = 100     // Remains unchaged
    var y int = int(x)     // Creates a new int copy of x
    
    greeting := "hello"         // Remains unchanged
    slice := []byte(greeting)   // Creates a copy as a byte slice
    slice[0] = 'H'              // Modifies 'slice', but not greeting
    ```
    
- **Type Assertion**
    - **Syntax**: `variable.(type)`
    - Used when an interface holds a value of a concrete type, and you want to get back that original type
    - It extracts the original value and **does not create a new copy**—just a new reference to the same underlying value
    - Type assertion happens at **runtime** because interface values store dynamic types
    - If an assertion is wrong, the program panics while running
    ```go
    // Unsafe Type Assertion (runtime panic)
    var x interface{} = 25
    hello := x.(string)   // Runtime panic: interface conversion: int is not string
    
    // Preventing panics using safe Type Assertion
    var y interface{} = "hello"
    greeting, ok := y.(string)
    if ok {
    	fmt.Println("value is: ", greeting)
    } else {
    	fmt.Println("type assertion failed")
    }
    
    // Using Type Switch
    var z interface{} = 42.56
    switch val := z.(type) {
    	case string:
    		fmt.Println("type string")
    	case int:
    		fmt.Println("type int")
    	case float32:
    		fmt.Println("type float32")
    	default:
    		fmt.Println("type unknown")
    }
    ```

> [!TIP]
>  **Rule of Thumb**: If you have to wrap your concrete type into an interface and want your concrete type back, use a Type Assertion (or Type Switch). If you need to convert one concrete type to another, use a Type Conversion.

## Naming Convention
- Can only start with a letter or an underscore
- You should use `MixedCase`, don't use `names_with_underscores`
- Acronyms should be all capitals (e.g. `ServeHTTP`)

### Local Variables
- Keep them short; long names obscure what the code does
    - Obscure here means that the main logic gets hidden behind a long name, making the code harder to scan and understand quickly
- Avoid redundant names, given their context
```go
// Prefer 'b' to bueffer
func RuneCount(b []byte) int {
    // Prefer 'count' to runeCount
    count := 0
    
    // Prefer 'i' to index
    for i := 0; i < len(b); {
        if b[i] < RuneSelf {
            i++
        } else {
            _, n := DecodeRune(b[i:])
            i += n
        }
        
        count++
    }
    
    return count
}
```

### Parameters
- Where the types are descriptive, they should be short
    ```go
    func AfterFunc(d Duration, f func()) *Timer {...}
    func Escape(w io.Writer, s []byte) {...}
    ```
- Where the types are more ambiguous, the names may provide documentation
    ```go
    func Unix(sec, nsec int64) Time {...}
    func HasPrefix(s, prefix []byte) bool {...}
    ```

### Return Values
- Should only be named for documentation purposes
```go
func Copy(dst Writer, src Reader) (written int64, err error) {...}
```

### Receivers
- By convention, they are one or two characters that reflect the receiver type, because they typically appear on almost every line
- Should be consistent across a type's methods (don't use `r` in one method and `rdr` in another)

```go
func (b *Buffer) Read(p []byte) (n int, err error) {...}
func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request) {...}
func (r Rectangle) Size() Point {...}
```

### Interface Types
- Interfaces that specify just one method are usually just that function name with ‘er' appended to it
    ```go
    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    
    type ByteReader interface {
        ReadByte() (c byte, err error)
    }
    ```
- If it includes multiple methods, choose a name that accurately describes its purpose (e.g. `net.Conn`, `http.ResponseWriter`, `io.ReadWriter`)

### Errors
- Error types should be of the form `FoorError`
    - If you are defining a custom error type (`struct`), its name should end with `Error`
    ```go
    type ExitError struct {
        ...
    }
    ```
- Error-values should be in the form of `ErrFoo`
    - If you are creating an error value (a specific instance of an error), its name should start with `Err`
    ```go
    var ErrFormat = errors.New("image: unknown format")
    ```

## Expressions
- Specifies the computation of a value by applying operators and functions to operands, and always returns a result
- Some expressions may be used as statements, but not all statements can be used as expressions
- **Examples**
    - **Literal Expressions**
        ```go
        5          // Integer literal
        "hello"    // String literal
        3.14       // Floating-point literal
        ```
    - **Arithmetic Expressions**
        ```go
        x + y        // Sum of x and y
        a * (b - c) // Multiplication and subtraction
        ```
    - **Comparison Expressions (`true` or `false`)**
        ```go
        x > y      // Greater than
        a == b     // Equality check
        ```
    - **Logical Expressions**
        ```go
        isReady && hasPermission  // Logical AND
        isOn || isOff            // Logical OR
        ```
    - **Function Call as an Expression**
        ```go
        math.Sqrt(16)   // Evaluates to 4.0
        len("Golang")   // Evaluates to 6
        ```
    - **Type Conversion Expressions**
        ```go
        float64(10)    // Converts integer 10 to float64
        string(65)     // Converts ASCII code 65 to "A"
        ```

## Statements
- It performs an action but does not return a value
- Control executions, do not return results and are executed solely for their side effects
- The increment (`x++`) and decrement (`x--`) operators are statements and not an expression
    - Why can't it be used as expressions?
        - They don't produce a value and they simply modify `x`
        - Go treats them as statements that change a variable in place
            ```go
            // ❌ Compilation error
            // syntax error: unexpected ++, expecting expression
            y := x++
            ```

## 🔖 Extras
- [What's in a name?](https://go.dev/talks/2014/names.slide#1)
- [Type assertions vs. type conversions](https://blog.logrocket.com/type-assertions-vs-type-conversions-go/)
- [Expressions, Statements and Simple Statements](https://go101.org/article/expressions-and-statements.html)
- [What is the difference between type cast and type assertion?](https://www.reddit.com/r/golang/comments/y3tmm4/what_is_the_difference_between_type_cast_and_type/?rdt=56919)
- [What is the difference between type conversion and type assertion?](https://stackoverflow.com/questions/20494229/what-is-the-difference-between-type-conversion-and-type-assertion)