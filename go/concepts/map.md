# Map

- Essentially a reference to a **hash table**
- It is a data structure that provides support for storing and accessing data based on a key
- Uses a hash map and bucket system that maintains contiguous block of memory underneath
- A powerful and versatile data structure that acts as a collection of **unordered key-value pairs** and **does not allow duplicates**
- Are widely used due to their efficiency in providing fast lookups, updates, and deletions based on keys
- This reference type is inexpensive to pass — taking up only 8 bytes on a 64-bit machine and 4 bytes on a 32-bit machine
- The length of a map is the number of its elements and you can find it using the `len()` function
    ```go
    size := len(myMap)
    ```

## Components
- **Keys**
    - It must be unique and a comparable type (e.g., `int`, `float64`, `rune`, `string`, arrays, structs, or pointers)
    - Types like slices and non-comparable maps, arrays, functions or structs cannot be used as keys
- **Values**
    - Can be of any type, including another map, pointers, or even reference types

## Syntax and Declaration
- **Syntax**
    ```go
    map[KeyType]ValueType
    ```
    - Where:
        - `KeyType`: It must be unique and of a comparable type, such as `bool`, `int`, `float64`, `rune`, `string`, arrays, structs, interfaces or pointers
        - `ValueType`: Can be of any type, including another `map`, pointers, or even reference type
- **Declaration**
    - **Empty Map**
        - A map set to its zero value is not usable and will result in your program panicking if you try to set any value
            ```go
            // Syntax
            map[KeyType]ValueType{}
            
            type user struct{
            	name     string
            	username string
            }
            
            // Construct a map set to its zero value, that can store user values
            // based on a key of type string. Trying to set any value to this map
            // will result in a runtime error (panic).
            var users map[string]user
            
            // Attempting to write to a nil map will cause a runtime panic; DON'T
            // DO THAT.
            // panic: assignment to entry in nil map
            users["user_1"] = user{name: "John Doe", username: "john.doe"}
            ```

        - A structure that has a map as a field (technically zero as well)
            ```go
            type user struct{
            	name     string
            	contact  map[string]string
            }
            
            var u user
            fmt.Println(u.name)    // "" - emptry string; zero value for string
            fmt.Println(u.contact == nil) // true
            ```

	- **Map Literal**
        - Initializing a map with some data
            ```go
            // Creating and initializing a map with names and ages
            ages := map[string]int{
            	"A": 30,
            	"B": 25,
            	"C": 35,
            }
            
            // Retrieving
            fmt.Println("B's age:", ages["B"])
            ```

        - Initializing an empty map, which is functionality identical to using the `make` function
            ```go
            var ages = map[string]int{}
            ages["A"] = 30
            ```

    - **Using `make()` function**
        ```go
        // Syntax
        make(map[KeyValue]ValueType, Capacity)
        make(map[KeyValue]ValueType)
        
        // Creating a map using the make function
        ages := make(map[string]int)
        
        // Initializing the map with names and ages
        ages["A"] = 30
        ages["B"] = 25
        ages["C"] = 35
        ```
        
        - If the built-in `make` is used to construct a map, then the assignment operator can be used to add and update values in the map
        - The order of how keys/values are returned when ranging over a map is undefined by the spec and up to the compiler to implement

## Operations with Maps
### Adding Key-Value Pairs
- **Syntax**
    ```go
    yourMap[key] = value
    ```
    
- If the key **already exists**, its value **will be overwritten**
    ```go
    yourMap[3] = "three"       // Adds a new entry
    yourMap[1] = "Updated One" // Updates existing entry
    ```

### Retrieving Values
- You can retrieve a value from a map using its key
    ```go
    value := yourMap[key]
    ```
- If the **key does not exist**, it will **return the zero value** for the map's value type
    ```go
    value := yourMap[4] // "", empty stirng
    ```

### Comma, OK Idiom (Lookup)
- To perform a key lookup, square brackets (`[]`) are used with the map variable
- Two values can be returned from a map lookup, the **value** and a **boolean** that represents if the value was found or not
    ```go
    // Syntax
    value, ok := yourMap[key]
    
    // You can leave the '**ok**' variable out, if you don't need to know this
    value, _ := yourMap[key]
    ```
- When a **key is not found** in the map, the operation returns a value of the map type set to its zero value state
- **Example**
    ```go
    // If **ok** is true, the key is present; if false, it is not
    if value, ok := yourMap[2]; ok {
    	fmt.Println("Key exists:", value)
    	
    } else {
    	fmt.Println("Key does not exist")
    }
    ```

> [!NOTE]
> Don't use zero value to determine if a key exists in the map or not, since zero value may be valid and what was actually stored for the key

### Deleting a Key
- There is a built-in function named `delete` that allows for the deletion of data from the map based on a key
    ```go
    // Syntax
    delete(yourMap, key)
    
    // Removes the key 1 from the map
    delete(yourMap, 1)
    ```
    
