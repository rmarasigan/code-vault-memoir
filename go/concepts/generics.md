# Generics

- A **template or boilerplate code** written in a form to use on the Go with types that can be added later.
- The primary goal of generics is to **increase code flexibility** by reducing the number of lines required.
- It provides effective tools to achieve greater levels of abstraction in your code and also improve reusability.
- Generics add three new big things to the language:
    - Type parameters for functions and types.
    - Defining interface types as sets of types, including types that don’t have methods.
    - Type interference which permits omitting type arguments in many cases when calling a function.
- **Key Benefits**
    - **Code reuse**: One function or type can be used for multiple types.
    - **Type safety**: Catch type errors at compile time.
    - **Cleaner code**: No repetitive code or risky type casting.
- **Use Cases**
    - Writing generic algorithms, like sorting, searching, or filtering.
    - Creating generic data structures, like linked lists, trees or queues.
    - Developing reusable utility functions for error handling, logging, or caching.
- **Best Practices**
    - Use generics when they simplify your code and improve reusability **without sacrificing readability**.
    - Avoid using overly complex type constraints, as they can make your code harder to understand.
    - When designing generic APIs, consider the trade-offs between flexibility and complexity.
- **Common Misconception in Generics**
    - Generics **don’t allow mixing types** in one instantiation.
        - You cannot have `T` where some elements are `int` and others are `string`.
    - Generics **don’t allow returning different types** at runtime.
        - Generics are resolved at compile time, not runtime.
        - Once you declare `T` it has to be consistent throughout that function call.

## Type Parameters
- These types of parameters can be used with either functions or structs.
- Type parameters are **placeholders for the actual types** that will be used when the generic function or data structure is instantiated.
- It looks like a parameter list, except that it **uses square brackets** instead of parentheses.
- **Syntax**
    
    ```go
    func FunctionaNme[T Constraint](param T) {
    	// Code here...
    }
    ```
    - Where:
        - `T` is the type parameter.
        - `Constraint` specifies allowed types (e.g., `comparable`, `any`, or a custom interface).
- **Example #1**
    ```go
    // The T is a **type parameter**, and int and float are used as a **type**
    // **constraint**. Use the pipe (|) to combine multiple types.
    func circumference**[T** int | float32**]**(radius T) {
    	c := 2 * 3 * radius
    	fmt.Println("The circumference is:", c)
    }
    
    func main() {
    	var radius1 int = 8
    	var radius2 float32 = 9.5
    
    	circumference(radius1) // The circumference is: 48
    	circumference(radius2) // The circumference is: 57
    }
    ```
    
- **Example #2**
    ```go
    func Min[T constraints.Ordered](x, y T) T {
    	if x < y {
    		return x
    	}
    	
    	return y
    }
    
    func main() {
    	// Providing the type arguments to Min, is called ***instantiation***. The
    	// instantiation happens in two steps. First, the compiler substitutes
    	// all type arguments for their respective type parameters throughout
    	// the generic function or type. Second, the compiler verifies that
    	// each type argument satisfies the respective constraint.
    	x := Min[int](2, 3)
    	
    	// This is a non-generic function that can be called just like any
    	// other function.
    	y := Min[float64]
    	z := y(2.71, 3.14)
    	// Output: 2 2.71
    }
    ```
    
- Type parameters can be used with types.
    ```go
    // The generic type Tree stores values of the type parameter T.
    type Tree[T interface{}] struct {
    	left, right *Tree[T]
    	value        T
    }
    
    func (t *Tree[T]) Lookup(x T) *Tree[T] { ... }
    var x Tree[string]
    ```
    
- The **`~T**` form, where `T` is a type literal or type name. `T` must denote a **non-interface type whose underlying is itself**. The `~T` form is called a _tilde form_ or _type tilde_.
    - When you use the `~` symbol in the constraints of a type parameter, you specify a set of types that include all types with the same underlying type as the specified type in the constraint.
    - With the **`~`** character, we are telling the compiler that `T` can be of any type whose underlying type is `T`.
    - **Example**
        ```go
        // We want this function to accept any type whose underlying type is
        // either uint, int or int64.
        func Min[T ~uint | ~int | ~int64](a, b T) T {
        	if a < b {
        		return a
        	}
        	
        	return b
        }
        ```

## Type Constraint
- A **rule or requirement** that restricts what types the type parameter can take.
- In Go, type constraints **must be interfaces**. That is, an interface type can be used as a value type, and it can also be used as a meta-type (_type constraint_).
- A constraint means a type constraint; it is used to constrain some type parameters.
- It is written right **after the type parameter** name.
- **Common Constraints**
    - `any`: Any type at all (e.g., `int`, `string`, `struct`, etc.)
    - `comparable`: Types that support `==` and `!=` (e.g., `int`, `string`, `bool`).
    - Custom Interface: User-defined interface with methods.
