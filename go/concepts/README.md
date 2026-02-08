## What does Go look like?
```go
// This is our package declaration and all Go files must start with one of these.
// If you want to run the code directly, you'll need to name it main. If you
// don't name it main, then you can use it as a library and import it into other
// Go code.
package main

// Importing extra functionality from packages. These packages are all from Go's
// standard library. Go has a module system that makes using external packages
// easy. To use a new module, add it to your import path and Go will
// automatically dowload it for you.
import (
	"errors"
	"fmt"
	"strconv"
)

// Declaring a global variable, which is a list of strings, and initializing it
// with data. The text/string in Go support multi-byte UTF-8 encoding, making
// them safe for any language. The first key is always 0 in slices and arrays.
var helloList = []string{
	"Hello, world",
	"Καλημέρα κόσμε",
	"こんにちは世界",
	"‫مالس‬ ‫ایند‬‎",
	"Привет, мир",
}

// Declaring a function. A function acts as a unit of logic that's called when
// and as often as is needed. When calling a function, the code that calls it
// stops running and waits for the function to finish running. The code between
//  the {} is the body of the function.
func hello(index int) (string, error) {
	// The if statement runs the code inside {} if its Boolean expression true.
	if index < 0 || index > len(helloList)-1 {
		// Create an error, convert the int type to a string.
		return "", errors.New("out of range: " + strconv.Itoa(index))
	}

	// The nil represents something with no value and no type.
	return helloList[index], nil
}

// Declaring a function. A function is some code that runs when called. The main
// function is the entry point of your Go code. When your code runs, Go
// automatically calls main to get things started.
func main() {
	// Seed random number generator using the current time.
	rand.Seed(time.Now().UnixNano())

	// Generate a random number in the range of out list.
	// rand.Intn gives us a random number between 0 and 1, minutes the number
	// you pass in
	index := rand.Intn(len(helloList))

	// Call a function and receive multiple return values.
	msg, err := hello(index)
	if err != nil {
		// Writes out a logging message and kills the application. Once the app's
		// been killed, no more code runs.
		log.Fatal(err)
	}

	// Print our message to the console.
	fmt.Println(msg)
}
```

## Table of Contents
* [Fundamentals](fundamentals.md)
* [Variables](variables.md)
* [Understanding Scope](understanding-scope.md)
* [Comma OK Idiom](comma-ok-idiom.md)
* [Checksum](checksum.md)
* [Dependency Management](dependency-management.md)
* [Hash, Encryption, and Communication](hash-encryption-and-communication.md)
* [Control Flow](control-flow.md)
* [Conditional Statements](conditional-statements.md)
* [Logical Operators](logical-operators.md)
* [Statement; Statement Idiom](statement-idiom.md)
* [Switch Statement](switch-statement.md)
* [Select Statement](select-statement.md)
* [For Statement](for-statement.md)
* [Modulo or Remainder](modulo-or-remainder.md)
* [The init Function](the-init-function.md)
* [Arrays](arrays.md)
* [Slices](slices.md)
* [Map](map.md)
* [Struct](struct.md)
* [Functions](functions.md)
* [Methods](methods.md)
* [Interfaces and Polymorphism](interfaces-and-polymorphism.md)
* [Pointers](pointers.md)
* [Generics](generics.md)
* [Concrete Type vs Interface Type](concrete-type-vs-interface-type.md)
* [JSON](json.md)
* [Concurrency](concurrency.md)
* [Channels](channels.md)
* [Select](select.md)
* [Context](context.md)