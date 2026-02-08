# Functions

- A fundamental building block of programs and are **used to encapsulate and organize code** for **better modularity and re-usability**.
- Functions in Go are **first-class citizens**.
    - **First-class citizen** term refers to the status of certain entities, such as values, types, and functions that are treated equally and have the same capabilities as other entities in the language. These entities can be assigned to variables, passed as arguments to functions, and returned as values from functions.
    - This allows Go to support higher-order functions, where functions can operate on other functions.
- Are **blocks of code that perform specific tasks**, which can be reused throughout the program to save memory, improve readability, and save time.
- Functions **can take zero or more parameters**, which are the values passed to the function when it is called.
- Functions **can return zero, one, or multiple value** (or none, in which case you can omit the return type). Returning multiple values is useful in cases where you can to return more than one result from a function. The **`return`** statement is used to specify the value to be returned.
- Go **allows you to specify names for the return values** in a function's signature. _Named return values_ are automatically initialized and can be used as regular variables within the function. They are particularly useful when dealing with complex functions or error handling.

## Syntax and Declaration
- **Syntax**
    ```go
    func functionName(parameterName type, ...) returnType {
    	// Function Body
    }
    ```
    - Where:
        - **`func`**: The keyword to declare a function.
        - **`functionName`**: The name of the function.
        - **`parameterName type`**: These are the parameters accepted by the function. You can provide any number of parameters separated by a comma. Each parameter has a name and a type.
        - **`returnType`**: It specifies the return type. If a function doesn't return any value, the return type can be omitted.
        - **`{ // Function Body }`**: It can contain statements and also a return statement to return a value from the function.
- **Declaration**
    - To declare a function in Go, you use the **`func`** keyword followed the function name, a list of parameters enclosed in parentheses, and an optional return type.
    - **Example**
        ```go
        // multiply multiplies two integers and returns the result.
        func multiply(a, b int) int {
        	return a * b
        }
        ```
        
    - **Variadic Function**
        - **Syntax**
            ```go
            func functionName(parameterName ...Type) returnType {
            	// Function Body
            }
            ```
            - Where:
                - **`parameters ...Type`**: Indicates that the function can accept a variable number of arguments of type **`Type`**.
        - It accepts multiple argument so of the same type and can be called with any number of arguments, including none. This feature is useful when you don't know beforehand how many arguments you will pass.
        - When defining a variadic function, you specify the type of arguments followed by an **ellipsis (`…`)**. Inside the function, these arguments can be treated as a slice.
        - The variadic parameter **must always be the last parameter**.
        - **Example**
            ```go
            func sum(nums ...int) int {
            	total := 0
            	
            	for _, num := range nums {
            		total += num
            	}
            	
            	return total
            }
            
            func main() {
            	fmt.Println("Sum of 1, 2, 3:", sum(1, 2, 3))
            	
            	nums := []int{4, 5}
            	fmt.Println("Sum of 4, 5:", sum(nums...))
            	
            	fmt.Println("Sum of no numbers:", sum())
            }
            ```
            
        - **Limitations**
            - Can only have one variadic parameter, and it must be the last parameter.
            - Cannot have multiple variadic parameters in a single function definition.

## Function Calling
- To call a function in Go, you use the function name followed by a list of arguments enclosed in parentheses. If the function has multiple return values, you can capture them in variables.
- **Example**
    ```go
    // Call the multiply function and assign its result to result varibale
    result := multiply(5, 10)
    fmt.Println("Result:", result)
    ```

## Parameters / Arguments
- Information can be passed to functions as a parameter. Parameters acts as variables inside the function.
- Parameters and their types are specified after the function name, inside the parentheses. You can add as many parameters as you want, just separate them with a comma.
    ```go
    func functionName(parameter1 type, parameter2 type, ...) {}
    ```
    