- **Example**
    ```go
    type Number interface {
    	int | float64
    }
    
    // Number is the constraint (T must be int or float64). T is the type
    // parameter.
    func Sum[T Number](a, b T) T {
    	return a + b
    }
    ```

### Slice Constraints
```go
// operateFunc defines a function type that takes a value of some type T and
// returns a value of the same type T (to be determined later).
type operateFunc[T any] func(t T) T

// Slice defines a constraint that the data is a slice of some type T (to be
// determined later).
type Slice[T any] interface {
	~[]T
}

// When it's important that the slice being passed in is exactly the same
// as the slice being returned, use a slice constraint. This ensures that the
// result slice S is the same as the incoming slice S.
func operate[S Slice[T], T any](slice S, fn operateFunc[T]) S {
	ret := make(S, len(slice))
	
	for i, v := range slice {
		ret[i] = fn(v)
	}
	
	return ret
}

// If you don't care about the constraint defined above, then operate2 provides
// a simpler form. operate2 still works because you can assign a slice of some
// type T to the input and output arguments. However, the concrete types of the
// input and output arguments will be based on the underlying types. In this
// case not a slice of Numbers, but a slice of integers. This is not the case
// with operate function above.
func operate2[T any](slice []T, fn operateFunc[T]) []T {
	ret := make([]T, len(slice))
	
	for i, v := range slice {
		ret[i] = fn(v)
	}
	
	return ret
}

// I suspect most of the time operate2 is what you want: it's simpler, and more
// flexible: You can always assign a []int back to a Numbers variable and vice
// versa. But if you need to preserve that incoming type in the result for some
// reason, you will need to use operate.
//
// This code defines a named type that is based on a slice of integers. An
// integer is the underlying type.
type Numbers []int

// Double is a function that takes a value of type Numbers, multiplies each
// value in the underlying integer slice and returns that new Numbers value.
func DoubleUnderlying(n Numbers) Numbers {
	fn := func(n int) int {
		return 2 * n
	}

	numbers := operate2(n, fn)
	fmt.Printf("%T", numbers)
	
	return numbers
}

func DoubleUserDefined(n Numbers) Numbers {
	fn := func(n int) int {
		return 2 * n
	}

	numbers := operate(n, fn)
	fmt.Printf("%T", numbers)
	
	return numbers
}

func main() {
	n := Numbers{10, 20, 30, 40, 50}
	fmt.Println(n)

	n = DoubleUnderlying(n)
	fmt.Println(n)

	n = DoubleUserDefined(n)
	fmt.Println(n)
}

// OUTPUT
// [10 20 30 40 50]
// []int[20 40 60 80 100]
// main.Numbers[40 80 120 160 200]
```

### Generic Worker
```go
// Generic function type that accepts a context and returns a Result of any type.
type doworkFn[Result any] func(context.Context) Result

// doWork launches the given work function in a goroutine.
// It returns a channel that will receive the result. The caller can wait for
// either the result or a context timeout.
func doWork[Result any](ctx context.Context, work doworkFn[Result]) chan Result {
	// Creates a buffered channel so send won't block with a capacity of 1.
	ch := make(chan Result, 1)
	
	// Starts a goroutine and runs the work function with ctx.
	go func() {
		// Sends the result into ch.
		ch <- work(ctx)
		fmt.Println("doWork : work complete")
	}()

	return ch
}

func main() {
	// Create a context with 100ms timeout.
	ctx, cancel := context.WithTimeout(context.Background(), 100*time.Millisecond)
	
	// Ensure resources are cleaned up.
	defer cancel()

	// Define a sample work function that takes variable time.
	dwf := func(ctx context.Context) string {
		// Sleep for random duration between 0–200ms
		time.Sleep(time.Duration(rand.Intn(200)) * time.Millisecond)
		return "work complete"
	}

	// Use select to wait for either:
	// 1. The result from doWork.
	// 2. A timeout from the context.
	select {
	case v := <-doWork(ctx, dwf):
		// If the goroutine finishes before 100ms, it receives v and prints
		// "doWork : work complete" and "main: work complete"
		fmt.Println("main:", v)
		
	case <-ctx.Done():
		// If it takes longer than 100s, the context times out, and "main: timeout"
		// is printed.
		fmt.Println("main: timeout")
	}
}
```

