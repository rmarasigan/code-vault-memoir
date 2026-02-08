The scope of a declared identifier is the extent of a source text in which the identifier denotes the specified constant, type, variable, function, label, or package.

Go is lexically scoped using [^1]blocks:
- The scope of a predeclared/built-in identifier is the **universe block**
    - Built-in Identifiers:
        - Types: `bool`, `int32`, `int64`, `float64`, `error`, `string`, `rune`, `any`, `uint`, …
        - Zero Value: `nil`
        - Functions: `make`, `new`, `panic`, `append`, `recover`, `println`, `copy`, `delete`, …
        - Constants like `true`, `false` or `iota`
- The scope of the identifier of a package import is the **file block** containing the package import declaration
- The scope of an identifier denoting a constant, type, variable, or function (but not method) declared at the package level is the **package block**
- The scope of an identifier denoting a method receiver, function parameter, or result variable is the corresponding **function body (a local block)**
- The scope of the identifier of a constant or a variable declared inside a function body **begins at the end of the specification of the constant or variable** (or the end of the declaration for a short-declared variable) and **ends at the end of the innermost containing block**
- The scope of the identifier of a type or a type alias declared inside a function body **begins at the end of the identifier in the corresponding type specification** and **ends at the end of the innermost containing block**
- The scope of a label is the body of the innermost **function body block** containing the label declaration but excludes all the bodies of anonymous functions nested in the containing function
- The scope of an identifier denoting a type parameter of a function or declared by a method receiver **begins after the name of the function** and **ends at the end of the function body**
- The scope of an identifier denoting a type parameter of a type **begins after the name of the type** and **ends at the end of the specification of the type**

> [!NOTE]
> **Black identifiers have no scopes.**
> The blank identifier is represented by the underscore character (`_`). It serves as an anonymous placeholder instead of a regular (non-blank) identifier and has special meaning in declarations, as an operand, and in assignment statements.

## Scope Example
```go
package main

// The imported package "fmt" is in the "**File block**" scope of this file
import (
	"fmt"
	"mymodule/scope/furtherexplored"
)

// x is in "**Package block**" scope
var x = 42

func main() {
	// x can be accessed here
	fmt.Println(x)

	// x can be accessed here
	printMe()

	// y does not exist here yet so this won't work
	// fmt.Println(y)

	// y is in "**Block scope**"
	y := 43
	fmt.Println(y)

	p1 := person{
		"James",
	}
	p1.sayHello()

	// Variable "shadowing"
	x := 32
	fmt.Println(x)

	// **Package block** x is still the same
	printMe()

	furtherexplored.Fascinating()

	// Also good to know
	if z := 82; z > 50 {
		fmt.Println(z)
	}
	
	// You can't access z here
	// fmt.Println(z)
	
	// take a look at the "comma ok idiom"
	// <https://go.dev/doc/effective_go#maps>
}

func printMe() {
	// x can be accessed here
	fmt.Println(x)
}

// type person is in "**Package block**" scope
type person struct {
	first string
}

// the method sayHello which is attached to VALUES of TYPE person is in
// "**Package block**" scope
func (p person) sayHello() {
	fmt.Println("Hi, my name is", p.first)
}
```

---

```go
// furtherexplored.go
package furtherexplored

// This is "**Package block**" scope
// This is not exported because the identifier "planetSpeed" is NOT capitalized
var planetSpeed = 67000

// useit.go
package furtherexplored

import "fmt"

// This is also "**Package block**" scope
// This is exported because the identifier "Fasincating" is capitalized
func Fascinating() {
	fmt.Println("Planet speed:", planetSpeed)

	planetRotationSpeed := 1000
	fmt.Println("Planet spinning speed:", planetRotationSpeed)
}
```

## Package Scope
- Are declared at the package level and can be accessed by all files within that package
- Should start with a capital letter to be exported

```go
// mathfuncs.go
package mathfuncs

var Pi = 3.14159

// anotherfile.go
package mathfuncs

import "fmt"

func PrintPi() {
	fmt.Println("The value of Pi is:", Pi)
}
```

## File Scope
- Are declared within a single Go source file and can be accessed within that file

```go
package main

var x = 10
```

## Block Scope
- Are declared within a code block, such as a function or an `if` statement, and can only be accessed within that block

```go
package main

import "fmt"

func main() {
	if true {
		y := 20
		fmt.Println("y in block scope:", y)
	}
	
	// Error: undefined: y
	// fmt.Println("y outside block scope:", y)
}
```

## Public/Global Scope
- A concept where a variable or function name starts with an uppercase letter
- Public variables or functions are accessible from other packages

```go
package mypackage

var PublicVar = "This is a public variable"
```

## Shadowing
- Occurs when a variable declared within a certain scope (e.g. function or block) has the same name as a variable declared in an outer scope
    ```go
    package main
    
    import "fmt"
    
    // Global Variable
    var num = 10
    
    func main() {
    	// Expected to print 10
    	fmt.Println("Global num: ", num)
    	
    	if true {
    		// Shadowing the global variable 'num'
    		num := 5
    		
    		// In here, we're referring to the ***local*** 'num', not the ***global*** one
    		// Prints 5, not 10
    		fmt.Println("Local num: ", num)
    	}
    	
    	// Prints 10, not 5
    	fmt.Println("Global num after shadowing: ", num)
    }
    ```
    
- If an identifier is shadowed, its scope will exclude the scopes of its shadowing identifiers. Below is an example, where an `X` declared in a deeper block shadows the `X`s declared in the shallower blocks.
    ```go
    package main
    
    import "fmt"
    
    var p0, p1, p2, p3, p4, p5 *int
    
    // Global Variable
    var x = 9999 // x#0
    
    func main() {
    	// It stores the address of global **x**
    	p0 = &x
    
    	// This shadows the global x#0
    	// x#1
    	var x = 888
    	
    	// Stores address of x#1 (888)
    	p1 = &x
    	
    	// It shadows x#1 and initialy, x = 70
    	// x#2
    	for x := 70; x < 77; x++ {
    		// It captures the value of x#2
    		p2 = &x
    		
    		// This shadows x#2 (x#3 = x#2 - 70)
    		// x#3
    		x := x - 70
    		
    		// This stores the address of x#3
    		p3 = &x
    		
    		// This shadows x#3 (x#4 = x#3 - 3; x#4 > 0)
    		// x#4
    		if x := x - 3; x > 0 {
    			// It stores the address of x#4
    			p4 = &x
    			
    			// This shadows x#4 within this block (x#5 = -x#4)
    			// x#5
    			x := -x
    			
    			// It stores the address of x#5
    			p5 = &x
    		}
    	}
    
    	// p0 = 9999 | p1 = 888 | p2 = 77 | p3 = 6 | p4 = 3 | p5 = -3
    	fmt.Println(*p0, *p1, *p2, *p3, *p4, *p5)
    }
    ```
    

> [!IMPORTANT]
> Identifier shadowing is useful, but please **don't abuse it**.

## 🔖 Extras
- [Blank identifier](https://go.dev/ref/spec#Blank_identifier)
- [Code Blocks and Identifier Scopes](https://go101.org/article/blocks-and-scopes.html)
- [learn-to-code-go-version-03/031-scope/](https://github.com/GoesToEleven/learn-to-code-go-version-03/tree/main/031-scope)
- [Golang Quick Reference: Packages and Scopes](https://medium.com/@cndf.dev/golang-quick-reference-packages-and-scopes-5d9e449c4844)

[^1]: A possibly empty sequence of declarations and statements within matching brace brackets.