- When working with multiple parameters, the function call must have the same number of arguments as there are parameters, and the arguments must be passed in the same order.
- **Example**
    ```go
    // We can view parameter and named return values as standard variable
    // declarations without the var keywords. Both parameters and named returned
    // values are treated as local variables within their corresponding function
    // bodies.
    func SquaresOfSumAndDiff(a int64, b int64) (s int64, d int64) {
    	x, y := a + b, a - b
    	
    	s = x * x
    	d = y * y
    	
    	return // <=> return s, d
    }
    ```
    - **`SquareOfSumAndDiff`**: The function name, which must be an identifier.
    - **`a int64, b int64`**: The _parameter_ declaration list, which is enclosed in a pair of `()`.
    - **`s int64, d int64`**: The _named return values_ that is also enclosed in a pair of `()`. However, if the list is blank or it is composed of only one anonymous result declaration, then the pair of `()` in the result declaration is optional. The initial value of each result is zero value of its type.
    - **`return`**: It is used to end the normal forward execution flow and enter the exiting phase of a call of the function.
- Go supports two ways to pass arguments to function: **Call by Value** and **Call by Reference**. By default, Go uses call by value, meaning values are copied, and changes inside the function do not affect the caller's variable.

### Call by Value
- The values of the arguments are copied to the function parameters, changes in the function do not affect the original variables.
- **Example**
    ```go
    func multiply(a, b int) int {
    	a = a * 2 // modifying a inside the function
    	return a * b
    }
    
    func main() {
    	x := 5
    	y := 10
    	fmt.Printf("Before: x = %d, y = %d\\n", x, y)
    	
    	result := multiply(x, y)
    	
    	fmt.Println("multiplication:", result)
    	fmt.Printf("After: x = %d, y = %d\\n", x, y)
    }
    ```

### Call by Reference
- Pointers are used so that changes inside the function reflect in the caller's variables.
- **Example**
    ```go
    func multiply(a, b *int) int {
    	*a = *a * 2 // modifying a's value at its memory address
    	return *a * *b
    }
    
    func main() {
    	x := 5
    	y := 10
    	fmt.Printf("Before: x = %d, y = %d\\n", x, y)
    	
    	result := multiply(&x, &y)
    	
    	fmt.Printf("multiplication: %d\\n", result)
    	fmt.Printf("After: x = %d, y = %d\\n", x, y)
    }
    ```

## Function Returns
- **Named Return**
    ```go
    func SquaresOfSumAndDiff(a int64, b int64) (s int64, d int64) {
    	// Function Body
    }
    ```
    - Named returns are less readable and are not idiomatic.
- **Anonymous Return**
    ```go
    func SquaresOfSumAndDiff(a int64, b int64) (int64, int64) {
    	// Function Body
    }
    ```

## Function Naming Rules
- A function name **must start with a letter**.
- It can only contain alpha-numeric characters and underscores (`A-Z`, `a-z`, `0-9`, and `_`).
- Function names **are case-sensitive** and **cannot contain spaces**.
- If the function name consists of multiple words, techniques introduced for _multi-word variable naming_ can be used.
    - **Camel Case**
        - Each word, except the first, starts with a capital letter: `squareOfSumAndDiff()`
    - **Pascal Case**
        - Each word starts with a capital letter: `SquareOfSumAndDiff()`
    - **Snake Case**
        - Each word is separated by an underscore character: `Square_Of_Sum_And_Diff()`
- Give the function a name that reflects what the function does.

## Anonymous Function
- **Syntax**
    ```go
    func(params) returnType {
    	// Use return statement if returnType is given. If returnType is not given,
    	// then do not use return statement
    	return
    }()
    ```
- In Go, an anonymous function can also form a closure. Also known as a function literal.
- It is often also called a _function literal_, _lambda_, or _closure_, is a function that is defined without a given name.
- They can be used for creating inline functions, closures, and even for passing and returning functions.
- **Example**
    ```go
    func main() {
    	// Anonymous function
    	func() {
    		fmt.Println("Hello, World!")
    	}()
    	
    	// Passing argument in anonymous function
    	func(val string) {
    		fmt.Println("My name is", val)
    	}("John Doe")
    }
    ```

