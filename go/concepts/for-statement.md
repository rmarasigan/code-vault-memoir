# `for` Statement

- A loop is used to execute a block of code repeatedly until a condition is satisfied
- Go doesn’t have `while` or `do while` loops which are present in other languages like C

#### Syntax
```go
for initialization; condition; post {
	// some code here ...
}
```

Where:
- **`initialization`**
    - It will be executed once
    - The loop starts with the keyword `for` and initializes the looping variable
- **`condition`**
    - After the loop is initialized, the condition is checked
    - If the condition evaluates to `true`, the body of the loop inside the `{}` will be executed followed by the `post` statement
- **`post`**
    - It will be executed after each successful iteration of the loop
    - After the `post` statement is executed, the condition will be rechecked
    - If it’s true, the loop will continue executing, else the `for` loop terminates

#### Example
```go
func main() {
	// **initialization**: i is initialized as 1.
	// **condition**: It will check whether i <= 10. If true, the value is printed.
	// Otherwise, the loop is terminated.
	// **post**: It increments i by 1 at the end of each iteration. Once i is greater
	// than 10, the loop terminates.
	for i := 1; i <= 10; i++ {
		fmt.Printf("%d ", i)
	}
}
```

> [!NOTE]
> The variables declared in a `for` loop are only available within the scope of the loop. Hence, `i` cannot be accessed outside the body of the for loop.

## Infinite Loop
- It will be running continuously and by simply removing the conditional clauses will make the loop an infinite one
- To stop the infinite execution after certain condition matches, we can use `break` to break out of the loop

```go
for {
	// this block executes infinitely
}
```

## Conditional `for` Loop
- This is similar to `while` loop
- Excluding the initialization and post condition
- It only checks the condition and runs if it matches

```go
i := 0
for i<5 {
	fmt.Println(i)
	i += 2
}
```

## Multiple Variable Declaration
- It is possible to declare and operate on multiple variables in a `for` loop

```go
// Multiple initialization and increment
for no, i := 10, 1; i <= 10 && no <= 19; i, no = i+1, no+1 {
	fmt.Printf("%d * %d = %d\\n", no, i, no*i)
}
```

**Output**
```go
10 * 1 = 10
11 * 2 = 22
12 * 3 = 36
13 * 4 = 52
14 * 5 = 70
15 * 6 = 90
16 * 7 = 112
17 * 8 = 136
18 * 9 = 162
19 * 10 = 190
```

## Range `for` Loop
- We can use `range` function to iterate over a slice or array
- It gives two things to work with, one is the current `index` and the other is the current `value`
- If the current value is the only thing needed, we can omit the index using the blank identifier (`_`)

```go
var items []int = []int{1, 2, 3, 4, 5}

for index, value := range items {
	fmt.Println(index, value)
}

// Omitting index
for _, value := range items {
	fmt.Println(value)
}
```

## Range `for` with `map`
- It will provide use the `key` and `value` both at the same time, with the `key` being the index
- It is a great for-construct which can substantially reduce code and make the code more concise

```go
var m map[int]string = map[int]string{
	1: "One",
	2: "Two",
	3: "Three",
}

for key, value := range m {
	fmt.Println(key, value)
}
```

## Break
- The `break` statement is used to **terminate the `for` loop abruptly** before it finishes its normal execution and move the control to the line of code just after the `for` loop

```go
for i := 1; i <= 10; i++ {
	if i > 5 {
		// The loop is terminated if i > 5
		break
	}
	
	fmt.Printf("%d ", i)
}
fmt.Printf("\\t|\\tloop ended")
```

## Continue
- The `continue` statement is used to **skip the current iteration** of the `for` loop
- All code present in a `for` loop after the `continue` statement will not be executed for the current iteration and the loop will move to the next iteration

```go
for i := 1; i <= 10; i++ {
	// Checks if the remainder of dividing i by 2 is 0. If it is zero, then the
	// number is even and continue statement is executed and the control moves to
	// the next iteration of the loop.
	if i%2 == 0 {
		continue
	}
	
	fmt.Printf("%d ", i)
}
```

## Labels
- Can be used to break the outer loop from inside the inner loop
- It is used in `break` and `continue` statement where it is optional but it is required in `goto` statement
- The scope of the label is the function where it is declared

```go
func main() {
outer:
	for i := 0; i < 3; i++ {
		for j := 1; j < 4; j++ {
			fmt.Printf("i = %d, j = %d\\n", i, j)
			if i == j {
				break outer
			}
		}
	}
}
```

**Output**
```go
i = 0 , j = 1
i = 0 , j = 2
i = 0 , j = 3
i = 1 , j = 1
```

## Nested Loop
- A type of control structure used in programming to repeat a set of instructions multiple times
- It consist of one loop inside another loop
    - The outer loop is executed first
    - In each iteration of the outer loop, the inner loop is executed completely
    - The inner loop executes its set of instructions for every iteration of the outer loop
- **Commonly used when working with multi-dimensional data structures** such as arrays or matrices
- Can be used for tasks that require performing a specific action for every combination of two or more variables

```go
for i := 0; i < 5; i++ {
	fmt.Println("--")
	for j := 0; j < 5; j++ {
		fmt.Printf("outer loop 5v \\t inner loop 5v\\n", i, j)
	}
}
```

**Output**
```go
--
outer loop 0 	 inner loop 0
outer loop 0 	 inner loop 1
outer loop 0 	 inner loop 2
outer loop 0 	 inner loop 3
outer loop 0 	 inner loop 4
--
outer loop 1 	 inner loop 0
outer loop 1 	 inner loop 1
outer loop 1 	 inner loop 2
outer loop 1 	 inner loop 3
outer loop 1 	 inner loop 4
--
outer loop 2 	 inner loop 0
outer loop 2 	 inner loop 1
outer loop 2 	 inner loop 2
outer loop 2 	 inner loop 3
outer loop 2 	 inner loop 4
--
outer loop 3 	 inner loop 0
outer loop 3 	 inner loop 1
outer loop 3 	 inner loop 2
outer loop 3 	 inner loop 3
outer loop 3 	 inner loop 4
--
outer loop 4 	 inner loop 0
outer loop 4 	 inner loop 1
outer loop 4 	 inner loop 2
outer loop 4 	 inner loop 3
outer loop 4 	 inner loop 4
```

## 🔖 Extras
- [Loops](https://golangbot.com/loops/)
- [Label Breaks In Go](https://www.ardanlabs.com/blog/2013/11/label-breaks-in-go.html)
- [The for-loop in Golang](https://golangdocs.com/for-loop-in-golang)
- [For Loops and More in Go](https://www.ardanlabs.com/blog/2024/03/for-loops-and-more-in-go.html)
- [Golang for loop (A Complete Guide)](https://www.kelche.co/blog/go/golang-for-loop/)