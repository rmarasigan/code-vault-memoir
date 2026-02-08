# JSON

- JSON = JavaScript Object Notation.
- It is a **key-value** pair in JS, and it is easier to transmit than XML.
- A human-readable, machine transferable and generally the preferred way to send and receive data via REST APIs.
- It’s not the most efficient way, but it is the web developer's preferred way.

## Marshaling
- A **process of transforming the memory representation of an object to a data format** suitable for storage or transmission.
- It is typically used when data must be moved between different parts of a computer program or from one program to another.
- In Go, it is a way of saying "encode or convert to a JSON object".
- Golang is a strictly typed language, and JSON is a dynamically typed language.
- **`func Marshal(v interface{}) ([]byte, error)`**
    - It takes a `v interface{}`, which can be ‘any’ type.
        - **`interface{}`**
            - A way of defining a variable in Go that is "this could be anything".
            - At runtime, Go will then allocate the appropriate memory to fit whatever you decide to store in it.
    - Everything from a `struct` to a primitive, it will attempt to convert it to a JSON object.
    - It returns two things:
        - A slice of byte (`[]byte`), containing the literal string that is the JSON object.
        - An error, letting you know if anything went wrong.
    - Typically, you give the JSON marshal function a pre-filled `struct` or a raw `string` literal formatted to JSON.
    - Only the data that can be represented as JSON will be encoded or converted by the `json.Marshal()` function.
    - Only the exported or public fields of a `struct` will be present in the JSON output. A field with a _JSON tag_ is stored with its tag name instead of its variable name.
    - Pointers will be encoded as the values they point to, or `null` if the pointer is `nil`.

### Exposed vs Not Exposed Fields
- It is important to remember that **only fields with a capital first letter are visible** to external programs or packages.
- If you don’t capitalise the first letter, it won’t get exposed when you convert it to JSON using the `json.Marshal()` function.

## Unmarshal
- When we want to **convert a JSON Object into a Go struct**, we use the `json.Unmarshal()`.
- Unmarshal is a way of saying, "parse this JSON object into a valid Go struct".
- **`func Unmarshal(data []byte, v interface{}) error`**
    - It takes the following parameters:
        - A slice of bytes (`[]byte`) — a raw string, the JSON object you want to parse.
        - A **pointer** to a struct to parse JSON into.
    - It returns an error if anything went wrong with parsing.

