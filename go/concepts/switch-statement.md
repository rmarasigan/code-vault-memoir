# `switch` Statement

- Allows a program to evaluate an expression or variable and then selectively execute different blocks of code based on the value of the expression or variable
- Provides a convenient way to write code that performs different actions based on a single variable or expression without having to use multiple `if`/`else` statements
- Typically consists of a single expression or variable followed by a series of `case` statements that specify the different values that the expression or variable can take and the corresponding blocks of code to execute in each case

#### Syntax
```go
switch expression {
	case condition_1:
		//... some code here
	case condition_2:
		//... some code here
	case condition_n:
		//... some code here
	default:
		//... some code here
}
```

#### Example
```go
x := 40

switch {
	case x < 42:
		fmt.Println("x is LESS THAN 42")
		
	case x == 42:
		fmt.Println("x is EQUAL TO 42")
		
	case x > 42:
		fmt.Println("x is GREATER THAN 42")
		
	default:
		fmt.Println("this is the default case for x")
}
```

> [!NOTE]
> - **Duplicate cases** with the same constant value are **not allowed**
> - The `default` case will be executed when none of the other cases match
> - It is possible to include multiple expressions in a `case` by separating them with comma

## Expressionless Switch
- If the expression is omitted, the `switch` is considered to be `switch true` and each of the `case` expression is evaluated for truth and the corresponding block of code is executed
- This type of switch can be considered as an alternative to multiple `if else` clauses

```go
hour := 15 // hour in 24 hour format

// Using switch to determine the work shift
switch {
case hour >= 6 && hour < 12:
	fmt.Println("It's the morning shift.")
	
case hour >= 12 && hour < 17:
	fmt.Println("It's the afternoon shift.")
	
case hour >= 17 && hour < 21:
	fmt.Println("It's the evening shift.")
	
case (hour >= 21 && hour <= 24) || (hour >= 0 && hour < 6):
	fmt.Println("It's the night shift.")

default:
	fmt.Println("Invalid hour.")
}
```

## Fallthrough
- The control comes out of the `switch` statement immediately after a case is executed
- `fallthrough` statement is used to transfer control to the first statement of the case that is present immediately after the case which has been executed
- `fallthrough` should be the last statement in a `case`, if it is present somewhere in the middle, the compiler will complain that ‘`fallthrough statement out of place`'
- Cannot be used in the last case of a `switch` since there are no more cases to fallthrough

```go
func number() int {
	num := 15 * 5
	return num
}

func main() {
	// num is not a constant
	switch num := number(); {
	case num < 50:
		fmt.Printf("%d is lesser than 50\\n", num)
		fallthrough
		
	case num < 100:
		fmt.Printf("%d is lesser than 100\\n", num)
		fallthrough
		
	case num < 200:
		fmt.Printf("%d is lesser than 200", num)
	}
}
```

The `case num 100` is true and evaluated, so the program will print `75 is lesser than 100`. The next statement is `fallthrough`. When `fallthrough` is encountered the control moves to the first statement of the next case and also prints `75 is lesser than 200`.

```go
75 is lesser than 100
75 is lesser than 200
```

The `fallthrough` will happen even when the case evaluates to false.

```go
// num is 25 which is less than 50
switch num := 25; { 
case num < 50:
	fmt.Printf("%d is lesser than 50\\n", num)
	fallthrough

// fallthrough will happen even though the case evaluates to false
case num > 100:
	fmt.Printf("%d is greater than 100\\n", num)		
}
```

## Breaking Switch
- `break` statement can be used to terminate a `switch` early before it completes

```go
switch num := -5 {
case num < 50:
	if num < 0 {
		break
	}
	
	fmt.Printf("%d is lesser than 50\\n", num)
	fallthrough
	
case num < 100:
	fmt.Printf("%d is lesser than 100\\n", num)
	fallthrough
	
case num < 200:
	fmt.Printf("%d is lesser than 200", num)
}
```

When it reaches the `if` statement, the condition is satisfied since `num < 0`. The `break` statement terminates the `switch` before it completes and the program doesn't print anything.

## Breaking the Outer `for` Loop
- It can be done by **labeling** the `for` loop and breaking the `for` loop using that label inside the `switch` statement
```go
func main() {
// An infinite for loop and a switch case
randloop:
	for {
		// A random number is generated between 0 and 99 (100 is not included)
		// using the Intn function
		switch i := rand.Intn(100); {
		// If the generated number is even, the loop is broken using the label
		case i%2 == 0:
			fmt.Printf("Generated even number %d", i)
			break randloop
		}
	}
}
```

If the `break` statement is used without the label, the `switch` statement will only be broken and the loop will continue running. Labeling the loop and using it in the `break` statement inside the `switch` is necessary to break the outer `for` loop.

## 🔖 Extras
- [Go Wiki: Switch](https://go.dev/wiki/Switch)
- [Switch Statement](https://golangbot.com/switch/)
- [How To Write Switch Statements in Go](https://www.digitalocean.com/community/tutorials/how-to-write-switch-statements-in-go)