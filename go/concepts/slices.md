# Slices
- Are similar to arrays, but are more powerful and flexible
- Also used to store multiple values of the same type in a single variable
- Unlike arrays, the **length of a slice can grow and shrink** as you see fit
- They form the basis for how we manage and manipulate data in a flexible, performant, and dynamic way

## Components
- **Pointer**
    - Used to points to the first element of the array that is accessible through the slice
- **Length**
    - The **total number of elements** present in the array
    - It represents the number of elements that can be read and written to
- **Capacity**
    - It represents the **maximum size up to which it can expand**
    - It represents the total number of elements that exist in the backing array from that pointer position

## Syntax and Declaration
- **Syntax**
    
    ```go
    // Slice of T set to its zero value state
    var slice []T
    ```
    - Where:
        - `T`: The type of each element
- **Declaration**
    - **Short hand**
        ```go
        // Declares a slice of integers of length 3 and also the capacity of 3
        slice := []int{1, 2, 3}
        fmt.Println(slice)
        // Output: [1 2 3]
        ```
    - **Using `make()` Function**
        ```go
        // Syntax
        slice := make([]T, length, capacity)
        
        // A slice of string set with a length and capacity of 5
        slice := make([]string, 5)
        
        // A slice of string set with a length of 5 and a capacity of 8
        slice := make([]string, 5, 8)
        ```
        - Where:
            - `T`: The type of the element of the slice
            - `length`: The initial length of a slice
            - `capacity` (_optional_): The initial capacity of the underlying array and if this is omitted, it defaults to the length
        - This is used to create an empty slice, maps and channels
        - It assigns an underlying array with a size that is equal to the given capacity and returns a slice which refers to the underlying array
    - **Slice from an Array**
        - You can create a slice by slicing an array by specifying a lower bound (`start`) and a upper bound (`end`)
            ```go
            var array = [length]T{values}
            
            // A slice made from the array
            slice := array[start:end]
            ```
        - This **selects a half-open range** which includes the first element, but excludes the last
        - **Example**
            ```go
            array := [6]int{10, 11, 12, 13, 14,15}
            
            slice := array[2:4]
            fmt.Printf("slice: %v length: %d capacity: %d", slice, len(slice), cap(slice))
            // Output: slice [12 13] length: 2 capacity: 4
            ```

            The slice starts at the third element of the array (index 2), which has a value of `12`, and ends before index 4. It includes elements at index 2 and 3 (values `12` and `13`).
            The slice's length is 2 (from index 2 to 4, exclusive), and its capacity is 4 because it can grow from index 2 up to the end of the array at index 6.
	- **Slicing a Slice**
	```go
	slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	
	// [lower:upper]
	fmt.Println(slice[0:4]) // [0 1 2 3]
	
	// [:upper]
	fmt.Println(slice[:7])  // [0 1 2 3 4 5 6]
	
	// [lower:]
	fmt.Println(slice[4:])  // [4 5 6 7 8 9]
	
	// [:]
	fmt.Println(slice[:])  // [0 1 2 3 4 5 6 7 8 9]
	```

> [!NOTE]
>- The **upper bound is exclusive**, so the slice will not include the element at that index.
>- If the **lower bound** is omitted, it defaults to `0`.
>- If the **upper bound** is omitted, it defaults to the length of the array.

## Length vs Capacity
- The **zero value** of a slice is `nil`
    - The `len` and `cap` functions will both return `0` for a slice
- The **length** (`len`) of a slice is the **number of elements it contains**
- The **capacity** (`cap`) of a slice is the **number of elements in the underlying array**, counting from the first element in the slice

## Access Elements
- The first index position in a slice is always `0` and the last one will be (`length of slice - 1`)
- **Using `for` loop**
    - The simplest way to iterate slice
        ```go
        slice := []string{"This", "is", "the", "tutorial", "of", "Go", "language"}
        
        for index := 0; index < len(slice); index++ {
        	fmt.Println(slice[index])
        }
        ```