> [!NOTE]
> An anonymous function can access and modify variables defined outside of the function itself. This is a powerful feature, but it can also lead to unexpected side effects if not used carefully.

## **`func` Expression**
- _**What is an expression?**_
    - A combination of values, variables, operators, and function calls that are evaluated to **produce a single value**.
    - **Examples**
        - **Literal Values**
            - `42`: An _integer literal_ expression representing the value 42.
            - `"Hello, World!"`: A _string literal_ expression representing the text "Hello World!".
        - **Variables**
            - `x`: A variable expression representing the value stored in the variable `x`.
        - **Arithmetic Expressions**
            - `x + y`: An arithmetic expression that adds the values of variables `x` and `y`.
            - `5 * (x - 2)`: An arithmetic expression involving multiplication, subtraction, and parentheses.
        - **Function Calls**
            - `math.Sqrt(16)`: A function call expression invoking the `Sqrt` function from the `math` package with an argument `16`.
        - **Logical Expressions**
            - `x > 10 && y < 20`: A logical expression combining the greater-than and less-than operators with logical AND.
        - **Type Conversions**
            - `float64(x)`: A type conversion expression that converts the value of `x` to a `float64`.
    - Expressions are the building blocks of Go programs, and they are **used to perform computations, manipulate values, and make decisions based on conditions**. They can be assigned to variables, passed as arguments to functions, or used in control flow statements like `if` statements and loops.
    - The ability to treat functions as first-class citizens in Go allows for more flexible and modular code, enabling the use of higher-order functions, function composition and other functional programming techniques.
- **Example**
    ```go
    func main() {
    	// You can assign an anonymous function to a variable.
    	val := func() {
    		fmt.Println("Hello, World!")
    	}
    	
    	val()
    }
    ```
    
- **Passing Arguments**
    ```go
    func main() {
    	// Passing arguments in anonymous function
    	func(val string) {
    		fmt.Println(val)
    	}("John Doe")
    	
    	multiply := func(a, b int) int {
    		return a * b
    	}
    	
    	result := multiply(5, 10)
    	fmt.Println(result)
    }
    ```

## **Returning Anonymous Function**
- Returning a function in the Go programming language is a form of using higher-order functions, which are functions that can accept other functions as arguments and/or return other functions as results.
- **Example**
    ```go
    // Returning anonymous function from another function.
    func Path() func(base, sub string) string {
    	out := func(base, sub string) string {
    		return base + sub + ".com"
    	}
    	
    	return out
    }
    
    // It creates a variable i and then defines an anonymous function that
    // increments i by one each time it's called. This anonymous function
    // captures the i variable from its enclosing scope, a feature known as
    // closure. The incrementer function returns this anonymous function.
    func incrementer() func() int {
    	i := 0
    	
    	// Define and return an anonymous function
    	return func() int {
    		i += 1
    		return i
    	}
    }
    
    func main() {
    	val := Path()
    	fmt.Println(val("<https://example.com/>", "go-lang"))
    	
    	// We call the incrementer() function and assign the returned function
    	// to the increment variable. This variable is now itself a function
    	// that we can call, and each time we call it, it increments its
    	// internal i counter and returns the new value. 
    	increment := incrementer()
    	fmt.Println(increment())
    	fmt.Println(increment())
    	fmt.Println(increment())
    }
    ```

## **Callback / Passing a `func` as an argument**
- The term "callback" in programming refers to **a function or a piece of code that is passed as an argument to another function**. The term itself can be understood by breaking it down into "call" and "back".
    - **call**: Refers to the action of invoking or execution a function. When a _function is called_, it starts executing its code, performs certain operations, and then returns a result.
    - **back**: Refers to the idea that the callback function is _called back or invoked by the original function_ after it has completed its execution or reach a specific point in its code.
