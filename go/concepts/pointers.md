# Pointers

- Pointers serve the purpose of **sharing values across program boundaries**. There are several types of program boundaries. The most common one is between function calls.
- It is a **variable that stores the memory address of another variable**. This allows you to manipulate the memory directly and create references to variables.
- A pointer is declared using the `*` symbol and the `&` operator is used to obtain the memory address of a variable.
- The `*` operator is also used to deference a pointer, which means accessing the value stored at the memory address the pointer points to.
- Improper use of pointers can still lead to bugs, such as `nil` pointer dereferences, which can cause runtime panics.
- The **zero value** of a pointer is **`nil`**, meaning an uninitialized pointer does not point to any valid memory address.
- **Two Important Operators**
    - **`*`**
        - **Dereferencing operator**.
        - Used to **declare a pointer variable** and **access the value stored at the address**.
    - **`&`**
        - **Address operator**.
        - Used to **return the address of a variable** or to **access the address of a variable through a pointer**.
- **Example**
    ```go
    var x int = 10
    
    // A pointer variable of type *int and initialize it with the memory address
    // of x.
    var ptr *int = &x
    
    fmt.Println(x)
    fmt.Println(ptr)
    
    // Deferencing the pointer to give us the value stored at the memory address
    fmt.Println(*ptr)
    ```

### Important Points
- The default value or zero value of a pointer is always `nil`.
- Declaration and initialisation of the pointers can be done in a single line.
- If you are specifying the data type along with the pointer declaration, then the pointer will be able to handle the memory address of that specified data type variable.
- There is no need to specify the data type during the declaration. The type of a pointer variable can also be determined by the compiler, like a normal variable.
- You can use the shorthand (`:=`) syntax to declare and initialise the pointer variables.

### Pass By Value
| Non-Pointer Values | Pointer Wrapper Values |
|--------------------|------------------------|
| Strings            | Slices                 |
| Integers           | Maps                   |
| Floats             | Functions              |
| Booleans           |                        |
| Arrays             |                        |
| Structs            |                        |

- All data is moved around the program by value. This means that as data is being passed across program boundaries, each function or Goroutine is given its own copy of the data.
- There are two types of data you will work with: the value itself or the value's address. Addresses are data that need to be copied and stored across program boundaries.
- "Value of" → What's in the box.
- "Address of" (&) → Where is the box?
- **Example**
    ```go
    // increment1 declares the function to accept its own copy of
    // and integer value.
    func increment1(inc int) {
    
    	// Increment the local copy of the caller's int value.
    	inc++
    	println("inc1:\\tValue Of[", inc, "]\\tAddr Of[", &inc, "]")
    }
    
    // increment2 declares the function to accept its own copy of
    // an address that points to an integer value.
    // Pointer variables are literal types and are declared using *.
    func increment2(inc *int) {
    
    	// Increment the caller's int value through the pointer.
    	*inc++
    	println(
    		"inc2:\\tValue Of[",
    		inc, "]\\tAddr Of[", &inc,
    		"]\\tPoints To[", *inc, "]")
    }
    
    func main() {
    	// Declare variable of type int with a value of 10.
    	count := 10
    
    	// To get the address of a value, use the & operator.
    	println("count:\\tValue Of[", count, "]\\tAddr Of[", &count, "]")
    
    	// Pass a copy of the "value of" count (what's in the box)
    	// to the increment1 function.
    	increment1(count)
    
    	// Print out the "value of" and "address of" count.
    	// The value of count will not change after the function call.
    	println("count:\\tValue Of[", count, "]\\tAddr Of[", &count, "]")
    
    	// Pass a copy of the "address of" count (where is the box)
    	// to the increment2 function. This is still considered a pass by
    	// value and not a pass by reference because addresses are values.
    	increment2(&count)
    
    	// Print out the "value of" and "address of" count.
    	// The value of count has changed after the function call.
    	println(
    		"count:\\tValue Of[",
    		count, "]\\tAddr Of[", &count, "]")
    	// OUTPUT
    	// count: Value Of[ 10 ] Addr Of[ 0xc000050738 ]
    	// inc1: Value Of[ 11 ] Addr Of[ 0xc000050730 ]
    	// count: Value Of[ 10 ] Addr Of[ 0xc000050738 ]
    	// inc2: Value Of[ 0xc000050738 ] Addr Of[ 0xc000050748 ] Points To[ 11 ]
    	// count: Value Of[ 11 ] Addr Of[ 0xc000050738 ]
    }
    ```

### Reference Type
- A reference type is a pointer — a type of data that refers to, or "points to", the location in memory of a value.
- In Go, all data is passed by value, which means that whenever you pass data to a function, GO creates a copy of that data and assigns the copy to a parameter variable.
- When you assign a reference type to another variable, the new variable references the same memory location.
- Go has several reference types, including pointers, slices, maps, channels, functions, and interfaces.
- **Pointers**
    - It holds the memory address of a value. It allows you to access and modify the memory location of a value directly.
