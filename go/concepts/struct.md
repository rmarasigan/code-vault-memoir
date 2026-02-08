# `struct`

- A **composite data type** (aggregate data types / complex data types) that allow you to group together related data of different types into a single type
- Structures **can have methods** associated with them and the methods can be defined on structures **using the receiver syntax**
- The size of a zero-field struct type is zero
- The size of a struct is the sum of the sizes of all its field types plus the number of some padding bytes
    - Padding bytes are used to align the memory addresses of some files

## Syntax and Declaration
- **Syntax**
    
    ```go
    type StructName struct {
    	Field1 dataType
    	Field2 dataType
    	...
    }
    ```
    - Where:
        - **`type`**: Introduces a new type
        - **`StructName`**: The name of the type
        - **`struct`** (structure): It contains a list of various fields inside the curly braces
- **Declaration**
    ```go
    type Address struct {
    	Street  string
    	City    string
    	State   string
    	ZipCode int
    }
    ```
    - We can also make them compact by combining the various fields of the same type.
        ```go
        type Address struct {
        	Street, City, State string
        	ZipCode             int
        }
        ```
- To declare a structure:
    ```go
    var address Address
    ```
    - The `address` variable has a type `Address` which is by default set to zero. Zero means all fields are set to their corresponding zero value.
    - You can also initialize a variable of struct type using a struct literal.
        ```go
        // Create an address value with **no field names** present.
        // All fields must be present by the field declaration orders.
        var address = Address{"Storgatan 10A 12", "Stockholm", "Sweden", 11456}
        
        // Create an address with two fields specified.
        var address = Address{Street: "Högvaktsterrassen", City: "Stockholm"}
        
        // This is a zero-value and none of the fields are specified.
        var address = Address{}
        ```

> [!NOTE]
> - Always pass the field values in the same order in which they are declared in the struct.
> - Go also supports the `name: value` syntax for initializing a struct and the order of fields is irrelevant when using this syntax. This allows you to initialize only a subset of fields.

## Accessing Struct Fields
- To access individual fields of a struct you have to use dot (`.`) operator between the structure variable name and the structure member
    ```go
    type Address struct {
    	Street  string
    	City    string
    	State   string
    	ZipCode int
    }
    
    func main() {
    	// Create an address value with no field names present.
    	var address = Address{"Storgatan 10A 12", "Stockholm", "Sweden", 11456}
    
    	// Access and print address
    	fmt.Println("Street:", address.Street)
    	fmt.Println("City:", address.City)
    	fmt.Println("State:", address.State)
    	fmt.Println("ZipCode:", address.ZipCode)
    }
    ```

## Embedded Structs
- **Syntax**
    ```go
    type X struct {
    	field1 dataType
    	field2 dataType
    	...
    }
    
    type Y struct {
    	X
    	field3 dataType
    	field4 dataType
    	...
    }
    ```
- A way to reuse a struct's fields without having to inherit from it
- Allows you to include the fields and methods of an existing struct into a new struct
- **Example**
    ```go
    type Address struct {
    	Street  string
    	City    string
    	State   string
    	ZipCode int
    }
    
    // Method to return full address as a formatted string
    func (a Address) FullAddress() string {
    	return fmt.Sprintf("%s, %s %s %d", a.Street, a.City, a.State, a.ZipCode)
    }
    
    type Person struct {
    	Name    string
      Address //embedded struct
    }
    
    func main() {
    	person := Person{
    		Name: "John Doe",
    		Address: Address{
    			Street: "123 Main St",
    			City:   "New York",
    			State:  "NY",
    			ZipCode: 10001,
    		},
    	}
    
    	// Because the Address struct is included in the Person struct, we can
    	// access the person's city and state directly from the Person struct
    	// using dot notation
    	fmt.Println("Name:", person.Name)
    	fmt.Println("Street:", person.Street)
    	fmt.Println("City:", person.City)
    	fmt.Println("State:", person.State)
    	fmt.Println("ZipCode:", person.ZipCode)
    	
    	// Also access Address method directly
    	fmt.Println("Full Address:", person.FullAddress())
    }
    ```
- The main purpose of type embedding is to **extend the functionalities of the embedded types into the embedding type**, so that we don't need to re-implement the functionalities of the embedded types for the embedding type.
- This means:
    - When you embed a type (like a struct), you get its methods and fields on the outer type for free.
    - You can use these methods without having to write them again for the outer type.

## Anonymous Structure
- **Unnamed, temporary structures used for a one-time purpose**, while anonymous fields allow embedding fields without names
- Anonymous structs are great for writing [table driven tests](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests)
- **Syntax**
    ```go
    variable := struct {
    	field1 dataType1
    	field2 dataType2
    }{field1: value1, field2: value2}
    ```
- **Example**
    ```go
    response := struct {
    		Message string `json:"message"`
    		Status  int    `json:"status"`
    }{Message: "sucessfully processed", Status: http.StatusOK}
    ```