- Instead of the original function returning a result and ending, it "calls back" the call back function, allowing it to execute some additional code or handle specific tasks.
- Often used in event-driven programming or asynchronous operations, where the flow of execution is not linear.
- **Example**
    ```go
    // Passing anonymous function as an argument
    func Path(i func(base, sub string) string) {
    	fmt.Println(i("<https://example>", "go-lang"))
    }
    
    func main() {
    	val := func(base, sub string) string {
    		return base + ".com/" + sub
    	}
    	
    	Path(val)
    }
    ```

## Closure
- One scope enclosing other scopes. The variables declared in the outer scope are accessible in inner scopes.
- Refers to the combination of a function (or a method) and the environment in which it was defined. It allows a function to access variables and bindings that are outside of its own scope, even after the outer function has finished executing.
- It is created when a nested function references a variable from its containing (parent) function. The nested function retains a reference to the environment in which it was defined, so it can access and manipulate variables from that environment.
- Particularly useful in situations where you want to create functions that have access to certain variables or data that persist across multiple invocations. They enable you to encapsulate data and behavior together, creating self-contained functions with their own private state.
- **Example**
    ```go
    func incrementor() func() int {
    	x := 0
    
    	return func() int {
    		x++
    		return x
    	}
    }
    
    func main() {
    	x := incrementor()
    	fmt.Println(x()) // 1
    	fmt.Println(x()) // 2
    	fmt.Println(x()) // 3
    	fmt.Println(x()) // 4
    	fmt.Println(x()) // 5
    	fmt.Println(x()) // 6
    
    	y := incrementor()
    	fmt.Println(y()) // 1
    	fmt.Println(y()) // 2
    	fmt.Println(y()) // 3
    	fmt.Println(y()) // 4
    	fmt.Println(y()) // 5
    	fmt.Println(y()) // 6
    }
    ```
    