- **Using `for...range` loop**
    - It is allowed to iterate over a slice using `range` in the `for` loop
        ```go
        slice := []string{"This", "is", "the", "tutorial", "of", "Go", "language"}
        
        for index, elem := range slice {
        	fmt.Printf("index: %d element: %s\\n", index, elem)
        }
        ```
    - Using `range` in the `for` loop, you can get the index and the element value
- **Using a blank (`_`) identifier in `for` loop**
    - If you don’t want to get the index value of the elements, then you can use blank space (`_`) in place of index variable
        ```go
        slice := []string{"This", "is", "the", "tutorial", "of", "Go", "language"}
        
        for _, elem := range slice{
        	fmt.Printf("element: %s\\n", elem)
        }
        ```
- **Using index position**
    - You can access a specific element in a slice by using its index
        ```go
        slice := []int{10, 20, 30}
        fmt.Println(slice[0])
        // Output: 10
        ```
    - Indexes in slices start at `0`, that means that `0` is the first element

## Append Elements
- You can append elements to the end of a slice using the `append()` function
    ```go
    // Syntax
    slice = append(slice, element1, element2, element3, ...)
    
    slice := []int{1, 2, 3, 4, 5}
    slice = append(slice, 6, 7, 8, 9, 10)
    fmt.Println(slice)
    // Output: [1 2 3 4 5 6 7 8 9 10]
    ```
- You can also append all elements of one slice to another slice
    ```go
    slice1 := []int{1, 2, 3}
    slice2 := []int{4, 5, 6}
    slice1 = append(slice1, slice2...)
    fmt.Println(slice1)
    // Output: [1 2 3 4 5 6]
    ```
    
## Deleting from a Slice
- To delete an element from a slice, we can’t directly remove an element from it, we need to perform certain copying of all elements in a different location and then relocate them to a new position in the original slice
- **Using the `append` function to delete an element**
    - We delete or remove the element by referencing its index of it in the slice
    - We have to used the `append` function which will take two parameters
        - First is the slice itself to which we want to append
        - Second is what we want to append i.e. the elements to append into the slice
    
    ```go
    slice := []int{10, 20, 30, 40, 50, 90, 60}
    
    var index int = 3
    
    // Get the element at a provided index in the slice
    elem := slice[index]
    fmt.Println("Element at index 3 is:", elem) // Element at index 3 is: 40
    
    // Using append to combine two slices
    // first slice is the slice of all elements before the given index
    // second slice is the slice of all the elements after the given index
    slice = append(slice[:index], slice[index+1:]...)
    fmt.Println(slice) // [10 20 30 50 90 60]
    ```

