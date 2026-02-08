# Interfaces and Polymorphism

### Polymorphism

- In Object-Oriented Programming (OOP), **an object can behave like another object**. Polymorphism is a property that is available to many OO-languages.
- Polymorphism **allows objects of different types to be treated as objects of a common superclass**, providing a unified interface for multiple concrete implementations.
- In Go, it is achieved with the help of interfaces. A type implementing a function defined in interface becomes the type defined as an interface.
- It is **used to reduce code** in general. There will be less coupling if polymorphism is used. A single function can be used to do the same thing on multiple different objects.

### What is an interface?

- It is kind of like a definition. It **defines and describes** the exact methods that some other type must have.
- A **collection of method signatures**. It defines a contract, specifying what method types a type must have to be considered as implementing the interface.
- **Why are they useful?**
    - To help reduce duplication or boilerplate code.
    - To make it easier to use mocks instead of real objects in unit tests.
    - As an architectural too, to help enforce decoupling between parts of your code base.
- **Empty Interface**
    - It essentially **describe no methods** and has **no rules**. Because of that, it follows that any and every object satisfies the empty interface.
    - In a more plain-English way, it is kind of like a wildcard. Wherever you see it in a declaration (such as a variable, function parameter or struct field) you can use an object of any type.
    - As a general rule it is clearer, safer and more performant to use concrete types — or non-empty interface types — instead. With that said, the empty interface is useful in situations where you need to accept and work with unpredictable or user-defined types.

#### Benefits of Using Interfaces
- **Code Re-usability**
    - By defining behaviour through interfaces, we can write functions that work with multiple types.
- **Easy Extensibility**
    - Interfaces allow us to extend existing code without modifying it. If we have a function that works with a specific interface, we can create new types that implement the same interface and seamlessly use them with the existing function.
- **Mocking and Testing**
    - By defining interfaces for dependencies, we can create mock implementations to simulate behavior during testing. This isolates the code under test from its dependencies, making it easier to test each component in isolation.
- **Type Assertions and Type Switches**
    - **Type Assertions**
        - Type assertion allow us to extract the concrete value from an interface. It has two forms:
            - Returning a value.
            - Returning a value and a boolean, indicating whether the assertion was successful.
                ```go
                val, ok := input.(string)
                if !ok {
                	// your code here
                } else {
                	// your code here
                }
                ```
    - **Type Switches**
        - Type switches offer a concise way to check the underlying type of an interface against multiple possibilities.
        - **Example**
            ```go
            switch v := val.(type) {
            	case int:
            		// your code here
            	case string:
            		// your code here
            	default:
            		// your code here
            }
            ```

#### Best Practices
- **Keep Interfaces Small**
    - Define interfaces with a few methods. Large interfaces can lead to unnecessary coupling and make it challenging to implement.
- **Name Interfaces Descriptively**
    - Choose descriptive and precise names for interfaces to clearly communicate their purpose and role.
- **Provide Meaningful Method Names**
    - Use method names that convey their purpose and behavior, making the code more self-explanatory.
- **Favor Multiple Interfaces Over Large Ones**
    - Prefer composing smaller interfaces to achieve the required functionality.
- **Document Interfaces**
    - Write clear documentation for your interfaces, detailing their purpose and expected behavior. Well-documented interfaces are essential for their effective use.

### Implementing Interfaces
```go
// It contains two methods calculate() and source().
type Income interface {
	// calculate() calculates and returns the income from the source.
	calculate() int
	
	// source() returns the name of the source.
	source() string
}

type FixedBilling struct {
	// projectName represents the name of the project.
	projectName string
	
	// biddedAmount is the amount that the organization has bid for the project.
	biddedAmount int
}

// The income is just the amount bid for the project.
func (fb FixedBilling) calculate() int {
	return fb.biddedAmount
}

func (fb FixedBilling) source() string {
	return fb.projectName
}

type TimeAndMaterial struct {
	projectName string
	noOfHours  int
	hourlyRate int
}

// The income is the product of the noOfHours and hourlyRate.
func (tm TimeAndMaterial) calculate() int {
	return tm.noOfHours * tm.hourlyRate
}

func (tm TimeAndMaterial) source() string {
	return tm.projectName
}

// It accepts a slice of Income interfaces as argument and calculates the total
// income by iterating over the slice and calling calculate() method on each of
// its items.
func calculateNetIncome(ic []Income) {
	var netincome int = 0
	
	for _, income := range ic {
		fmt.Printf("Income From %s = $%d\\n", income.source(), income.calculate())
		netincome += income.calculate()
	}
	
	fmt.Printf("Net income of organization = $%d", netincome)
}

func main() {
	project1 := FixedBilling{projectName: "Project 1", biddedAmount: 5000}
	project2 := FixedBilling{projectName: "Project 2", biddedAmount: 10000}
	project3 := TimeAndMaterial{projectName: "Project 3", noOfHours: 160, hourlyRate: 25}
	incomeStreams := []Income{project1, project2, project3}
	
	calculateNetIncome(incomeStreams)
}
```

## `any` Identifier
- An alias for the empty interface (`interface{}`). The `any` identifier is straight-up syntactic sugar — using it in your code is equivalent in all ways to using `interface{}` — it means exactly the same thing and has exactly the same behavior.

## 🔖 Extras
- [Golang Interfaces Explained](https://www.alexedwards.net/blog/interfaces-explained)
- [Polymorphism in GoLang](https://www.geeksforgeeks.org/go-language/polymorphism-in-golang/)
- [Polymorphism - OOP in Go](https://golangbot.com/polymorphism/)
- [Understanding Go's Interface and Polymorphism: Writing Flexible Code](https://clouddevs.com/go/interface-and-polymorphism/)