- **Use Cases**
    - **Isolating data**
        - A function that has access to data that persists even after the function exists, but you don't want anyone else to have access to that data.
        - **Example**
            ```go
            func makeFibGen() func() int {
              f1 := 0
              f2 := 1
            
              return func() int {
                f2, f1 = (f1 + f2), f2
                return f1
              }
            }
            
            func main() {
              gen := makeFibGen()
              
              for i := 0; i < 10; i++ {
                fmt.Println(gen())
              }
            }
            ```
            
    - **Middleware in Web Applications**
        - Middleware is basically a fancy term for reusable function that can run code both before and after your code designed to handle web request.
        - Using middleware is common when writing web applications, and they can be useful for more than just timers.
        - **Example**
            ```go
            type Middleware func(http.HandlerFunc) http.HandlerFunc
            
            func createMiddlewareChain(middlewares ...Middleware) Middleware {
                return func(final http.HandlerFunc) http.HandlerFunc {
                    return func(w http.ResponseWriter, r *http.Request) {
                        // Build the chain from the inside out
                        chain := final
                        for i := len(middlewares) - 1; i >= 0; i-- {
                            chain = middlewares[i](chain)
                        }
                        chain(w, r)
                    }
                }
            }
            
            func withLogging(next http.HandlerFunc) http.HandlerFunc {
                return func(w http.ResponseWriter, r *http.Request) {
                    startTime := time.Now()
                    
                    // Call the wrapped handler
                    next(w, r)
                    
                    // Log after request is processed
                    fmt.Printf("Request to %s took %v\\n", 
                        r.URL.Path, 
                        time.Since(startTime))
                }
            }
            
            func withAuth(next http.HandlerFunc) http.HandlerFunc {
                return func(w http.ResponseWriter, r *http.Request) {
                    if token := r.Header.Get("Authorization"); token == "" {
                        http.Error(w, "Unauthorized", http.StatusUnauthorized)
                        return
                    }
                    next(w, r)
                }
            }
            
            func main() {
                handler := func(w http.ResponseWriter, r *http.Request) {
                    fmt.Fprintln(w, "Hello, World!")
                }
                
                chain := createMiddlewareChain(withLogging, withAuth)
                http.HandleFunc("/", chain(handler))
                http.ListenAndServe(":8080", nil)
            }
            ```
    - **Event Emitter Pattern**
        ```go
        type EventEmitter struct {
            listeners map[string][]func(interface{})
        }
        
        func NewEventEmitter() *EventEmitter {
            return &EventEmitter{
                listeners: make(map[string][]func(interface{})),
            }
        }
        
        func (e *EventEmitter) On(event string, callback func(interface{})) func() {
            e.listeners[event] = append(e.listeners[event], callback)
            
            // Return unsubscribe function
            return func() {
                listeners := e.listeners[event]
                for i, cb := range listeners {
                    if &cb == &callback {
                        e.listeners[event] = append(listeners[:i], listeners[i+1:]...)
                        break
                    }
                }
            }
        }
        
        func (e *EventEmitter) Emit(event string, data interface{}) {
            if listeners, ok := e.listeners[event]; ok {
                for _, callback := range listeners {
                    callback(data)
                }
            }
        }
        
        func main() {
            emitter := NewEventEmitter()
            
            // Subscribe to events
            unsubscribe := emitter.On("userLoggedIn", func(data interface{}) {
                if user, ok := data.(string); ok {
                    fmt.Printf("User logged in: %s\\n", user)
                }
            })
            
            // Emit events
            emitter.Emit("userLoggedIn", "john.doe")
            
            // Unsubscribe
            unsubscribe()
        }
        ```
        
    - **Rate Limiter Implementation**
        ```go
        func newRateLimiter(requests int, duration time.Duration) func() bool {
            timestamps := make([]time.Time, 0, requests)
            
            return func() bool {
                now := time.Now()
                
                // Remove timestamps outside the window
                for len(timestamps) > 0 && now.Sub(timestamps[0]) > duration {
                    timestamps = timestamps[1:]
                }
                
                // Check if we can make a new request
                if len(timestamps) < requests {
                    timestamps = append(timestamps, now)
                    return true
                }
                
                return false
            }
        }
        
        func main() {
            // Allow 3 requests per second
            isAllowed := newRateLimiter(3, time.Second)
            
            for i := 0; i < 5; i++ {
                fmt.Printf("Request allowed: %v\\n", isAllowed())
                time.Sleep(300 * time.Millisecond)
            }
        }
        ```
        
    - **Be Mindful of Resource Management**
        ```go
        func createResourceManager() func() {
            resource := acquireExpensiveResource()
            
            // Return cleanup function
            return func() {
                resource.Release()
            }
        }
        
        func main() {
            cleanup := createResourceManager()
            defer cleanup() // Important!
        }
        ```
        
- **Best Practices**
    - **Keep It Simple**
        - Closures should have a single, clear purpose.
        - Avoid capturing too many variables.
    - **Be Careful with Goroutines**
        - Always pass loop variables as parameters when using closures in goroutines.
        - Be aware of variable lifetime and scope.
    - **Use for Encapsulation**
        - Closures are great for hiding implementation details.
        - They help create clean APIs.

## Recursion
- It refers to the technique of solving a problem by breaking it down into smaller sub-problems of the same type.
- A recursive function is **a function that calls itself during its execution**.
- When a recursive function is called, it solves a smaller instance of the same problem, and then combines the result of the smaller instance with the current instance to obtain the final result. The function continues to call itself on smaller sub-problems until it reaches a base case, which is a simple case that can be solved directly without further recursion.
- It is important to design recursive functions carefully, ensuring that they have well-defined base cases and properly handle the termination condition, to avoid infinite recursion.
- **Components**
    - **Base Case**
        - The base case in a recursive algorithm is a condition or set of conditions that stop the recursion.
        - **Example**
            ```go
            // This is a base case because it terminates the recursion.
            if n > 100 {
                return n - 10
            }
            ```
    - **Recursive Case**
        - The recursive case in a recursive algorithm is the part of the function where the function calls by itself.
        - **Example**
            ```go
            // The function keeps calling itself deeper and deeper until it
            // eventually hits the base case.
            return nestedRecursion(nestedRecursion(n + 11))
            ```
