# Arrays
- **Collection of elements** that belong to the same type
- Allows us to **allocate contiguous blocks** of **fixed-size memory**
- Fixed size and store elements of the same type in contiguous memory locations
    - This means that we can access each element quickly since their addresses are calculated based on the starting address of the array and the element’s index
- **Are value types and not reference types**
    - This means that when they are assigned to a new variable, a copy of the original array is assigned to the new variable
    - If changes are made to the new variable, it will not be reflected in the original array
    - Similarly, when arrays are passed to functions as parameters, they are passed by value and the original is unchanged
- They do not need to be initialized explicitly; the zero value of an array is a ready-to-use array whose elements are themselves zeroed

## Syntax and Declaration
- **Syntax**
    ```go
    var array [n]T
    ```
    - Where:
        - `n`: Number of elements in an array
        - `T`: The type of each element
- **Declaration**
    - **All elements in an array are automatically assigned the zero value of the array type**
        ```go
        var array [3]int
        fmt.Println(array)
        // Output: [0 0 0]
        
        // Assigning values
        array[0] = 23
        array[1] = 30
        array[2] = 6
        ```
        - `array` is an integer array and hence all elements of `array` are assigned to `0`, the zero value of `int`
        - The index of an array starts from `0` and ends at `length - 1`
    - **Short hand declaration**
        ```go
        array := [3]int{12, 8, 3}
        fmt.Println(array)
        // Output: [12, 8, 3]
        ```
        - It is not necessary that all elements in an array have to be assigned a value during short hand declaration
    - **Ellipsis `...`**
        ```go
        // Makes the compiler determine the length
        array := [...]int{12, 8, 3}
        ```
        - You can even ignore the length of the array in the declaration and replace it with `...` and let the compiler find the length for you
    - Initialize Only Specific Elements
        ```go
        array := [5]int{1:10,2:40}
        fmt.Println(array)
        ```
        - The array above has 5 elements:
            - `1:10` means: assign `10` to array index `1` (second element)
            - `2:40` means: assign `40` to array index `2` (third element)
    - **The size of the array is part of the type**
        ```go
        // Declare an array of 5 integers that is initialized to its zero value
        var five [5]int
        
        // Declare an array of 4 integers that is initialized with some values
        four := [4]int{10, 20, 30, 40}
        
        // It is not possible since [5]int and [4]int are distinct types
        // and this will throw an error
        five = four  
        ```
        - The **size must be a constant expression**, whose value can be computed as the program is being compiled
        - In Go, the size of an array has to be know at compile time

## Access Elements
- You can access a specific array element by referring to the **index number**
- The array **indexes start at 0**, that means that `[0]` is the first element, `[1]` is the second element, etc.

## Length of an Array
- Found by passing the array as parameter to the `len` function
    ```go
    array := [...]float64{67.7, 89.8, 21, 78}
    fmt.Println("length of array is", len(array))
    ```

## Iterating Over Collection
- **Value Semantic Iteration**
    ```go
    fruits := [5]string{"Apple", "Orange", "Banana", "Grape", "Plum"}
    
    // The loop iterates over each string the collection and displays the index
    // position and the string value. The for range is iterating over its own
    // shallow copy of the array and each iteration the fruit variable is a copy
    // of each string (the two-word data structure).
    for i, fruit := range fruits {
        fmt.Println(i, fruit)
    }
    ```
    - The collection that we’re iterating over is copied and we iterate over the copy
- **Pointer Semantic Iteration**
    ```go
    fruits := [5]string{"Apple", "Orange", "Banana", "Grape", "Plum"}
    
    // The loop iterates over each string the collection and displays the index
    // position and the string value. The for range is iterating over the fruits
    // array directly and on each iteration, the string value for each index
    // position is accessed directly for the fmt.Println call.
    for i := range fruits {
        fmt.Println(i, fruits[i])
    }
    ```
    - We iterate over the original collection and access each element associated with the collection directly

## Comparison of Arrays
- If element type if comparable, then the array type is also comparable
- We can directly compare two arrays of that type using the `==` operator, which reports whether all corresponding elements are equal
    ```go
    a := [2]int{1, 2}
    b := [...]int{1, 2}
    c := [2]int{1, 3}
    fmt.Println(a == b, a == c, b == c)
    // Output: true false false
    ```
- We can also use the `!=` operator as the negation
    ```go
    a := [2]int{1, 2}
    d := [3]int{1, 2}
    fmt.Println(a == d)
    // Error: invalid operation: a == d (mismatched types [2]int and [3]int)
    ```
    
## 🔖 Extras
- [Arrays](https://notes.shichao.io/gopl/ch4/#arrays)
- [Arrays and Slices](https://golangbot.com/arrays-and-slices/)
- [Ultimate Go Tour — Arrays](https://tour.ardanlabs.com/tour/eng/arrays/1)
- [Understanding Arrays and Slices in Go](https://www.digitalocean.com/community/tutorials/understanding-arrays-and-slices-in-go)
- [Go Slices: usage and internals — Arrays](https://go.dev/blog/slices-intro#arrays)
- [How Go Arrays Work and Get Tricky with For-Range](https://victoriametrics.com/blog/go-array/)