## Stacks and Queues
- The idiomatic way to implement a stack or queue in Go is to use a slice directly
    - **Stack (LIFO — Last In, First Out)**
        - To push, you use the built-in `append` function
        - To pop, you slice off the top element
        ```go
        var stack []string
        
        // Push
        stack = append(stack, "world!")
        stack = append(stack, "Hello ")
        
        for len(stack) > 0 {
        	top := len(stack) - 1 // Top element
        	fmt.Print(stack[top])
        	
        	// Pop
        	stack = stack[:top]
        }
        ```
        
    - **FIFO (First In, First Out) Queue**
        - A simple way to implement a temporary queue data structure in Go is to use a slice:
            - To enqueue, you use the built-in `append` function
            - To dequeue, you slice off the first element
            ```go
            var queue []string
            
            // Enqueue
            queue = append(queue, "Hello ")
            queue = append(queue, "world!")
            
            for len(queue) > 0 {
            	fmt.Print(queue[0]) // First element
            	queue = queue[1:]   // Dequeue
            }
            ```
            
        - For a long-living queue, you should probably use a dynamic data structure, such as a linked list
            - The [container/list](https://pkg.go.dev/container/list) package implements a doubly linked list which can be used as a queue
                ```go
                queue := list.New()
                
                queue.PushBack("Hello ") // Enqueue
                queue.PushBack("world!")
                
                for queue.Len() > 0 {
                	e := queue.Front() // First element
                	fmt.Print(e.Value)
                
                	queue.Remove(e) // Dequeue
                }
                ```

## Multi-dimensional Slice
- Multi-dimensional slice is just like the multidimensional array, except that slice does not contain the size
    ```go
    // The [][]int means a slice of slices of ints
    // Each inner slice ([]int) can have different number of columns
    matrix := [][]int{
        []int{1, 2},
        []int{3, 4},
        []int{5, 6},
        []int{7, 8, 9},
        []int{10},
    }
    fmt.Println(matrix)
    
    // The rowIndex is the index of the current row and row is the actual slice
    // at that index ([]int)
    for rowIndex, row := range matrix {
    		// colIndex is the index of the element in the current row and value is
    		// is actual number (int) at that column in that row
        for colIndex, value := range row {
            fmt.Printf("matrix[%d][%d] = %d\\n", rowIndex, colIndex, value)
        }
    }
    ```
    
    **Output**
    
    ```bash
    [[1 2] [3 4] [5 6] [7 8 9] [10]]
    matrix[0][0] = 1
    matrix[0][1] = 2
    matrix[1][0] = 3
    matrix[1][1] = 4
    matrix[2][0] = 5
    matrix[2][1] = 6
    matrix[3][0] = 7
    matrix[3][1] = 8
    matrix[3][2] = 9
    matrix[4][0] = 10
    ```

> [!NOTE]
> - You are allowed to create a `nil` slice that does not contain any element in it
> 	- The capacity and the length of this slice is `0`
> 	- `nil` slice does not contain an array reference
> - When we copy a slice, we are making a copy of its direct part (i.e. `len`, `cap`, `pointer` to elements) and the indirect part (the actual elements are not copied)
> 	```go
> 	x := []int{2, 4, 6, 8, 10}
> 	// We created another slice (y) and we assigned (copied) the value of x to y.
> 	// x and y share the same underlying parts (array elements). So when we modify
> 	// an element of x it also affects y since they share the same underlying
> 	// elements.
> 
> 	y := x
> 
> 	// Modify x
> 	x[0] = 100
> 	fmt.Printf("x: %v    y: %v") // Output: x: [100 4 6 8 10] y: [100 4 6 8 10]
> 	```
> 	
> 	- They both point to the same memory for the actual elements — the “indirect part” (the underlying array) is shared
> 
> - To make a true copy, we use the `copy()` function
> 	```go
> 	x := []int{2, 4, 6, 8, 10}
> 	y := make([]int, len(x))
> 	copy(y, x)
> 
> 	x[0] = 100
> 	fmt.Printf("x: %v    y: %v") // Output: x: [100 4 6 8 10] y: [2 4 6 8 10]
> 	```

## 🔖 Extras
- [Slices in Golang](https://www.geeksforgeeks.org/slices-in-golang/)
- [Ardan Labs — Slice](https://tour.ardanlabs.com/tour/eng/slices/1)
- [Go by Example: Slices](https://gobyexample.com/slices)
- [Two-dimensional slice](https://nanxiao.gitbooks.io/golang-101-hacks/content/posts/two-dimensional-slice.html)
- [W3 Schools — Go Slices](https://www.w3schools.com/go/go_slices.php)
- [Delete Elements in a Slice](https://www.geeksforgeeks.org/delete-elements-in-a-slice-in-golang/)
- [Go Slices: usage and internals](https://go.dev/blog/slices-intro)
- [Slices/arrays explained: create, index, slice, iterate](https://yourbasic.org/golang/slices-explained/)
- [How to Create and Print Multi Dimensional Slice in Golang?](https://www.geeksforgeeks.org/how-to-create-and-print-multi-dimensional-slice-in-golang/)