- **Direct Recursion**
    - The function calls itself directly, without the assistance of another function is called a direct recursion.
    - **Example**
        ```go
        func main() {
        	fmt.Println(factorial(4))
        	fmt.Println(factorialLoop(4))
        }
        
        // Recursive function for calculating a factorial of an integer.
        func factorial(n int) int {
        	// This is the base condition if number is 0, the function will return 1.
        	if n == 0 {
        		return 1
        	}
        	
        	// Recursive call to itself with argument decremented by 1, so that it
        	// eventually reaches the base case.
        	return n * factorial(n -1)
        }
        
        func factorialLoop(n int) int {
        	total := 1
        	
        	for n > 0 {
        		total *= n
        		n--
        	}
        	
        	return total
        }
        // Output: 24 24
        ```
- **Indirect Recursion**
    - The indirect recursion occurs when a function calls another function, which later calls the first function again. It involve two or more functions that call each other in a specific order.
    - It takes the assistance of another function. The function does call itself, but indirectly, i.e., through another function.
    - **Example**
        ```go
        // Recursive function that prints the current number and calls printTwo.
        // This is part of a mutually recursive pair (printOne <-> printTwo).
        func printOne(n int) {
        	// Continue only if n is non-negative (base case is when n < 0).
        	if n >= 0 {
        		// Indirect recursion: call printTwo, which will call printOne again.
        		fmt.Println("PrintOne:", n)
        		printTwo(n - 1)
        	}
        }
        
        func printTwo(n int) {
        	// Continue only if n is non-negative (base case is when n < 0).
        	if n >= 0 {
        		fmt.Println("PrintTwo:", n)
        		
        		// Continue the mutual recursion by calling printOne.
        		printOne(n - 1)
        	}
        }
        
        func main() {
        	// Start mutual recursion with printOne(10), prints numbers from 10
        	// down to 0.
        	printOne(10)
        	
        	// This won't print anything because n is less than 0, so the base case
        	// is hit immediately.
        	printOne(-1)
        	// OUTPUT
        	// PrintOne: 10
        	// PrintTwo: 9
        	// PrintOne: 8
        	// PrintTwo: 7
        	// PrintOne: 6
        	// PrintTwo: 5
        	// PrintOne: 4
        	// PrintTwo: 3
        	// PrintOne: 2
        	// PrintTwo: 1
        	// PrintOne: 0
        }
        ```
- **Tail Recursion**
    - A special form of recursion where the **recursive call is the last operation in the function**. This means that there are no other additional calculations or operations after the recursive call.
    - **Example**
        ```go
        // Tail recursive function to print numbers from n to 1 in decreasing
        // order.
        func printNum(n int) {
        	// Base case: if n is 0 or less, stop the recursion.
        	if n > 0 {
        		// Print the current number before the recursive call (tail
        		// recursion).
        		fmt.Println(n)
        		
        		// Recursive call is the last action — this makes it tail recursion.
        		printNum(n - 1)
        	}
        }
        
        func main() {
        	printNum(5) // Output: 5 4 3 2 1
        }
        ```
- **Head Recursion**
    - The head recursion occurs when a function makes its **recursive call at the beginning (or "head") of the function**, before performing any other operations. This means the recursive call is made first, and other operations are performed after the recursive call.
    - **Example**
        ```go
        // Head recursive function to print numbers from 1 to n in increasing
        // order.
        func printNum(n int) {
        	// Base case: if n is 0 or less, stop the recursion.
        	if n > 0 {
        		// Recursive call comes first (head recursion), so numbers are printed
        		// after the call stack unwinds.
        		prtinNum(n - 1)
        		
        		fmt.Println(n)
        	}
        }
        
        func main() {
        	printNum(5) // Output: 5 4 3 2 1
        }
        ```
