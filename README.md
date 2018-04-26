# GothamGo 2018 Notes

[Link to Course](https://www.gopherguides.com/)

* More limitations with Go = Simpler
* Easy to read

## Syntax and Types

* string
    * immutable and can not be modified
    * `a := "Say hello to Go!"`
* bool

### Declaring Variables

**Without Initialization**
`var a int`

**With initialization, explicit type**
`var a int = 1`

**With initialization, implicit type**
`a := 1`

All builtin types have a zero value. Any allocated variable is usable even if it never has a value assigned.

### Iota
Go's `iota` identifier is used in const declarations to simplify definitions of incrementing numbers. Because it can be used in expressions, it provides a generality beyond that of simple enumerations.

**Input**
```go
package main

import "fmt"

const (
	Apple int = iota
	Orange
	Banana
)

func main() {
	fmt.Printf("The value of Apple is %v\n", Apple)
	fmt.Printf("The value of Orange is %v\n", Orange)
	fmt.Printf("The value of Banana is %v\n", Banana)
}
```

**Output**
```go
The value of Apple is 0
The value of Orange is 1
The value of Banana is 2
```

### Structs

A struct is a collection of fields, often called members (or attributes).

Structs are used so we can create our own custom complex types in Go.

When trying to understand structs, it helps to think of them as a blueprint for what your new type will do.

A struct definition, does NOT contain any data.

**Structs may have zero or more fields**
```go
type User struct {
	Name  string
	Email string
}
```

**Fields can be referenced using a period and the field name.**
```go
func main() {
	u := User{}
	u.Name
	u.Email
}
```

#### Initializing Structs

```go
u := User{
	Name:  "Homer Simpson",
	Email: "homer@example.com",
}
```
**You need a comma after values ^ ^**

You can set as many of the field values on a struct at initialization time as you want.
```go
u := User{Email: "marge@example.com"}
u.Name = "Marge Simpson"
```

## Arrays and Iteration

* Fixed length
* Fixed type
* Zero based

The capacity of an array is defined at creation time. Once an array is allocated it's size can no longer be changed.

### Defining an array

```go
names := [4]string{}
names[0] = "John"
names[1] = "Paul"
names[2] = "George"
names[3] = "Ringo"
```

### Initializing an array

```go
package main

import "fmt"

func main() {
	names := [4]string{"John", "Paul", "George", "Ringo"}

	fmt.Println(names)
}
```

## Array Type
The length is actually part of the type that is defined for arrays.

```go
package main

import "fmt"

func main() {
	a1 := [2]string{"one", "two"}
	a2 := [2]string{}

	a2 = a1

	fmt.Println(a2)
	a3 := [3]string{}

	// This can't be done, as it is not of the same type
	//a3 = a2

	fmt.Println(a3)
}
```
## 2-D Arrays (Matrix)

Go's arrays are one-dimensional. To create an equivalent of a 2D array, it is necessary to define an array-of-arrays.



```go
type Matrix [3][3]int

func main() {
	m := Matrix{
		{0, 0, 0},
		{1, 1, 1},
		{2, 2, 3},
    }
```

### The `for` Loop
Go only has one loop! Use it for `for`, `while`, `do while`, `do until`, etc...

```go
for i := 0; i < N; i++ {
  // do work until i equals N
}
```

```go
for {
  // loop forever
}
```

### Iterating over arrays with `len`

```go
package main

import "fmt"

func main() {
	names := [4]string{"John", "Paul", "George", "Ringo"}

	for i := 0; i < len(names); i++ {
		fmt.Println(names[i])
	}
}
```

**NOTE:** Explicit array length MUST be declared, it cannot infer any type data.

### `range` Keyword

Looping over arrays, and other collection types, is so common that Go created the range tool to simplify this code.

```go
package main

import "fmt"

func main() {
	names := [4]string{"John", "Paul", "George", "Ringo"}

	for i, n := range names {
		fmt.Printf("%d - %s\n", i, n)
	}
}
```

Range returns the the index and the value of the array.

## Slices

Slices wrap arrays in Go, and provide a more general, powerful, and convenient interface to data sequences.

* Similar to Arrays
* Fixed type
* Dynamically sized
* Flexible

Unless you know that your list will contain a finite and fixed set of elements, you'll almost always use a slice when dealing with data.

```go
package main

import "fmt"

func main() {
	// create an array of names
	namesArray := [4]string{"John", "Paul", "George", "Ringo"}
	// create a slice of names
	namesSlice := []string{"John", "Paul", "George", "Ringo"}

	fmt.Println(namesArray)
	fmt.Println(namesSlice)
}
```

Think of a slice as having three members:

* Length
* Capacity
* Pointer to the underlying array

```go
type slice struct {
	Length   int
	Capacity int
	Array    [10]array
}
```

### Appeding to slices

The `append` keyword allows us to add elements to a slice.

```go
package main

import "fmt"

func main() {
	names := []string{}
	names = append(names, "John")

	// Append multiple items at once
	names = append(names, "Sally", "George")

	// Append an entire slice to another slice
	moreNames := []string{"Bill", "Ginger", "Wilma"}
	names = append(names, moreNames...)

	fmt.Println(names)
}
```

## Len & Cap

* The `len` keyword tells us how many elements the slice actually has.

* The `cap` keyword tells us the capacity of the slice, or how many elements it can have.

```go
names := []string{}
fmt.Println("len:", len(names)) // 0
fmt.Println("cap:", cap(names)) // 0

names = append(names, "John")
fmt.Println("len:", len(names)) // 1
fmt.Println("cap:", cap(names)) // 1

names = append(names, "Paul")
fmt.Println("len:", len(names)) // 2
fmt.Println("cap:", cap(names)) // 2

names = append(names, "George")
fmt.Println("len:", len(names)) // 3
fmt.Println("cap:", cap(names)) // 4

names = append(names, "Ringo")
fmt.Println("len:", len(names)) // 4
fmt.Println("cap:", cap(names)) // 4

names = append(names, "Stu")
fmt.Println("len:", len(names)) // 5
fmt.Println("cap:", cap(names)) // 8
```

### Making a slice

Slices can also be created using the `make` keyword which allows us to define the starting "length" of the slice, and optionally, the starting "capacity" of the slice.


```go
package main

import "fmt"

func main() {
	a := make([]int, 1, 3)

	fmt.Println(a)      // [0]
	fmt.Println(len(a)) // 1
	fmt.Println(cap(a)) // 3
}
```

### 2-D Slices

* Go's slices are one-dimensional. To create an equivalent of a 2D slice, it is necessary to define an slice-of-slices.

* Because slices are variable-length, it is possible to have each inner slice be a different length.

```go
// a slice of byte slices
type Modules [][]byte

course := Modules{
	[]byte("Chapter One: Syntax"),
	[]byte("Chapter Two: Arrays and Slices"),
	[]byte("Chapter Three: Maps"),
}
```

### Slice Subsets

Subsets of a slice (or a slice of a slice) allow us to work with just section of a slice.

```go
package main

import "fmt"

// slice[starting_index : (starting_index + length)]

func main() {
	names := []string{"John", "Paul", "George", "Ringo"}

	fmt.Println(names)      // [John Paul George Ringo]
	fmt.Println(names[1:3]) // [Paul George] - names[1:1+2]

	// functionally equivalent
	fmt.Println(names[2:])           // [George Ringo]
	fmt.Println(names[2:len(names)]) // [George Ringo]

	// functionally equivalent
	fmt.Println(names[:2])  // [John Paul]
	fmt.Println(names[0:2]) // [John Paul]
}
```

### Mutating Slice Subsets

```go
names := []string{"John", "Paul", "George", "Ringo"}

fmt.Println(names) // [John Paul George Ringo]

guitars := names[:3]

fmt.Println(guitars) // [John Paul George]

for i, g := range guitars {
	guitars[i] = strings.ToUpper(g)
}

fmt.Println(names) // [JOHN PAUL GEORGE Ringo]
```

Let's not do this!

### Using `copy` to not mutate data

```go
package main

import "fmt"

func main() {
	veggies := []string{"carrot", "potato", "cucumber", "onion"}
	v := veggies

	v2 := make([]string, len(veggies))
	copy(v2, veggies)

	v[0] = "zuchinni"

	fmt.Println(veggies)
	fmt.Println(v)
	fmt.Println(v2)
}
```

```go
// copy(source, destination)
```

[more slice tricks + tips](https://github.com/golang/go/wiki/SliceTricks)

## Maps