## Type Sets
- A type set consists of only **non-interface types**.
- Defining the **collection of types that a type parameter can accept** when used as a constraint. Every constraint, whether it is a built-in interface like `any` or `comparable`, or a custom interface, has an associated type set.
- The concept of type sets allows the Go compiler to determine which operations are valid on a type parameter `T` with a given constraint.
- Type sets can be **defined using several syntactic constructs**. A union element, written as a sequence of types or approximation elements separated by a pipe (`|`), creates a type set that is the union of the type sets of the individual elements.
- **Examples**
    - **Union of types**
        ```go
        // Type set: int, float64
        type Number interface {
        	int | float64
        }
        ```
    - **Predeclared constraint `any`**
        ```go
        // any means "all types".
        // Type set: the set of all possible Go types.
        func Print[T any](x T) { 
        	fmt.Println(x)
        }
        ```
    - **Method in constraints**
        ```go
        // Type set: all types that have a String() string method.
        type Stringer interface {
        	String() string
        }
        ```
- **Key Points**
    - **Type parameter** is a placeholder (`T`).
    - **Constraint** is a rule (`Number`).
    - **Type set** is the actual set of types that satisfy the rule.

## Parameterized Types
- **Example**
    ```go
    // It holds the types that we can pass to the function, which are: int64,
    // int8, and float64.
    type Radius interface {
    	int64 | int8 | float64
    }
    
    // We pass an instance of the interface we declared, Radius. If we invoke
    // the circumference function with a float64, then T in the context of
    // this function is a value with type float64.
    func circumference[T Radius](radius T) {
    	var c T
    	c = 2 * 3 * radius
    	
    	fmt.Println("The circumference is:", c)
    }
    ```

## Generic Zero Value
```go
package main

import "fmt"

// A generic type named keymap that uses an underlying type of map with a key
// of type string and a value of some type T.
type keymap[T any] map[string]T

// A method named set that accepts a key of type string and a value of type T.
func (km keymap[T]) set(k string, v T) {
	km[k] = v
}

// A method named get that accepts a key of type string and return a value of
// type T and true or false if the key is found.
func (km keymap[T]) get(k string) (T, bool) {
	var zero T

	v, found := km[k]
	if !found {
		return zero, false
	}

	return v, true
}

func main() {
	// Construct a value of type keymap that stores integers.
	km := make(keymap[int])

	// Add a value with key "jack" and a value of 10.
	km.set("jack", 10)

	// Add a value with key "jill" and a value of 20.
	km.set("jill", 20)

	// Get the value for "jack" and verify it's found.
	jack, found := km.get("jack")
	if !found {
		fmt.Println("jack not found")
		return
	}

	// Print the value for the key "jack".
	fmt.Println("jack", jack) // jack 10

	// Get the value for "jill" and verify it's found.
	jill, found := km.get("jill")
	if !found {
		fmt.Println("jill not found")
		return
	}

	// Print the value for the key "jill".
	fmt.Println("jill", jill) // jill 20
}
```

## Generic Struct Types
This code defines two user-defined types that implement a linked list. The `node` type contains data of some type `T` (to be determined later) and points to other nodes of the same type `T`. The list type contains pointers to the `first` and the `last` nodes of some type `T`. The `add` method is declared with a pointer receiver based on a list of some type and is implemented to add nodes to the list of the same type `T`.

```go
// A node holds one piece of data (of generic type T) and links to
// the next and previous nodes in the list.
type node[T any] struct {
	Data T         // the actual value we store
	next *node[T]  // pointer to the next node (nil if none)
	prev *node[T]  // pointer to the previous node (nil if none)
}

// A list keeps track of the first and last nodes.
// It doesn't store values directly, just manages the linked structur
type list[T any] struct {
	first *node[T] // pointer to the first node in the list
	last  *node[T] // pointer to the last node in the list
}

// add inserts a new node holding `data` at the end of the list.
// It updates the list pointers (first/last) accordingly.
func (l *list[T]) add(data T) *node[T] {
	// Create a new node containing the data.
	// Its `prev` points to the current last node (if any).
	n := node[T]{
		Data: data,
		prev: l.last,
	}
	
	// If the list is empty, this new node becomes both
	// the first and last node.
	if l.first == nil {
		l.first = &n
		l.last = &n
		return &n
	}
	
	// Otherwise, link the current last node to the new one.
	l.last.next = &n
	
	// Update the list's last pointer to the new node.
	l.last = &n
	
	return &n
}

// This user type represents the data to be stored into the linked list.
type user struct {
	name string
}

func main() {
	// Store values of type user into the list.
	var lv list[user]
	n1 := lv.add(user{"bill"})
	n2 := lv.add(user{"ale"})
	fmt.Println(n1.Data, n2.Data)

	// Store pointers of type user into the list.
	var lp list[*user]
	n3 := lp.add(&user{"bill"})
	n4 := lp.add(&user{"ale"})
	fmt.Println(n3.Data, n4.Data)
	// OUTPUT
	// {bill} {ale}
	// &{bill} &{ale}
}
```

