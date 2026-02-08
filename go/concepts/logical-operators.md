# Logical Operators

- Apply to boolean values and yield a result of the same type as the operands
    - `&&` AND
        - `true && true` = `true`
        - `true && false` = `false`
    - `||` OR
        - `true || true` = `true`
        - `true || false` = `true`
    - `!` NOT
        - `!true` = `false`
- Left operand is evaluated and then the right if the condition requires it

```go
package main

import "fmt"

func main() {
	x := 40
	y := 5

	if x < 42 && y < 42 {
		// && requries both to be true to run
		fmt.Println("both are less than the meaning of life")
	}

	if x > 30 || x < 42 {
		// || requires one of them to be true to run
		fmt.Println("x is getting close to the meaning of life")
	}

	if x != 42 && y != 10 {
		fmt.Println("x is not 42 & y is not 10")
	}
}
```