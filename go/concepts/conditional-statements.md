## If Statements
- Used to control the flow of execution in a program based on a condition
    ```go
    if (condition) {
       // code to execute if the **condition is true**
    }
    ```
    - `condition` is an expression that evaluates to either `true` or `fase`
        - `true`: the code inside the curly braces will be executed
        - `false`: the code inside the curly braces will be skipped and execution will continue with the next statement following the `if` statement

```go
package main

import (
	"fmt"
)

func main() {
	x := 40
	y := 5
	z := 42
	
	if x < z {
		fmt.Println("x is less than z")
	} else if x == y {
		fmt.Println("x is equal to y")
	} else if x == z {
		fmt.Println("x is equal to z")
	} else {
		fmt.Println("x is greater than y and z")
	}
}
```

## Comparison Operators
- Compare two operands and yield an untyped boolean value
    - `==` equal
    - `!=` not equal
    - `<` less
    - `<=` less or equal
    - `>` greater
    - `>=` greater or equal