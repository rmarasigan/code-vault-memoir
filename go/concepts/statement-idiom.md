# Statement Idiom

- In an `if` statement, the expression may be preceded by a simple statement, which executes before the expression is evaluated
    ```go
    x := 40
    
    if y := 2 * rand.Intn(x); y >= x {
    	fmt.Printf("y is %v and that is GREATER THAN OR EQUAL x which is %v\\n", y, x)
    } else {
    	fmt.Printf("y is %v and that is LESS THAN x which is %v\\n", y, x)
    }
    ```
    
- This is also like the "[Comma OK Idiom](comma-ok-idiom.md)" and it follows a specific syntax:
    ```go
    value, ok := expression
    ```
    - `value` — outcome or output of the operation if it is successful
    - `ok` — indicates whether the action was success, that is `true` or `false`
    - `expression` — operation being performed, which typically involves lookup, type assertion, channel receive, or any function that might fail

## **Use Cases**
### Map Key Lookup
- When retrieving a value from a `map`, the Comma OK idiom allows you to check if the key exists in the map
- Allows you to differentiate between a key that doesn't exist and a key that exists with a zero value, thereby avoiding incorrect assumptions in your code

```go
value, ok := myMap[key]
if ok {
	fmt.Printf("Value found: %v\\n", value)
} else {
	fmt.Println("Key not found in map")
}
```

### Type Assertion
- You can use Comma OK idiom when working with interfaces to safely attempt type assertions
- Allows you to check if an interface value holds a specific type without causing panic if the assertion fails

```go
var i interface{} = "hello"

text, ok := i.(string)
if ok {
 fmt.Printf("'text' is a string: %s\\n", text)
} else {
 fmt.Println("'text' is not a string")
}
```

### Reading from Channels
- When reading from a channel, you can use the Comma OK idiom to check if the channel has been closed
- Helps distinguish between a zero value received from an open channel and a zero value received because the channel is closed

```go
value, ok := <-ch
if !ok {
 fmt.Println("Channel is closed")
} else {
 fmt.Printf("Received value: %v\\n", value)
}
```

### Comma OK with Blank Identifier
- You can use the blank identifier (`_`) when you only care about the boolean result of the Comma OK idiom and don't need to use the value itself
- Allows you to check for the existence of a key without assigning the value to a variable
- Particularly useful when the value is not needed but you still want to confirm the presence of the key

```go
if _, ok := myMap[key]; ok {
 fmt.Println("Key exists in map")
}
```

## 🔖 Extras
- [How the Comma Ok Idiom and Package System Work in Go](https://www.freecodecamp.org/news/how-the-comma-ok-idiom-and-package-system-work-in-go/)