- **Mutual Recursion**
    - The mutual recursion occurs when **two or more functions call each other in a cyclic manner**, creating a loop of function calls between the function involved. It can involve any number of functions, but the simplest case involves two functions calling each other.
    - **Example**
        ```go
        // It checks if n is odd and next to determine if a number n is odd.
        func isEven(n int) bool {
           if n == 0 {
              return true
           }
           
           return isOdd(n - 1)
        }
        
        // It checks if n is even.
        func isOdd(n int) bool {
           if n == 0 {
              return false
           }
           return isEven(n - 1)
        }
        
        func main() {
        	n := 5
        	fmt.Println("Is even:", isEven(n)) // Is even: false
        	fmt.Println("Is odd:", isOdd(n))   // Is odd: true
        }
        ```
- **Infinite Recursion**
    - It happens when a recursive function doesn't have a proper base case to stop the recursion. This results in the function calling itself endlessly, causing a **stack overflow error** due to continuous function calls.
- **Nested Recursion**
    - It occurs when a **function's recursive call itself contains another recursive call as a parameter** means one recursive call is nested inside another.
    - **Example**
        ```go
        func nestedRecursion(n int) int {
           // Base case: if n is greater than 100, return n - 10
           if n > 100 {
              return n - 10
           }
           
           // Recursive case: call the function again, but with another
           // call inside it (nested recursion).
           return nestedRecursion(nestedRecursion(n + 11))
        }
        
        func main() {
           n := 64
           result := nestedRecursion(n)
           
           fmt.Println("Nested recursion for", n, "is", result)
           // Output: Nested recursion for 64 is 91
        }
        ```

## Wrapper Function
- A function that provides an additional layer of abstraction of functionality around an existing function or method.
- It acts as an intermediary between the caller and the wrapped function, allowing you to modify inputs, outputs, or behaviour **without directly modifying the original function**.
- Commonly used for various purposes, such as:
    - **Logging**
        - Can add logging statements before and after invoking the wrapped function.
        - This helps in capturing information about the function calls, input parameters, return values, and any errors that may occur.
    - **Timing and Profiling**
        - Can be used to measure the execution time of functions, enabling performance analysis and profiling.
        - By recording the start and end times, you can calculate the elapsed time and gather statistics.
    - **Authentication and Authorization**
        - Can handle authentication and authorization checks before executing the wrapped function.
    - **Error Handling**
        - Can intercept errors returned by the wrapped function and transform them into a different error type or add more contextual information.