- To access the fields of an anonymous struct, the `.` operator can be used
- **Anonymous Array Struct**
    - **Example**
        ```go
        // The array of anonymous struct is created that has 3 elements. Each
        // element is an anonymous struct with the name, age and active fields.
        users := [3]struct {
        		name   string
        		age    int
        		active bool
        	}{
        		{
        			name:   "John",
        			age:    30,
        			active: true,
        		},
        		{
        			name:   "Mary",
        			age:    25,
        			active: true,
        		},
        		{
        			name:   "Peter",
        			age:    35,
        			active: false,
        		},
        }
        	
        fmt.Println(users[0].name)   // John
        fmt.Println(users[0].age)    // 30
        fmt.Println(users[0].active) // true
        ```
- **Anonymous Struct with a Method (Function)**
    - **Example**
        ```go
        // This defines an anonymous struct with a greet field of type func().
        user := struct {
        		name   string
        		age    int
        		active bool
        		greet  func()
        	}{
        		name:   "John",
        		age:    30,
        		active: true,
        }
        
        // This assigns a function that uses the user variable's fields directly.
        user.greet = func() {
        	fmt.Printf("Hello, my name is %s and I'm %d y/o.\\nAccount Active: %v", user.name, user.age, user.active)
        }
        
        // This works exactly like calling a method, though it's technically
        // just a function field.
        user.greet()
        
        // **OUTPUT**
        // Hello, my name is John and I'm 30 y/o.
        // Account Active: true
        ```
- **Nested Anonymous Struct**
    - **Example**
        ```go
        person := struct {
        		name    string
        		age     int
        		address struct {
        			street string
        			city   string
        		}
        	}{
        		name: "John",
        		age:  30,
        		address: struct {
        			street string
        			city   string
        		}{
        			street: "123 Main St",
        			city:   "New York",
        		},
        }
        
        // **OUTPUT**: John lives in 123 Main St, New York
        fmt.Printf("%s lives in %s, %s\\n", person.name, person.address.street, person.address.city)
        ```

## Composition
- It refers to a **way of structuring and building complex types** by combining multiple simpler types.
- One of the fundamental principles of object-oriented programming and allows you to create more flexible and reusable code.
- The end goal of composition is the same as that of inheritance, however, instead of inheriting features from a parent class/object, larger objects in this regard are composed of the other objects and can thereby use their functionality.
- **Example**
    ```go
    // We create an Engine struct to hold generic information
    type Engine struct {
    	// Engine fields
    }
    
    // Engine Method
    func (e *Ending) Start() {
    	fmt.Println("Engine started")
    }
    
    // The Car struct embeds the Engine struct, which means it includes all the
    // fields and methods of the Engine struct within itself.
    type Car struct {
    	// Embedding the Engine struct. We use composition through embedding to add
    	// the fields of the Engine struct.
    	Engine
    	// Car-specific fields
    }
    
    func main() {
    	// Defining a struct object of type Car
    	car := Car{}
    	
    	// Calling the Start() method form the embedded Engine struct
    	car.Start()
    }
    ```
- By using Composition, you can build complex types by combining simpler ones, promoting code reuse and modular design.
- Go has OOP aspects, but it's all about **TYPE**.
    - You create **types** and **values** of those types.
    - You assign them to **variables**.
    - You attach **methods** to types.
    - You **compose** types by embedding others.
    - You use **interfaces** to abstract behavior.
- Go is Object-Oriented (in its own way)
    - **Encapsulation**
        - State = Fields (`struct` fields)
        - Behavior = Methods (functions with receivers)
        - Visibility: Exported (Capitalized) vs. Unexported (Lowercase)
    - **Reusability**
        - Composition via embedded type (not classical inheritance)
    - **Polymorphism**
        - Interfaces (satisfied implicitly via method sets)
    - **Overriding / Behavior Composition**
        - Promotion — methods and fields of embedded types are promoted to the embedding type (can be "shadowed" or customized)
- **Traditional OOP**
    - **Classes**
        - Define a **blueprint** for objects.
        - You instantiate "objects" from the class.
        - Classes hold both:
            - State (fields, attributes)
            - Behavior (methods)
        - Visibility: Public, Private, Protected
    - **Inheritance**
        - Single or multiple inheritance of classes
        - Override base class methods in child classes
        - Use `super` or similar keywords to reference parent behavior

### Traditional OOP vs Go
| Concept         | Traditional OOP      | Go                             |
| --------------- | -------------------- | ------------------------------ |
| Class           | Core concept         | Not used (uses struct)         |
| Object          | Instance of a class  | Value of a user-defined type   |
| Inheritance     | Class hierarchy      | Embedded structs (composition) |
| Method Override | Subclass override    | Shadowing promoted methods     |
| Polymorphism    | Based on inheritance | Based on interfaces            |
| Encapsulation   | Access modifiers     | Capitalization (export rules)  |