- **Slices**
    - A descriptor of an array segment. It includes a pointer to the array, the length of the segment, and its capacity (the maximum length of the segment).
- **Maps**
    - A powerful data structure that associates values of one type (the key) with values of another type (the value).
    - It's an unordered collection of key-value pairs.
- **Channels**
    - Used for communication between goroutines (Go's term for threads).
    - They allow you to pass data from one goroutine to another.
- **Functions**
    - First-class citizens. This means that they can be assigned to variables, passed as arguments to other functions, and returned as values from other functions.
    - When a function is assigned to a variable, the variable stores a reference to the function.
- **Interfaces**
    - It represents a set of method signatures. It provides a way to specify the behaviour of an object: if something can do this, then it can be used here.

### Mutability
- Mutability plays a critical role when you consider how "pass by value" works in combination with more composite, aggregate, or complex data types like slices and maps, and pointers.
- A mutable value is a value that **can be changed**. In Go, slices, maps, and pointers are mutable data types.
- Even though they are passed by value, they still behave as if they were passed by reference, because **the "value" that is copied and passed is the reference to the underlying data, not the actual data**.
- **Example**
    
    ```go
    func modify(s []int) {
    	s[0] = 100
    }
    
    func main() {
    	nums := []int{1, 2, 3}
    	fmt.Println(nums) // [1 2 3] 
    
    	// The changes made inside the function are visible outside of it, even
    	// though the data was passed by value.
    	modify(nums)
    	fmt.Println(nums) // [100 2 3]
    }
    ```
    
- In the case of pointers, the pointer itself is passed by value, meaning the function receives a copy of the address, but the data it points to remains the same. Therefore, dereferencing the pointer and modifying the value it points to inside the function will modify the original value.

## Pass By Value vs Pass By Reference
- In Go, all function parameters are passed by value by default. This means that a copy of the parameter value is passed to the function.
- However, you can use pointers to pass parameters by reference, which means that the function can modify the original parameter value.

## Memory Management with Pointers
- In Go, memory management is automatic and handled by the garbage collector. However, Go also provides the ability to allocate memory dynamically using pointers.
- Dynamic memory allocation allows you to create data structures that can grow and shrink as needed.
- **Example**
    ```go
    
    // A pointer varibale with a new() function to allocate memory for an integer
    // value.
    var ptr *int = new(int)
    
    fmt.Println(ptr)
    
    // Deferencing to print the value stored at the memory address, which is
    // initially set to 0.
    fmt.Println(*ptr)
    
    // Assigning a value of 10 to the memory location pointed pointed to by ptr
    // using the * operator.
    *ptr = 10
    fmt.Println(*ptr)
    
    // Setting the pointer variable to nil, which frees the memory block allocated
    // by new().
    ptr = nil
    ```

## Pointer and Value Semantics
- **Value Semantics**
    - Refers to when the **actual data** of a variable is passed to a function or assigned to another variable.
    - The new variable or function parameter **gets a completely independent copy of data** — everything in Go is ‘pass by value'.
    - It facilitates higher levels of integrity.
        - "The majority of bugs that we get in software have to do with the mutation of memory" — Bill Kennedy
    - Everybody gets their own copy of data (pass by value) helps keep data bug-free.
        - If everybody is isolated to their own copy of the data (pass by value), then the bug is isolated to that one piece of code.
    - It reduces side effects around concurrency and is more likely to **keep values on the stack**.
    - **The Stack**
        - In Go, when a function receives a value (not a pointer), it gets its own copy of that value. This copy is typically placed on the stack, which is fast and doesn't involve any form of garbage collection. Once the function returns, this memory can be instantly reclaimed.
- **Pointer Semantics**
    - Passing the **memory address** (a "pointer") rather than the data itself.
    - You can modify the original data, not just a copy of it.
    - **The Heap**
        - When a function receives a pointer or returns a pointer to a local variable, it indicates to the compiler that this value could be shared across goroutine boundaries or could persist after the function returns.
        - To ensure that the data will remain available, the Go compiler must allow it on the heap, rather than on the stack.
        - **Heap allocation is more expensive and requires garbage collection**.
- The choice between pointer and value semantics in Go depends on the needs of your application.
    - **Value Semantics** are **simpler and often safer** because they avoid side effects, but they can be **inefficient for large data** structures because they involve copying data.
    - **Pointer Semantics** can be **more efficient and flexible**, but they also **require careful management** to avoid bugs related to shared, **mutable state**.
- "The stack is for data that needs to persist only for the lifetime of the function that constructs it, and is reclaimed without any cost when the function exits. The heap is for data that needs to persist after the function that constructs it exits, and is reclaimed by a sometimes costly garbage collection." — Ayan George

## Pointer and Value Semantics Heuristics
- **Use Value Semantics when possible.**
    - Value semantics are **simpler and usually safer**, since they don't involve shared state or require you to think about memory management.
    - **Rule of Thumb**:
        - If a function **doesn't need to modify its input**, or the data you're working with is small (like built-in types or small structs), use value semantics.
        - Use value semantics for built-in types (numeric, string, and bool) and reference types (slices, maps, channels, pointers, interfaces, functions).
- **Use Pointer Semantics:**
    - **For large data**
        - Copying **large structs or arrays** can be inefficient.
        - If the data you're working with is large, you might want to use pointer semantics to avoid the cost of copying the data.
        - **Rule of Thumb**: 64 bytes or larger, use pointers.
    - **For mutability**
        - If a function or method **needs to modify its receiver or an input parameter**, you'll need to use pointer semantics.
        - This is a common use case for methods that need to update the state of a struct.
    - **When interfacing with other code**
        - If you're interfacing with other code (e.g., library or system call), you might need to use pointer semantics.
        - For example, the `json.Unmarshal` function in the Go standard library requires a pointer to a value to populate it with unmarshalled data.
- **Consistency**
    - If some functions on a type use pointer semantics and others use value semantics, it can lead to confusion.
    - Typically, once a type has a method with pointer semantics, all methods on that type should have pointer semantics.

## Stack vs Heap Allocation

|**Aspect**|**Stack Allocation**|**Heap Allocation**|
|---|---|---|
|**Allocation Criteria**|If the variable doesn't escape the scope of the function where it is defined (i.e., it is not returned or referenced outside), it can be safely allocated on the stack.|If the variable escapes (for example, by being returned from the function or captured by a goroutine), it must be allocated on the heap to ensure that it remains accessible after the function returns.|
|**Performance**|Faster due to simple pointer manipulation.|Slower due to dynamic memory allocation.|
|**Garbage Collection Pressure**|None. Memory is automatically reclaimed when function returns.|May cause overhead due to garbage collection.|
|**Memory Efficiency**|Generally more efficient due to cache locality and automatic deallocation.|May be less efficient due to fragmentation and overhead.|

## Escape Analysis
- Go's escape analysis decides whether a variable should be allocated on the stack or heap.
- It examines the function to see if the pointer to a variable is being returned or if the variable is captured within a function literal (closure). If so, the variable escapes to the heap. In other cases, even with pointer semantics, if the variable does not escape, it may be kept on the stack.
- Understanding escape analysis is about understanding value ownership. When a value is constructed within the scope of a function, then that function owns the value.
    - Does the value being constructed still have to exist when the owning function returns?
        - If not, the value can be constructed on the stack.
        - If yes, the value must be constructed on the heap.
- **Example**
    ```go
    // user represents a user in the system.
    type user struct {
        name  string
        email string
    }
    
    // stayOnStack is using **value semantics** to return a user value back to the
    // caller. The caller gets their own copy of the user value being constructed.
    // The user value it constructs no longer needs to exist, since the caller is
    // getting their own copy. Therefore, the construction of the user value
    // inside of stayOnStack can happen on the stack.
    func stayOnStack() user {
        u := user{
            name:  "Bill",
            email: "bill@email.com",
        }
    
        return u
    }
    
    // escapeToHeap is using **pointer semantics** to return a user value back to the
    // caller. The caller gets shared access (an address) to the user value being
    // constructed. The user value it constructs does still need to exist, since
    // the caller is getting shared access to the value. Therefore, the
    // construction of the user value inside of escapeToHeap can't happen on the
    // stack, it must happen on the heap. 
    func escapeToHeap() *user {
        u := user{
            name:  "Bill",
            email: "bill@email.com",
        }
    
        return &u
    }
    ```
    
- The **stack is self-cleaning** since a frame is taken and initialised for the execution of each function call. The **stack is cleaned during function calls** and not on returns because the compiler doesn't know if that memory on the stack will ever be needed again.
- Escape analysis decides if a value is constructed on the stack (the default) or the heap (the escape).
- **How to check Escape Analysis?**
    - Go provides a `-gcflags` flag that allows you to pass flags to the Go compiler.
        ```bash
        # You can use the -m flag to print escape analysis information for your
        # code or "-m=2" for more detailed output.
        $ go build -gcflags "-m" main.go
        ```

## Inlining Decisions
- Inlining is the act of combining smaller functions into their respective callers.
- Nowadays, inlining is one of a class of fundamental optimisations performed automatically during the compilation process.
- **Why is it important?**
    - It eliminates the overhead of the function call itself (i.e., the time it takes to jump to the function code, set up the function's local variables, and then return when the function is complete).
    - It enables the compiler to apply other optimisation strategies more effectively.
- Inlining too much code can lead to a larger binary, and possibly due to cache effects, slower code.
- When you run the Go compiler with the ‘`gcflags "-m"`', it will print information about its inlining decisions along with escape analysis information. You'll see lines like ‘`inlining call to X`' where `X` is a function that the compiler decided to inline.
- **Remember**
    - The decision of using pointer or value semantics should be more about sharing and mutating state rather than memory allocation and performance.

## Method Sets
- A type may have a method set associated with it. The method set of an interface type is its interface.
- The method set of any other named **`type T`** consists of all methods with the **receiver type `T`**.
    - These methods can be called using variables of **`type T`**.
- The method set of the corresponding **pointer type `*T`** is the set of all methods with the **receiver `*T` or `T`** (that is, it also contains the method set of **`T`**).
    - These methods can be called using variables of the **`type *T`**.
    - It can call methods of the corresponding non-pointer type as well.
- **Key Point**: Integral to how interfaces are implemented and used in Go.
    - An interface in Go defines a method set, and any type whose method set is a superset of the interface's method set is considered to implement that interface.
- **Remember**
    - If you **define a method with a pointer**, the **method is only in the method set of the pointer type**.
    - If an **interface requires a method that's defined on the pointer** (not the value), then you can **only use a pointer to that type** to satisfy the interface, not a value of the type.
- **Example**
    ```go
    type List []int
    
    func (l List) Len() int        { return len(l) }
    func (l *List) Append(val int) { *l = append(*l, val) }
    
    type Appender interface {
        Append(int)
    }
    
    func CountInto(a Appender, start, end int) {
        for i := start; i <= end; i++ {
            a.Append(i)
        }
    }
    
    type Lener interface {
        Len() int
    }
    
    func LongEnough(l Lener) bool {
        return l.Len()*10 > 42
    }
    
    func main() {
        // A bare value
        var lst List
        
        // INVALID: Append has a pointer receiver
        CountInto(lst, 1, 10)
        
        // VALID: Identical receiver type
        if LongEnough(lst) {
            fmt.Printf(" - lst is long enough")
        }
    
        // A pointer value
        plst := new(List)
        
        // VALID: Identical receiver type
        CountInto(plst, 1, 10)
    
        // VALID: a *List can be dereferenced for the receiver
        if LongEnough(plst) {
            fmt.Printf(" - plst is long enough")
        }
    }
    ```

## 🔖 Extras
- [Using Pointers In Go](https://www.ardanlabs.com/blog/2014/12/using-pointers-in-go.html)
- [Ultimate Go Tour — Pointers](https://tour.ardanlabs.com/tour/eng/pointers/1)
- [A Comprehensive Guide to Pointers in Go](https://medium.com/@jamal.kaksouri/a-comprehensive-guide-to-pointers-in-go-4acc58eb1f4d)
- [Source file escape.go](https://tip.golang.org/src/cmd/compile/internal/escape/escape.go)
- [Stack or Heap? Going Deeper with Escape Analysis in Go for Better Performance](https://syntactic-sugar.dev/blog/nested-route/go-escape-analysis)
- [Inlining optimisations in Go](https://dave.cheney.net/2020/04/25/inlining-optimisations-in-go)
- [Understanding Go Inline Optimization by Example](https://dave.cheney.net/2020/04/25/inlining-optimisations-in-go)
- [Go's hidden #pragmas](https://dave.cheney.net/2018/01/08/gos-hidden-pragmas)
- [Go Wiki: MethodSets](https://go.dev/wiki/MethodSets)

## 📹 Videos
- [Go (Golang) Tutorial #13 - Pass By Value](https://www.youtube.com/watch?v=LBLN4Wu5U4w)
- [Go (Golang) Tutorial #14 - Pointers](https://www.youtube.com/watch?v=4B2rwYvuiBo)
- [Go (Golang) Tutorial #16 - Receiver Functions](https://www.youtube.com/watch?v=HE6tbWlymmk&list=PL8LbIS8AqHOYbbC7wAk1sHdq9Zk21eQpa&index=16)
- [Go (Golang) Tutorial #17 - Receiver Functions with Pointers](https://www.youtube.com/watch?v=cgBA5k50At8&list=PL8LbIS8AqHOYbbC7wAk1sHdq9Zk21eQpa&index=17)
- [Pointers in golang](https://www.youtube.com/watch?v=-BFJ0dZyxHg)
- [Memory management in golang](https://www.youtube.com/watch?v=G1SP9uDJD0g)
- [Advanced Golang: Pointers](https://www.youtube.com/watch?v=DVNOP1LE3Mg&t=419s)
- [Go Pointers: When & How To Use Them Efficiently](https://www.youtube.com/watch?v=3WsEDZRif6U)