- **Example**
    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    // TimedFunction acts as a wrapper function that measures the elapsed time
    // taken by 'MyFunction'. It captures the start time, calls the fn(),
    // calculates the elapsed time, and then prints it.
    func TimedFunction(fn func()) {
    	start := time.Now()
    	fn()
    	
    	elapsed := time.Since(start)
    	
    	fmt.Println("Elapsed time:", elapsed)
    }
    
    // Function to be wrapped
    func MyFunction() {
    	time.Sleep(2 * time.Second) // Simulate some work
    	fmt.Println("MyFunction completed")
    }
    
    func main() {
    	// Call the wrapped function
    	TimedFunction(MyFunction)
    }
    ```

## Defer
- It is used to ensure that a **function call is executed later in program's execution**, usually for cleanup (e.g., close the channel, catch the panics, etc.).
- **`defer`** is used to schedule a function call to be executed just before the function returns.
- **Example**
    ```go
    func main() {
    	// The defer statement schedules fmt.Println("world") to be executed at the
    	// very end of the main function.
    	defer fmt.Println("world")
    	
    	// This will be called immediately, and is printed first. After that,
    	// because we used defer, "world" is printed as the last step before main
    	// finishes.
    	fmt.Println("hello")
    	// OUTPUT
    	// hello
    	// world
    }
    ```
- **Arguments Evaluation**
    - The arguments of a deferred function are evaluated when the **`defer`** statement is executed and not when the actual function call is done.
    - **Example**
        ```go
        func display(a int) {
        	fmt.Println("value of 'a' in deferred function:", a)
        }
        
        func main() {
        	a := 5
        	
        	// Initially 'a' has a value of 5. When the defer statement is executed,
        	// the value of 'a' is 5 and hence this will be the argument to the
        	// display function which is deferred.
        	defer display(a)
        	
        	// We change the value of 'a' and the prints the value of 'a'.
        	a = 10
        	fmt.Println("before deferred function call:", a)
        	// OUTPUT
        	// before deferred function call: 10
        	// value of ‘a' in deferred function: 5
        }
        ```
- **Deferred Methods**
    - **`defer`** statement is not restricted only to functions. It is perfectly legal to defer a method call too.
    - **Example**
        ```go
        type person struct {
        	firstName string
        	lastName  string
        }
        
        func (p person) fullName() {
        	fmt.Printf("%s %s", p.firstName, p.lastName)
        }
        
        func main() {
        	p := person{
        		firstName: "John",
        		lastName:  "Smith",
        	}
        	defer p.fullName()
        	
        	fmt.Printf("Welcome ") // Output: Welcome John Smith
        }
        ```
- **Multiple `defer` Calls**
    - When a function has a multiple defer calls, they are pushed to a stack and **executed in LIFO (Last In, First Out) order**.
    - **Example**
        ```go
        func B() {
        	defer fmt.Print(1)
        	defer fmt.Print(2, " ")
        }
        
        func A() {
        	defer fmt.Print(3, " ")
        	defer fmt.Print(4, " ")
        }
        
        func main() {
        	// This will execute in LIFO order.
        	defer B()
        	defer A()
        }
        // Output: 4 3 2 1
        ```
- **Key Points**
    - When a function call is deferred, it is **placed onto a stack**. The function calls on the stack are executed when the surrounding function returns, not when the **`defer`** statement is called.
    - Multiple **`defer`** statements are allowed in the same program and they are **executed in LIFO (Last In, First Out) order**.

## 🔖 Extras
- [Defer](https://golangbot.com/defer/)
- [Go Functions](https://www.w3schools.com/go/go_functions.php)
- [Go - Recursion](https://www.tutorialspoint.com/go/go_recursion.htm)
- [Functions in Go](https://go101.org/article/function.html)
- [Methods in Golang](https://www.geeksforgeeks.org/methods-in-golang/)
- [Closures in Golang](https://www.geeksforgeeks.org/go-language/closures-in-golang/)
- [What is a Closure?](https://www.calhoun.io/what-is-a-closure/)
- [Variadic Functions in Go](https://www.geeksforgeeks.org/variadic-functions-in-go/)
- [Defer Keyword in Golang](https://www.geeksforgeeks.org/defer-keyword-in-golang/)
- [Ultimate Go Tour: Functions](https://tour.ardanlabs.com/tour/eng/functions/1)
- [Golang Defer: From Basic To Traps](https://victoriametrics.com/blog/defer-in-go/)
- [5 Useful Ways to Use Closures in Go](https://www.calhoun.io/5-useful-ways-to-use-closures-in-go/)
- [Function Declarations and Function Calls](https://go101.org/article/function-declarations-and-calls.html)
- [Understanding Closures in Go: A Beginner's Guide](https://medium.com/@sanhdoan/understanding-closures-in-go-a-beginners-guide-2795f6dae640)
- [Gotchas and Common Mistakes with Closures in Go](https://www.calhoun.io/gotchas-and-common-mistakes-with-closures-in-go/)