### How does `Unmarshal` decide which fields to try and parse?
- For any **key found** in the JSON, `Unmarshal` will try to match it to a key found in the struct with the following logic:
    - It will first look for an exported (field member with a capital letter) with a tag `json:"FieldName"` (for explanation's sake, we use `"FieldName"`).
    - An exported (field member with a capital letter) with the name `FieldName`.
    - Any exported field name that matches the field name if case sensitivity is not an issue, e.g., `fIeLdNaMe`, `FIELDNAME`, `fieldname`.
- Only fields found in the destination type (`struct`) will be decoded, meaning if there is a field in the JSON that isn’t in the destination, it will be ignored.

## Trade-offs of using JSON
- Formats that are easy for humans to read are typically slow for computers to parse.
- A JSON number literal, such as ‘`123.45`’, has to be decoded in two iterative steps:
    - Read each byte to inspect if it is a number or a dot.
    - If a non-number is read, then we are done scanning the number literal.
    - Convert the base-10 number literal into a base-2 format, such as `int64` or IEEE-754 floating point number representation.

## When to use JSON?
- Use when **ease-of-use is the primary goal** of data interchange and **performance is a low priority**.
- JSON is human-readable, and it is easy to debug if something breaks.
- In many applications, encoding/decoding performance is a low priority because it can easily be scaled horizontally.

## Encoding Streams
- [`json.Encoder`](https://golang.org/pkg/encoding/json/#Encoder)
    - It will encode the value to an `io.[**Writer**](<https://golang.org/pkg/io/#Writer>)`.
    ```go
    type Encoder struct {}
    func NewEncoder(w io.Writer) *Encoder
    func (enc *Encoder) Encode(v interface{}) error
    ```
    - **Type Inspection**
        - First step is to look up the value’s type encoder. The types are inspected using Go’s [reflect package](https://golang.org/pkg/reflect/), and the [json package](https://golang.org/pkg/encoding/json/) holds an internal mapping of these `reflect.Type` values.
- `json.Marshal`
    - It will return an in-memory byte slice of your encoded value.
    ```go
    func Marshal(v interface{}) ([]byte, error)
    ```
    

## Encoder Compilation
- For types that are not built-in, an encoder is built on the fly and then cached for reuse.
- First, the encoder will check if the type implements `json.Marshaler`. If it does, then the marshalling is deferred to the type
    ```go
    type Marshaler interface {
    	MarshalJSON() ([]byte, error)
    }
    ```
- Next, the encoder checks if a type implements `encoding.TextMarshaler`. If it does, then it will generate a value from that function and then encode the result as a JSON string.
    ```go
    type TextMarshaler interface {
    	MarshalText() (text []byte, err error)
    }
    ```
- If neither interface is implemented, then it will recursively build an encoder based on the primitive encoders.

## Per-field Options
- Tags are the backticked strings you sometimes see at the end of structs.
- **Example**
    ```go
    type User struct {
    	Name    string `json:"name"`
    	Age     int    `json:"age,omitempty"`
     	Zipcode int    `json:"zipcode,string"`
    }
    ```
- These options include:
    - Renaming the field’s key.
    - The `omitempty` flag can be set, which will remove any non-struct fields that have an empty value.
    - The `string` flag can be used to force a field to encode as a `string`.

## Decoding Streams
- [`json.Decoder`](https://golang.org/pkg/encoding/json/#Decoder)
    - It allows you to decode from an [`io.Reader`](https://golang.org/pkg/io/#Reader).
    ```go
    type Decoder struct {}
    func NewDecoder(r io.Reader) *Decoder
    func (dec *Decoder) Decode(v interface{}) error
    ```
- `json.Unmarshal`
    - It decodes from a byte slice (`[]byte`).
    ```go
    func Unmarshal(data []byte, v interface{}) error
    ```
- These decoders work in two parts:
    - A scanner tokenises the input bytes.
    - A decode state converts the tokens to Go objects.

## Scanning JSON
- Scanner is an internal state machine used for parsing JSON.
- It operates in several steps:
    - It checks the first byte of a value to determine the type of token to parse.
        - If it is "**`{`**", it needs to **parse an object**.
        - If it is "**`[`**", it needs to **parse an array**.
    - Double quotes indicate the start of a `string`, a `t` or an `f` indicate the start of a boolean value, and 0-9 indicate the beginning of a number.
- For complex objects, a stack is used to keep track of closing braces.

## Custom Unmarshaling
- The decoder will first check if a type implements [`json.Unmarshaler`](https://golang.org/pkg/encoding/json/#Unmarshaler).
    ```go
    type Unmarshaler interface {
    	UnmarshalJSON([]byte) error
    }
    ```
    
    - This can be useful if you want to write your own optimisation implementation.
- Next, the decoder checks if the value type implements [`encoding.TextUnmarshaler`](https://golang.org/pkg/encoding/#TextUnmarshaler).
    ```go
    type TextUnmarshaler interface {
    	UnmarshalText(text []byte) error
    }
    ```
    
    - This is useful if you have a string representation of your type that you want to use.

## Pretty Printing
- JSON is typically written out as a single, long set of bytes with no extra whitespace.
- You can set indentation in two ways:
    - If you have an in-memory JSON-encoded byte slice, then you can pass it to the `json.Indent()`.
        ```go
        func Indent(dst *bytes.Buffer, src []byte, prefix, indent string) error
        ```
        - `prefix`: Specifies a character to write out on every line.
        - `indent`: Specifies the characters used for indentation.
    - There’s a helper function called `json.MarshalIndent()`, which literally just calls `json.Marshal()` and then `json.Indent()`.
- If you are using the stream-based `json.Encoder`, then you can set the indentation on it using the [`SetIndent()`](https://golang.org/pkg/encoding/json/#Encoder.SetIndent) method.
- The reverse of the indent function is the [`Compact()`](https://golang.org/pkg/encoding/json/#Compact). This will rewrite `src` to a destination buffer but remove all extraneous whitespace.

## 🔖 Extras
- [JSON and Marshaling](https://docs.google.com/document/d/1gqTe1ouyvGXKaD1kBBN9hNbTaDBSGZ6Ynt3GBFUbtM0/edit?tab=t.0)
- [Handwritten Parsers & Lexers in Go](https://blog.gopheracademy.com/advent-2014/parsers-lexers/)
- [Go Walkthrough: encoding/json package](https://medium.com/go-walkthrough/go-walkthrough-encoding-json-package-9681d1d37a8f)