- **Class**
	- **OOP**: A **`class` is a blueprint** that defines structure and behavior.
	- **Go**: It does not have a `class` keyword. You model similar behavior using a **`struct` + attached methods.**
- **Object**
	- **OOP**: You instantiate an object from a class (e.g., `new Person()`).
	- **Go**: You have to create a value of a `struct` type (e.g., `person := Person{name: "Alice"}`).
- **Inheritance**
	- **OOP**: Subclasses extend base classes and inherit methods/fields.
	- **Go**: No inheritance. Instead, you **embed one `struct` inside another** (composition), and **promote its fields/methods** into the outer type.
- **Method Override**
	- **OOP**: Subclass replaces a method from the parent class.
	- **Go**: You can "shadow" a promoted method from an embedded type by defining your own on the outer type. That acts like an override.
- **Polymorphism**
	- **OOP**: Based on inheritance — subclasses are used wherever parent types are expected.
	- **Go**: Based on **interfaces**, and it's **implicit**. If a type has the right method set, it satisfies the interface.
- **Encapsulation**
	- **OOP**: Done via `private`, `protected`, `public`.
	- **Go**: Done via capitalization.
		- **Capitalized = exported** (visible outside the package).
		- **lowercase = unexported** (package-private).

## Guidelines Around Declaring Types
- Declare types that represent something new or unique.
- Don't create aliases just for readability.
- Validate that a value of any type is created or used on its own.
- Embed types not because you need the state, but because we need the behaviour.
- If you are not thinking about behaviour, you're locking yourself into the design that you can't grow in the future without cascading code changes.
- Question types that are aliases or abstractions for an existing type.
- Question types whose sole purpose is to share a common set of states.

## Don't Design With Interfaces
As a developer, you exist in one of two modes: a **programmer** and then an **engineer**.

When you are programming, you are **focused on getting a piece of code to work**. Trying to solve the problem and break down walls. Prove that your initial ideas work. That is all you care about. This programming should be done in the concrete and is never production ready.

Once you have a prototype of code that solves the problem, you need to switch to **engineering** mode. You need to **focus on how to write the code at a micro-level for data semantics and readability, then at a macro-level for mental models and maintainability**. You also need to focus on errors and failure states.

Refactoring for readability, efficiency, abstraction, and for testability. Abstracting is only one of a few refactors that need to be performed. Don't apply abstractions unless they are absolutely necessary.

> [!TIP]
> If you don't understand the data, you don't understand the problem. If you don't understand the problem, you can't write any code.
> 
> **When is abstraction necessary?**
> When you see a place in the code where the data could change and you want to minimize the cascading code effects that would result. I might use abstraction to help make code testable, but you should try to avoid this if possible. **The best testable functions are functions that take raw data in and send raw data out**. It shouldn't matter where the data is coming from or going.

## Advantages and Disadvantages of using `struct`
- **Advantages**
    - **Encapsulation**
        - Allows you to encapsulate related data together, making it easier to manage and modify the data.
    - **Code Organization**
        - Helps organize the code in a logical way, which makes it easier to read and maintain.
    - **Flexibility**
        - Allow you to define custom types with their own behavior, making it easier to work with complex data.
    - **Type Safety**
        - Provide type safety by allowing you to define the type of each field, which helps to prevent errors caused by assigning the wrong type of value.
    - **Efficiency**
        - Very efficient, both in terms of memory usage and performance.
- **Disadvantages**
    - **Complexity**
        - Can make code more complex, especially if the structures have a large number of fields of methods.
    - **Boilerplate Code**
        - When defining large structures with many fields, it can be time-consuming to write out all of the field names and types.
    - **Inheritance**
        - Go **does not support** inheritance, which can make it more difficult to work with large hierarchies of related data.
    - **Immutability**
        - Go structures are mutable by default, which can make it more difficult to enforce immutability in your code.

## 🔖 Extras
- [Structs](https://golangbot.com/structs/)
- [Structs in Go](https://go101.org/article/struct.html)
- [Go Structures](https://www.w3schools.com/go/go_struct.php)
- [Type Embedding](https://go101.org/article/type-embedding.html)
- [Structures in Golang](https://www.geeksforgeeks.org/structures-in-golang/)
- [Grouping With Types](https://tour.ardanlabs.com/tour/eng/composition-grouping/1)
- [Composition in Golang](https://www.geeksforgeeks.org/composition-in-golang/)
- [Embedded Structures in Golang](https://www.geeksforgeeks.org/embedded-structures-in-golang/)
- [Is Go an object-oriented language?](https://go.dev/doc/faq#Is_Go_an_object-oriented_language)
- [Anonymous Structure and Field in Golang](https://www.geeksforgeeks.org/anonymous-structure-and-field-in-golang/)
- [What Are Golang's Anonymous Structs?](https://blog.boot.dev/golang/anonymous-structs-golang/)
- [10 things you (probably) don't know about Go](https://go.dev/talks/2012/10things.slide#1)