- The `delete` function doesn't return anything, and will do nothing if the specified key doesn't exist

### Modifying Maps
- Since maps are reference types, assigning one map to another variable will not create a copy but will instead reference the same underlying data structure
    - **Modification to one map will affect the other**

### Iterating Over a Map
- Since maps are unordered, the **order of iteration may vary**.

#### `for...range` with a `map`
```go
// Using key and value
for key, value := range yourMap {
	fmt.Println(key, value)
}

// Omit key
for _, value := range yourMap {
	fmt.Println(value)
}

// Key only
for key := range yourMap {
	fmt.Println(key)
}
```

#### `for...range` with a slice
```go
slice := []int{42, 43, 44}
for index, value := range slice {
	fmt.Println(index, value)
}

// Omit index
for _, value := range slice {
	fmt.Println(value)
}

// Index only
for index := range slice {
	fmt.Println(index)
}
```

> [!NOTE]
> - [**Maps are not safe for concurrent use**](https://go.dev/doc/faq#atomic_maps)
> 	- It's not defined what happens when you read and write to them simultaneously
> 	- One common way to protect maps is with [sync.RWMutex](https://go.dev/pkg/sync/#RWMutex)
> 
> ```go
> // This statement declares a counter variable that is an anonymous struct
> // containing a map and an embedded sync.RWMutex.
> var counter = struct{
> 	sync.RWMutex
> 	m map[string]int
> }{m: make(map[string]int)}
> 
> // To read from the counter, take the read lock.
> counter.RLock()
> n := counter.m["some_key"]
> counter.RUnlock()
> 
> fmt.Println("some_key:", n)
> 
> // To write to the counter, take the write lock.
> counter.Lock()
> counter.m["some_key"]++
> counter.Unlock()
> ```

## Example
### Count Words

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"log"
	"os"
	"sort"
	"strings"
)

func main() {
	// Open a file
	file, err := os.Open("your-filename.txt")
	if err != nil {
		log.Fatalf("error: %s", err)
	}
	defer file.Close()

	// The frequency of words in the file
	words, err := frequency(file)
	if err != nil {
		log.Fatalf("error from frequency in main: %s", err)
	}

	// Sort the word frequencies
	pairs := sortWordFrequency(words)

	// Print the sorted results
	for _, pair := range pairs {
		fmt.Printf("%s \\t\\t %d\\n", pair.Key, pair.Value)
	}

	// Word with greatest frequency, and it's frequency
	word, count, err := maxWord(words)
	if err != nil {
		log.Fatalf("error with maxWord: %s\\n", err)
	}

	fmt.Printf("%#v has a frequency %d", word, count)
}

func frequency(r io.Reader) (map[string]int, error) {
	// Create a map to store word frequencies
	wordFreq := make(map[string]int)

	// Create a scanner to read the file
	scanner := bufio.NewScanner(r)
	scanner.Split((bufio.ScanWords))

	// Read each word and update the word frequencies
	for scanner.Scan() {
		word := strings.ToLower(scanner.Text())
		wordFreq[word]++
	}

	err := scanner.Err()
	if err != nil {
		return nil, err
	}

	return wordFreq, nil
}

// For sorting frequency of words
type Pair struct {
	Key   string
	Value int
}

// Implement the Len, Less, and Swap methods on PairList
// to satisfy the sort.Interface interface.
type PairList []Pair

func (p PairList) Len() int { return len(p) }

func (p PairList) Less(i, j int) bool {
	// Sort in descending order
	return p[i].Value > p[j].Value
}

func (p PairList) Swap(i, j int) {
	p[i], p[j] = p[j], p[i]
}

func sortWordFrequency(m map[string]int) PairList {
	// Convert the map to a pair list
	pairs := make(PairList, len(m))
	i := 0

	for key, value := range m {
		pairs[i] = Pair{key, value}
		i++
	}

	// Sort the pair list
	sort.Sort(pairs)

	return pairs
}

func maxWord(m map[string]int) (string, int, error) {
	if len(m) == 0 {
		return "", 0, fmt.Errorf("empty map")
	}

	maxWordFrequency := "" // Word with max frequency
	maxFrequency := 0      // Max frequency of that word

	for key, value := range m {
		if value > maxFrequency {
			maxWordFrequency = key
			maxFrequency = value
		}
	}

	return maxWordFrequency, maxFrequency, nil
}
```

## 🔖 Extras
- [Go Maps](https://www.w3schools.com/go/go_maps.php)
- [Golang Maps](https://www.geeksforgeeks.org/golang-maps/)
- [Go maps in action](https://go.dev/blog/maps)
- [Ardan Labs | Maps](https://tour.ardanlabs.com/tour/eng/maps/1)
- [GopherCon 2016: Inside the Map Implementation](https://www.youtube.com/watch?v=Tl7mi9QmLns)