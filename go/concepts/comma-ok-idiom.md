- Also known as the **comma ok pattern**
- In this, an operation might return an optional value and the second return value will be a boolean (ok) indicating whether the operation succeeded or not
    ```go
    value, ok := expression
    ```
    - `value` — outcome of the operation if it's successful
    - `ok` — indicates whether the action was successful (`true` or `false`)
    - `expression` — operation being performed, which typically involves lookup, type assertion, channel receive, or any function that might fail

```go
func offset(tz string) int {
	// If tz is present, seconds will be set appropriately
	if seconds, ok := timeZone[tz]; ok {
		return seconds
	}
	
	// seconds will be set to zero if not
	log.Println("unknown timezone:", tz)
	return 0
}
```

## Use Cases
### Map Key Lookup
- It allows you to check if the key exists in the map to retrieve the value
- It allows you differentiate between a key that doesn't exist and a key that exists with a zero value, thereby avoiding incorrect assumptions in your code

```go
value, ok := myMap[key]
if ok {
	fmt.Println("Value found:", value)
} else {
	fmt.Println("Key not found in map")
}
```

### Type Assertions
- This allows you to check if an `interface` value holds a specific type without causing panic if the assertion fails
```go
var input interface{} = "hello"

text, ok := input.(string)
if ok {
	fmt.Println("'input' is string:", text)
} else {
	fmt.Println("'input' is not a string")
}
```

### Reading from Channels
- Helps distinguish between a zero value received from an open channel and a zero value received because the channel is closed
```go
value, ok := <-ch
if !ok {
	fmt.Println("Channel is closed")
} else {
	fmt.Println("Received value:", value)
}
```

### Comma OK with Blank Identifier (`_`)
- Allows you to check for the existence of a key without assigning the value to a variable
- Particularly useful when the value is not needed but you still want to confirm the presence of the key

```go
if _, ok := myMap[key]; ok {
	fmt.Println("Key exists in map")
}
```

## 🔖 Extras
- [Comma OK in GO](https://dev.to/saurabh975/comma-ok-in-go-l4f)
- [Go Comma Ok & Packages](https://glitchyhitchy.medium.com/go-comma-ok-packages-49032fe0940e)
- [How the Comma Ok Idiom and Package System Work in Go](https://www.freecodecamp.org/news/how-the-comma-ok-idiom-and-package-system-work-in-go/)