## Generic Functions
```go
func Max[T comparable](a, b T) T {
	if a > b {
		return a
	}
	
	return b
}
```

## Generic Data Structures
```go
// This defines a generic structure, where T is a type parameter that can be
// any type.
type Stack[T any] struct {
	// It is a slice used to store elements in the stack. The element type
	// of the slice is the generic type T.
	elements []T
}

// Push accepts a value of some type T and appends the value to the slice.
func (s *Stack[T]) Push(element T) {
	s.elements = append(s.elements, element)
}

// Pop returns the latest value of some type T that was appended to the slice
// and an error.
func (s *Stack[T]) Pop() (T, error) {
	var zero T
	
	if len(s.elements) == 0 {
		return zero, errors.New("stack is empty")
	}
	
	element := s.elements[len(s.elements)-1]
	s.elements = s.element[:len(s.elements)-1]
	
	return element, nil
}

func main() {
	// Constructs a value of type stack that stores integers.
	var s Stack[int]

	// Push the values of 10 and 20 to the stack.
	s.Push(10)
	s.Push(20)

	// Pop a value from the stack.
	v, err := s.Pop()
	if err != nil {
		fmt.Println(err)
		return
	}

	// Print the value that was popped.
	fmt.Println(v) // 20

	// Pop another value from the stack.
	v, err = s.Pop()
	if err != nil {
		fmt.Println(err)
		return
	}

	// Print the value that was popped.
	fmt.Println(v) // 10

	// Pop another value from the stack. This should return an error.
	v, err = s.Pop() // stack is empty
	if err != nil {
		fmt.Println(err)
		return
	}
}
```

## Generic Member Functions
```go
func (b Box[T]) Empty() bool {
	return b.Content == nil
}
```

## Generics as Methods of Interfaces
```go
// The Container interface can be used for any type of container, such as
// slice, list, or custom container type, as long as these containers
// implement the Add and Get methods.
type Container[T any] interface{
	Add(element T)
	Get(index int) T
}
```

## Building a Cache Component Using Generics
```go
type Cache[T any] struct {
	store map[string]T
}

func (c *Cache[T]) Set(key string, value T) {
	c.store[key] = value
}

func (c *Cache[T]) Get(key string) (T, bool) {
	value, exists := c.store[key]
	return value, exists
}

func main() {
	c := &Cache[string]{
		store: make(map[string]string),
	}
	
	c.Set("name", "John")
	value, exists := c.Get("name")
	fmt.Println(exists, value) // true John
}
```

## 🔖 Extras
- [Generics in Golang](https://www.geeksforgeeks.org/go-language/generics-in-golang/)
- [Why Generics?](https://go.dev/blog/why-generics)
- [Learning Generics in Go](https://programmingpercy.tech/blog/learning-generics-in-go/)
- [An Introduction To Generics](https://go.dev/blog/intro-generics)
- [Introduction to Go Generics](https://ganhua.wang/introduction-to-go-generics)
- [Crash Course on Go Generics](https://www.calhoun.io/crash-course-on-go-generics/)
- [Constraints and Type Parameters](https://gfw.go101.org/generics/555-type-constraints-and-parameters.html)
- [Tutorial: Getting started with generics](https://go.dev/doc/tutorial/generics)
- [Ultimate Go Tour — Generics - Basics](https://tour.ardanlabs.com/tour/eng/generics-basics/1)
- [What Does the Tilde Mean in Go Generics](https://www.calhoun.io/what-does-the-tilde-mean-in-go-generics/)
- [A Comprehensive Guide to Generics in Go](https://itnext.io/a-comprehensive-guide-to-generics-in-go-5a9dcda5669c)
- [Mastering Generics in Go: A Comprehensive Guide](https://medium.com/hprog99/mastering-generics-in-go-a-comprehensive-guide-4d05ec4b12b)
- [Generics in Go: Your Friendly Guide to Reusable Code](https://dev.to/shrsv/generics-in-go-your-friendly-guide-to-reusable-code-4fkc)

## 📽️ Videos
- [Generics in Go: Draft Proposal Review](https://www.youtube.com/watch?v=gIEPspmbMHM)