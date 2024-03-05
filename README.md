[Full Markdown Example](https://gist.github.com/allysonsilva/85fff14a22bbdf55485be947566cc09e)
</br>
[Markdown ICONS](https://gist.github.com/rxaviers/7360908)


# Table of contents
1. [CREATIONAL DESIGN PATTERNS](#CREATIONAL_DESIGN_PATTERNS)
	1. [BUILDER](#builder2)
		1.  [Builder](#builder2)
		2.  [Builder Facets](#builder_facets)
		3.  [Builder Parameter](#builder_parameter)
		4.  [Functional Builder](#functional_builder)
		
	2. [Factories](#factories)
		1. [Factory Function](#factory_function)
		2. [Interface Factory](#interface_factory)
		3. [Factory Generator](#factory_generator)
		4. [Prototype Factory](#prototype_factory)

	3. [PROTOTYPE](#prototype)
		1. [Deep Copying](#deep_copying)
		2. [Copy Method](#copy_method)
		3. [Copy Through Serialization](#copy_serialization)
		4. [Prototype Factory](#proprototype_factory)

	4. [Singelton](#singelton)
		1. [Singelton](#singelton2)
		2. [Problems with Singleton](#problems_singleton)
		3. [Singleton and Dependency Inversion](#singleton_inversion)
2. [STRUCTURAL DESIGN PATTERNS](#structural_design_patterns)
	1. [BRIDGE](#BRIDGE)
   2. [COMPOSITE](#COMPOSITE)
      1. [Geometric Shapes](#geometric_shapes)
      2. [Neural Networks](#neural_networks)
   3. [DECORATOR](#DECORATOR)
      1. [Multiple Aggregation](#multiple_aggregation)
      2. [Decorator](#Decorator2)
   4. [FACADE](#FACADE)
   5. [FLYWEIGHT](#FLYWEIGHT)
      1. [Text Formatting](#Text_Formatting)
      2. [User Names](#User_Names)
   6. [PROXY](#PROXY)
      1. [Protection Proxy](#Protection_Proxy)
      2. [Virtual Proxy](#Virtual_Proxy)
      3. [Proxy vs Decorator](#Proxy_vs_Decorator)
3. [BEHAVIORAL DESIGN PATTERN](#BEHAVIORAL_DESIGN_PATTERN)
   1. [CHAIN OF RESPONSABILITY](#CHAIN_OF_RESPONSABILITY)
      1. [Command Query Separation](#Command_Query_Separation)
      2. [Broker Chain](#Broker_Chain)
   2. [COMMAND](#COMMAND)
      1. [Command](#Command)
      2. [Undo Operations](#Undo_Operations)
      3. [Composite Command](#Composite_Command)
      4. [Functional Command](#Functional_Command)
   3. [INTERPRETER](#INTERPRETER)
      1. [Lexing & Parsing](#Lexing)
   4. [ITERATOR](#ITERATOR)
      1. [Iteration](#Iteration)
      2. [Tree Traversal](#Tree_Traversal)
   5. [MEDIATOR](#MEDIATOR)
      1. [Chat Room](#Chat_Room)
   6. [MEMENTO](#MEMENTO)
      1. [Memento](#Memento)
      2. [Undo and Redo](#Undo_and_Redo)
   7. [OBSERVER](#OBSERVER)
      1. [Observer and Observable](#Observer_and_Observable)
      2. [Property Observers](#Property_Observers)
      3. [Property Dependencies](#Property_Dependencies)
   8. [STATE](#STATE)
      1. [Classic Implementation](#Classic_Implementation)
      2. [Handmade State Machine](#Handmade_State_Machine)
      3. [Switch-Based State Machine](#Switch-Based_State_Machine)
   9. [STRATEGY](#STRATEGY)
      1. [Strategy](#Strategy)
   10. [TEMPLATE METHOD](#TEMPLATE_METHOD)
       1. [Template Method](#Template_Method)
       2. [Functional Template Method](#Functional_Template_Method)
   11. [VISITOR](#VISITOR)
       1. [Intrusive Visitor](#Intrusive_Visitor)
       2. [Reflective Visitor](#Reflective_Visitor)
       3. [Classic Visitor](#Classic_Visitor)
       

      
      
   
      
      
      
            
      


   
   
  
 </br>   
 </br>   
 </br>   
 </br>   
      
		
	


# GO DESIGN PATTERN
<style>
pre {
  max-height: 300px;
  overflow-y: auto;
}

pre[class] {
  max-height: 100px;
}

.boxBorder {
     border: 2px solid blue;
     padding: 10px;
     outline: blue solid 5px;
     outline-offset: 5px;
   }



/* Style the button */
.top-link {
  transition:       all .25s ease-in-out;
  position:         fixed;
  bottom:           0;
  right:            0;
  display:          inline-flex;
  color:            #000000;

  cursor:           pointer;
  align-items:      center;
  justify-content:  center;
  margin:           0 2em 2em 0;
  border-radius:    50%;
  padding:          .25em;
  width:            1em;
  height:           1em;
  background-color: #F8F8F8;
}

</style>



<a class="top-link hide" href="#top">↑</a>
<a name="top"></a>

# SOLID Design Principles

>## Single Responsibility Principle
- Separation of Concern
```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/url"
	"strings"
)

var entryCount = 0
type Journal struct {
	entries []string
}

func (j *Journal) String() string {
	return strings.Join(j.entries, "\n")
}

func (j *Journal) AddEntry(text string) int {
	entryCount++
	entry := fmt.Sprintf("%d: %s",
		entryCount,
		text)
	j.entries = append(j.entries, entry)
	return entryCount
}

func (j *Journal) RemoveEntry(index int) {
	// ...
}



// breaks srp

func (j *Journal) Save(filename string) {
	_ = ioutil.WriteFile(filename,
		[]byte(j.String()), 0644)
}

func (j *Journal) Load(filename string) {

}

func (j *Journal) LoadFromWeb(url *url.URL) {

}

var lineSeparator = "\n"
func SaveToFile(j *Journal, filename string) {
	_ = ioutil.WriteFile(filename,
		[]byte(strings.Join(j.entries, lineSeparator)), 0644)
}

type Persistence struct {
	lineSeparator string
}

func (p *Persistence) saveToFile(j *Journal, filename string) {
	_ = ioutil.WriteFile(filename,
		[]byte(strings.Join(j.entries, p.lineSeparator)), 0644)
}


func main_() {
	j := Journal{}
	j.AddEntry("I cried today.")
	j.AddEntry("I ate a bug")
	fmt.Println(strings.Join(j.entries, "\n"))

	// separate function
	SaveToFile(&j, "journal.txt")

	//
	p := Persistence{"\n"}
	p.saveToFile(&j, "journal.txt")
}
```

>## Open-Closed Principle

```go
package main

import "fmt"

// combination of OCP and Repository demo

type Color int

const (
	red Color = iota
	green
	blue
)

type Size int

const (
	small Size = iota
	medium
	large
)

type Product struct {
	name string
	color Color
	size Size
}

type Filter struct {

}

func (f *Filter) filterByColor(
	products []Product, color Color)[]*Product {
	result := make([]*Product, 0)

	for i, v := range products {
		if v.color == color {
			result = append(result, &products[i])
		}
	}

	return result
}

func (f *Filter) filterBySize(
	products []Product, size Size) []*Product {
	result := make([]*Product, 0)

	for i, v := range products {
		if v.size == size {
			result = append(result, &products[i])
		}
	}

	return result
}

func (f *Filter) filterBySizeAndColor(
	products []Product, size Size,
	color Color)[]*Product {
	result := make([]*Product, 0)

	for i, v := range products {
		if v.size == size && v.color == color {
			result = append(result, &products[i])
		}
	}

	return result
}

// filterBySize, filterBySizeAndColor

type Specification interface {
	IsSatisfied(p *Product) bool
}

type ColorSpecification struct {
	color Color
}

func (spec ColorSpecification) IsSatisfied(p *Product) bool {
	return p.color == spec.color
}

type SizeSpecification struct {
	size Size
}

func (spec SizeSpecification) IsSatisfied(p *Product) bool {
	return p.size == spec.size
}

type AndSpecification struct {
	first, second Specification
}

func (spec AndSpecification) IsSatisfied(p *Product) bool {
	return spec.first.IsSatisfied(p) &&
		spec.second.IsSatisfied(p)
}

type BetterFilter struct {}

func (f *BetterFilter) Filter(
	products []Product, spec Specification) []*Product {
	result := make([]*Product, 0)
	for i, v := range products {
		if spec.IsSatisfied(&v) {
			result = append(result, &products[i])
		}
	}
	return result
}

func main_() {
	apple := Product{"Apple", green, small}
	tree := Product{"Tree", green, large}
	house := Product{ "House", blue, large}

	products := []Product{apple, tree, house}

	fmt.Print("Green products (old):\n")
	f := Filter{}
	for _, v := range f.filterByColor(products, green) {
		fmt.Printf(" - %s is green\n", v.name)
	}
	// ^^^ BEFORE

	// vvv AFTER
	fmt.Print("Green products (new):\n")
	greenSpec := ColorSpecification{green}
	bf := BetterFilter{}
	for _, v := range bf.Filter(products, greenSpec) {
		fmt.Printf(" - %s is green\n", v.name)
	}

	largeSpec := SizeSpecification{large}

	largeGreenSpec := AndSpecification{largeSpec, greenSpec}
	fmt.Print("Large blue items:\n")
	for _, v := range bf.Filter(products, largeGreenSpec) {
		fmt.Printf(" - %s is large and green\n", v.name)
	}
}

```

>## Liskov Substitution Principle
>It is based on the concept of "substitutability" – a principle in object-oriented programming stating that an object (such as a class) may be replaced by a sub-object (such as a class that extends the first class) without breaking the program. It is a semantic rather than merely syntactic relation, because it intends to guarantee semantic interoperability of types in a hierarchy, object types in particular

```go
package main

import "fmt"

type Sized interface {
	GetWidth() int
	SetWidth(width int)
	GetHeight() int
	SetHeight(height int)
}

type Rectangle struct {
	width, height int
}

//     vvv !! POINTER
func (r *Rectangle) GetWidth() int {
	return r.width
}

func (r *Rectangle) SetWidth(width int) {
	r.width = width
}

func (r *Rectangle) GetHeight() int {
	return r.height
}

func (r *Rectangle) SetHeight(height int) {
	r.height = height
}

// modified LSP
// If a function takes an interface and
// works with a type T that implements this
// interface, any structure that aggregates T
// should also be usable in that function.
type Square struct {
	Rectangle
}

func NewSquare(size int) *Square {
	sq := Square{}
	sq.width = size
	sq.height = size
	return &sq
}

func (s *Square) SetWidth(width int) {
	s.width = width
	s.height = width
}

func (s *Square) SetHeight(height int) {
	s.width = height
	s.height = height
}

type Square2 struct {
	size int
}

func (s *Square2) Rectangle() Rectangle {
	return Rectangle{s.size, s.size}
}

func UseIt(sized Sized) {
	width := sized.GetWidth()
	sized.SetHeight(10)
	expectedArea := 10 * width
	actualArea := sized.GetWidth() * sized.GetHeight()
	fmt.Print("Expected an area of ", expectedArea,
		", but got ", actualArea, "\n")
}

func main() {
	rc := &Rectangle{2, 3}
	UseIt(rc)

	sq := NewSquare(5)
	UseIt(sq)
}
```

>## Interface Segregation Principle
> In the field of software engineering, the interface segregation principle (ISP) states that no code should be forced to depend on methods it does not use. ISP splits interfaces that are very large into smaller and more specific ones so that clients will only have to know about the methods that are of interest to them.
```go
package main

type Document struct {

}

type Machine interface {
	Print(d Document)
	Fax(d Document)
	Scan(d Document)
}

// ok if you need a multifunction device
type MultiFunctionPrinter struct {
	// ...
}

func (m MultiFunctionPrinter) Print(d Document) {

}

func (m MultiFunctionPrinter) Fax(d Document) {

}

func (m MultiFunctionPrinter) Scan(d Document) {

}

type OldFashionedPrinter struct {
	// ...
}

func (o OldFashionedPrinter) Print(d Document) {
	// ok
}

func (o OldFashionedPrinter) Fax(d Document) {
	panic("operation not supported")
}

// Deprecated: ...
func (o OldFashionedPrinter) Scan(d Document) {
	panic("operation not supported")
}

// better approach: split into several interfaces
type Printer interface {
	Print(d Document)
}

type Scanner interface {
	Scan(d Document)
}

// printer only
type MyPrinter struct {
	// ...
}

func (m MyPrinter) Print(d Document) {
	// ...
}

// combine interfaces
type Photocopier struct {}

func (p Photocopier) Scan(d Document) {
	//
}

func (p Photocopier) Print(d Document) {
	//
}

type MultiFunctionDevice interface {
	Printer
	Scanner
}

// interface combination + decorator
type MultiFunctionMachine struct {
	printer Printer
	scanner Scanner
}

func (m MultiFunctionMachine) Print(d Document) {
	m.printer.Print(d)
}

func (m MultiFunctionMachine) Scan(d Document) {
	m.scanner.Scan(d)
}

func main() {

}
```


>## Dependency Inversion Principle
>In object-oriented design, the dependency inversion principle is a specific methodology for loosely coupled software modules. When following this principle, the conventional dependency relationships established from high-level, policy-setting modules to low-level, dependency modules are reversed, thus rendering high-level modules independent of the low-level module implementation details. The principle states:[1]

> High-level modules should not import anything from low-level modules. Both should depend on abstractions (e.g., interfaces).
Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

```go
package main

import "fmt"

// Dependency Inversion Principle
// HLM should not depend on LLM
// Both should depend on abstractions

type Relationship int

const (
	Parent Relationship = iota
	Child
	Sibling
)

type Person struct {
	name string
	// other useful stuff here
}

type Info struct {
	from *Person
	relationship Relationship
	to *Person
}

type RelationshipBrowser interface {
	FindAllChildrenOf(name string) []*Person
}

type Relationships struct {
	relations []Info
}

func (rs *Relationships) FindAllChildrenOf(name string) []*Person {
	result := make([]*Person, 0)

	for i, v := range rs.relations {
		if v.relationship == Parent &&
			v.from.name == name {
			result = append(result, rs.relations[i].to)
		}
	}

	return result
}

func (rs *Relationships) AddParentAndChild(parent, child *Person) {
	rs.relations = append(rs.relations,
		Info{parent, Parent, child})
	rs.relations = append(rs.relations,
		Info{child, Child, parent})
}

type Research struct {
	// relationships Relationships
	browser RelationshipBrowser // low-level
}


func (r *Research) Investigate() {
	//relations := r.relationships.relations
	//for _, rel := range relations {
	//	if rel.from.name == "John" &&
	//		rel.relationship == Parent {
	//		fmt.Println("John has a child called", rel.to.name)
	//	}
	//}

	for _, p := range r.browser.FindAllChildrenOf("John") {
		fmt.Println("John has a child called", p.name)
	}
}

func main() {
	parent := Person{"John" }
	child1 := Person{ "Chris" }
	child2 := Person{ "Matt" }

	// low-level module
	relationships := Relationships{}
	relationships.AddParentAndChild(&parent, &child1)
	relationships.AddParentAndChild(&parent, &child2)

	research := Research{&relationships}
	research.Investigate()
}
```

</br>
</br>

# <a id="CREATIONAL_DESIGN_PATTERNS"></a> **CREATIONAL DESIGN PATTERN**

</br>


# <a id="builder2"></a><u>BUILDER</u>
> **The builder pattern is a design pattern designed to provide a flexible solution to various object creation problems in object-oriented programming. The intent of the builder design pattern is to separate the construction of a complex object from its representation. It is one of the Gang of Four design patterns.**

</br>


<div class='boxBorder'>

---
>>## <u>GO TIPS</u>
>>> ****Anonymous Embedded Fields***
An anonymous embedded field is a struct field that doesn’t have an explicit field name. Structs that have an anonymous field obtain all of the methods and properties of the nested struct. These methods and properties are called “promoted” methods and properties. Anonymous embedded fields can also be accessed by the type name directly.*

>>> `type Eagle struct {
  Bird // anonymous embedded field
}
e := Eagle{name: "Baldie"}
e.makeSound() // chirp
e.Bird.makeSound() // chirp`


>>> *In GO, **the function is also a type.** Two functions will be of the same type if
They have the same number of arguments with each argument is of the same type
They have the same number of return values and each return value is of the same type
Function as user-defined type can be declared using the type keyword like below. area is the name of the function of type func(int, int) int*

>>> `type area func(int, int) int`
</div>
</br>


```go
package main

import (
  "fmt"
  "strings"
)

const (
  indentSize = 2
)

type HtmlElement struct {
  name, text string
  elements []HtmlElement
}

func (e *HtmlElement) String() string {
  return e.string(0)
}

func (e *HtmlElement) string(indent int) string {
  sb := strings.Builder{}
  i := strings.Repeat(" ", indentSize * indent)
  sb.WriteString(fmt.Sprintf("%s<%s>\n",
    i, e.name))
  if len(e.text) > 0 {
    sb.WriteString(strings.Repeat(" ",
      indentSize * (indent + 1)))
    sb.WriteString(e.text)
    sb.WriteString("\n")
  }

  for _, el := range e.elements {
    sb.WriteString(el.string(indent+1))
  }
  sb.WriteString(fmt.Sprintf("%s</%s>\n",
    i, e.name))
  return sb.String()
}

type HtmlBuilder struct {
  rootName string
  root HtmlElement
}

func NewHtmlBuilder(rootName string) *HtmlBuilder {
  b := HtmlBuilder{rootName,
    HtmlElement{rootName, "", []HtmlElement{}}}
  return &b
}

func (b *HtmlBuilder) String() string {
  return b.root.String()
}

func (b *HtmlBuilder) AddChild(
  childName, childText string) {
  e := HtmlElement{childName, childText, []HtmlElement{}}
  b.root.elements = append(b.root.elements, e)
}

func (b *HtmlBuilder) AddChildFluent(
  childName, childText string) *HtmlBuilder {
  e := HtmlElement{childName, childText, []HtmlElement{}}
  b.root.elements = append(b.root.elements, e)
  return b
}

func main() {
  hello := "hello"
  sb := strings.Builder{}
  sb.WriteString("<p>")
  sb.WriteString(hello)
  sb.WriteString("</p>")
  fmt.Printf("%s\n", sb.String())

  words := []string{"hello", "world"}
  sb.Reset()
  // <ul><li>...</li><li>...</li><li>...</li></ul>'
  sb.WriteString("<ul>")
  for _, v := range words {
    sb.WriteString("<li>")
    sb.WriteString(v)
    sb.WriteString("</li>")
  }
  sb.WriteString("</ul>")
  fmt.Println(sb.String())

  b := NewHtmlBuilder("ul")
  b.AddChildFluent("li", "hello").
    AddChildFluent("li", "world")
  fmt.Println(b.String())
}
```

## <a id='builder_facets'></a>Builder Facet
```go
package main

import "fmt"

type Person struct {
  StreetAddress, Postcode, City string
  CompanyName, Position string
  AnnualIncome int
}

type PersonBuilder struct {
  person *Person // needs to be inited
}

func NewPersonBuilder() *PersonBuilder {
  return &PersonBuilder{&Person{}}
}

func (it *PersonBuilder) Build() *Person {
  return it.person
}

func (it *PersonBuilder) Works() *PersonJobBuilder {
  return &PersonJobBuilder{*it}
}

func (it *PersonBuilder) Lives() *PersonAddressBuilder {
  return &PersonAddressBuilder{*it}
}

type PersonJobBuilder struct {
  PersonBuilder
}

func (pjb *PersonJobBuilder) At(
  companyName string) *PersonJobBuilder {
  pjb.person.CompanyName = companyName
  return pjb
}

func (pjb *PersonJobBuilder) AsA(
  position string) *PersonJobBuilder {
  pjb.person.Position = position
  return pjb
}

func (pjb *PersonJobBuilder) Earning(
  annualIncome int) *PersonJobBuilder {
  pjb.person.AnnualIncome = annualIncome
  return pjb
}

type PersonAddressBuilder struct {
  PersonBuilder
}

func (it *PersonAddressBuilder) At(
  streetAddress string) *PersonAddressBuilder {
  it.person.StreetAddress = streetAddress
  return it
}

func (it *PersonAddressBuilder) In(
  city string) *PersonAddressBuilder {
  it.person.City = city
  return it
}

func (it *PersonAddressBuilder) WithPostcode(
  postcode string) *PersonAddressBuilder {
  it.person.Postcode = postcode
  return it
}

func main() {
  pb := NewPersonBuilder()
  pb.
    Lives().
      At("123 London Road").
      In("London").
      WithPostcode("SW12BC").
    Works().
      At("Fabrikam").
      AsA("Programmer").
      Earning(123000)
  person := pb.Build()
  fmt.Println(*person)
}
```

## <a id='builder_parameter'></a>Builder Parameter
```go

package main

import "strings"

type email struct {
	from, to, subject, body string
}

type EmailBuilder struct {
	email email
}

func (b *EmailBuilder) From(from string) *EmailBuilder {
	if !strings.Contains(from, "@") {
		panic("email should contain @")
	}
	b.email.from = from
	return b
}

func (b *EmailBuilder) To(to string) *EmailBuilder {
	b.email.to = to
	return b
}

func (b *EmailBuilder) Subject(subject string) *EmailBuilder {
	b.email.subject = subject
	return b
}

func (b *EmailBuilder) Body(body string) *EmailBuilder {
	b.email.body = body
	return b
}

func sendMailImpl(email *email) {
	// actually ends the email
}

type build func(*EmailBuilder)
func SendEmail(action build) {
	builder := EmailBuilder{}
	action(&builder)
	sendMailImpl(&builder.email)
}

func main() {
	SendEmail(func(b *EmailBuilder) {
		b.From("foo@bar.com").
			To("bar@baz.com").
			Subject("Meeting").
			Body("Hello, do you want to meet?")
	})
}
```

## <a id='functional_builder'></a>Functional Builder

```go
package main

import "fmt"

type Person struct {
  name, position string
}

type personMod func(*Person)
type PersonBuilder struct {
  actions []personMod
}

func (b *PersonBuilder) Called(name string) *PersonBuilder {
  b.actions = append(b.actions, func(p *Person) {
    p.name = name
  })
  return b
}

func (b *PersonBuilder) Build() *Person {
  p := Person{}
  for _, a := range b.actions {
    a(&p)
  }
  return &p
}

// extend PersonBuilder
func (b *PersonBuilder) WorksAsA(position string) *PersonBuilder {
  b.actions = append(b.actions, func(p *Person) {
    p.position = position
  })
  return b
}

func main() {
  b := PersonBuilder{}
  p := b.Called("Dmitri").WorksAsA("dev").Build()
  fmt.Println(*p)
}
```


# <a id='factories'></a><u>Factories</u>
> In object-oriented programming, the Factory Method is a Creational Design Pattern that allows us to create objects without specifying the exact class that will be instantiated. Instead of creating objects directly, we use a factory method to create and return them.

## <a id='factory_function'></a>Factory Function

```go
package factories

import "fmt"

type Person struct {
  Name string
  Age int
}

// factory function
//func NewPerson(name string, age int) Person {
//  return Person{name, age}
//}

func NewPerson(name string, age int) *Person {
  return &Person{name, age}
}

func main_() {
  // initialize directly
  p := Person{"John", 22}
  fmt.Println(p)

  // use a constructor
  p2 := NewPerson("Jane", 21)
  p2.Age = 30
  fmt.Println(p2)
}

```

## <a id='interface_factory'></a>Interface Factory

```go

```
## <a id='factory_generator'></a>Factory Generator

```go
package main

import "fmt"

type Employee struct {
  Name, Position string
  AnnualIncome int
}

// what if we want factories for specific roles?

// functional approach
func NewEmployeeFactory(position string,
  annualIncome int) func(name string) *Employee {
  return func(name string) *Employee {
    return &Employee{name, position, annualIncome}
  }
}

// structural approach
type EmployeeFactory struct {
  Position string
  AnnualIncome int
}

func NewEmployeeFactory2(position string,
  annualIncome int) *EmployeeFactory {
  return &EmployeeFactory{position, annualIncome}
}

func (f *EmployeeFactory) Create(name string) *Employee {
  return &Employee{name, f.Position, f.AnnualIncome}
}

func main() {
  developerFactory := NewEmployeeFactory("Developer", 60000)
  managerFactory := NewEmployeeFactory("Manager", 80000)

  developer := developerFactory("Adam")
  fmt.Println(developer)

  manager := managerFactory("Jane")
  fmt.Println(manager)

  bossFactory := NewEmployeeFactory2("CEO", 100000)
  // can modify post-hoc
  bossFactory.AnnualIncome = 110000
  boss := bossFactory.Create("Sam")
  fmt.Println(boss)
}
```


## <a id='prototype_factory'></a>Prototype Factory

```go
package factories

import "fmt"

type Employee struct {
  Name, Position string
  AnnualIncome int
}

const (
  Developer = iota
  Manager
)

// functional
func NewEmployee(role int) *Employee {
  switch role {
  case Developer:
    return &Employee{"", "Developer", 60000}
  case Manager:
    return &Employee{"", "Manager", 80000}
  default:
    panic("unsupported role")
  }
}

func main() {
  m := NewEmployee(Manager)
  m.Name = "Sam"
  fmt.Println(m)
}
```

</br>

# <a id='prototype'></a><u>PROTOTYPE</u>

> Prototype design pattern is used when the Object creation is a costly affair and requires a lot of time and resources and you have a similar object already existing. Prototype pattern provides a mechanism to copy the original object to a new object and then modify it according to our needs.


## <a id='deep_copying'></a>Deep Copying

```go
package prototype

import "fmt"

type Address struct {
  StreetAddress, City, Country string
}

type Person struct {
  Name string
  Address *Address
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"}}

  //jane := john

  // shallow copy
  //jane.Name = "Jane" // ok

  //jane.Address.StreetAddress = "321 Baker St"

  //fmt.Println(john.Name, john.Address)
  //fmt.Println(jane.Name, jane. Address)

  // what you really want
  jane := john
  jane.Address = &Address{
    john.Address.StreetAddress,
    john.Address.City,
    john.Address.Country  }

  jane.Name = "Jane" // ok

  jane.Address.StreetAddress = "321 Baker St"

  fmt.Println(john.Name, john.Address)
  fmt.Println(jane.Name, jane. Address)
}

```

## <a id='copy_method'></a>Copy Method

```go
package prototype

import "fmt"

type Address struct {
  StreetAddress, City, Country string
}

func (a *Address) DeepCopy() *Address {
  return &Address{
    a.StreetAddress,
    a.City,
    a.Country }
}

type Person struct {
  Name string
  Address *Address
  Friends []string
}

func (p *Person) DeepCopy() *Person {
  q := *p // copies Name
  q.Address = p.Address.DeepCopy()
  copy(q.Friends, p.Friends)
  return &q
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"},
    []string{"Chris", "Matt"}}

  jane := john.DeepCopy()
  jane.Name = "Jane"
  jane.Address.StreetAddress = "321 Baker St"
  jane.Friends = append(jane.Friends, "Angela")

  fmt.Println(john, john.Address)
  fmt.Println(jane, jane.Address)
}



```



## <a id='copy_serialization'></a>Copy Through Serialization

```go
package prototype

import (
  "bytes"
  "encoding/gob"
  "fmt"
)

type Address struct {
  StreetAddress, City, Country string
}

type Person struct {
  Name string
  Address *Address
  Friends []string
}

func (p *Person) DeepCopy() *Person {
  // note: no error handling below
  b := bytes.Buffer{}
  e := gob.NewEncoder(&b)
  _ = e.Encode(p)

  // peek into structure
  fmt.Println(string(b.Bytes()))

  d := gob.NewDecoder(&b)
  result := Person{}
  _ = d.Decode(&result)
  return &result
}

func main() {
  john := Person{"John",
    &Address{"123 London Rd", "London", "UK"},
    []string{"Chris", "Matt", "Sam"}}

  jane := john.DeepCopy()
  jane.Name = "Jane"
  jane.Address.StreetAddress = "321 Baker St"
  jane.Friends = append(jane.Friends, "Jill")

  fmt.Println(john, john.Address)
  fmt.Println(jane, jane.Address)

}
```


## <a id='proprototype_factory'></a>Prototype Factory

```go
package main

import (
  "bytes"
  "encoding/gob"
  "fmt"
)

type Address struct {
  Suite int
  StreetAddress, City string
}

type Employee struct {
  Name string
  Office Address
}

func (p *Employee) DeepCopy() *Employee {
  // note: no error handling below
  b := bytes.Buffer{}
  e := gob.NewEncoder(&b)
  _ = e.Encode(p)

  // peek into structure
  //fmt.Println(string(b.Bytes()))

  d := gob.NewDecoder(&b)
  result := Employee{}
  _ = d.Decode(&result)
  return &result
}

// employee factory
// either a struct or some functions
var mainOffice = Employee {
  "", Address{0, "123 East Dr", "London"}}
var auxOffice = Employee {
  "", Address{0, "66 West Dr", "London"}}

// utility method for configuring emp
//   ↓ lowercase
func newEmployee(proto *Employee,
  name string, suite int) *Employee {
  result := proto.DeepCopy()
  result.Name = name
  result.Office.Suite = suite
  return result
}

func NewMainOfficeEmployee(
  name string, suite int) *Employee {
    return newEmployee(&mainOffice, name, suite)
}

func NewAuxOfficeEmployee(
  name string, suite int) *Employee {
    return newEmployee(&auxOffice, name, suite)
}

func main() {
  // most people work in one of two offices

  //john := Employee{"John",
  //  Address{100, "123 East Dr", "London"}}
  //
  //jane := john.DeepCopy()
  //jane.Name = "Jane"
  //jane.Office.Suite = 200
  //jane.Office.StreetAddress = "66 West Dr"

  john := NewMainOfficeEmployee("John", 100)
  jane := NewAuxOfficeEmployee("Jane", 200)

  fmt.Println(john)
  fmt.Println(jane)
}



```
</br>

# <a id='singelton'></a><u>Singelton</u>

> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to a singular instance.

> One of the well-known "Gang of Four" design patterns, which describe how to solve recurring problems in object-oriented software,[1] the pattern is useful when exactly one object is needed to coordinate actions across a system.

> More specifically, the singleton pattern allows objects to:[2]:

>	* Ensure they only have one instance
>	* Provide easy access to that instance
>	* Control their instantiation (for example, hiding the constructors of a class)

> The term comes from the mathematical concept of a singleton.


<div class='boxBorder'>

---
>>## <u>GO TIPS</u>
>>> ### **The init Function**
>>>In Go, the init() function can be incredibly powerful and compared to some other languages, is a lot easier to use within your Go programs. These init() functions can be used within a package block and regardless of how many times that package is imported, the init() function will only be called once.
</br>
Now, the fact that it is only called once is something you should pay close attention to. This effectively allows us to set up database connections, or register with various service registries, or perform any number of other tasks that you typically only want to do once.

</br>


>>> ### **Sync Package**
>>> The right way to implement a singleton pattern in Go is to use the sync package’s Once.Do() function. This function makes sure that your specified code is executed only once and never more than once.
</br>
The way to use the Once.Do() function is as below.
</br>

code
```go
package main

    import (
      "fmt"
      "sync"
    )

    type DbConnection struct {}

    var (
      dbConnOnce sync.Once
      conn *DbConnection
    )

    func GetConnection() *DbConnection {
      dbConnOnce.Do(func() {
          conn = &DbConnection{}
      })
      return conn
    }
```


</div>
</br>

## <a id='singelton2'></a>singelton

```go
package main

import (
  "bufio"
  "fmt"
  "os"
  "path/filepath"
  "strconv"
  "sync"
)

// think of a module as a singleton
type Database interface {
  GetPopulation(name string) int
}

type singletonDatabase struct {
  capitals map[string]int
}

func (db *singletonDatabase) GetPopulation(
  name string) int {
  return db.capitals[name]
}

// both init and sync.Once are thread-safe
// but only sync.Once is lazy

var once sync.Once
//var instance *singletonDatabase
//
//func GetSingletonDatabase() *singletonDatabase {
//  once.Do(func() {
//    if instance == nil {
//      instance = &singletonDatabase{}
//    }
//  })
//  return instance
//}

var instance Database

// init() — we could, but it's not lazy

func readData(path string) (map[string]int, error) {
  ex, err := os.Executable()
  if err != nil {
    panic(err)
  }
  exPath := filepath.Dir(ex)

  file, err := os.Open(exPath + path)
  if err != nil {
    return nil, err
  }
  defer file.Close()

  scanner := bufio.NewScanner(file)
  scanner.Split(bufio.ScanLines)

  result := map[string]int{}
  
  for scanner.Scan() {
    k := scanner.Text()
    scanner.Scan()
    v, _ := strconv.Atoi(scanner.Text())
    result[k] = v
  }

  return result, nil
}

func GetSingletonDatabase() Database {
 once.Do(func() {
   db := singletonDatabase{}
   caps, err := readData(".\\capitals.txt")
   if err == nil {
     db.capitals = caps
   }
   instance = &db
 })
 return instance
}

func GetTotalPopulation(cities []string) int {
  result := 0
  for _, city := range cities {
    result += GetSingletonDatabase().GetPopulation(city)
  }
  return result
}

func GetTotalPopulationEx(db Database, cities []string) int {
  result := 0
  for _, city := range cities {
    result += db.GetPopulation(city)
  }
  return result
}

type DummyDatabase struct {
  dummyData map[string]int
}

func (d *DummyDatabase) GetPopulation(name string) int {
  if len(d.dummyData) == 0 {
    d.dummyData = map[string]int{
      "alpha" : 1,
      "beta" : 2,
      "gamma" : 3 }
  }
  return d.dummyData[name]
}

func main() {
  db := GetSingletonDatabase()
  pop := db.GetPopulation("Seoul")
  fmt.Println("Pop of Seoul = ", pop)

  cities := []string{"Seoul", "Mexico City"}
  //tp := GetTotalPopulation(cities)
  tp := GetTotalPopulationEx(GetSingletonDatabase(), cities)

  ok := tp == (17500000 + 17400000) // testing on live data
  fmt.Println(ok)

  names := []string{"alpha", "gamma"} // expect 4
  tp = GetTotalPopulationEx(&DummyDatabase{}, names)
  ok = tp == 4
  fmt.Println(ok)
}

```


## <a id='problems_singleton'></a>Problems with Singleton

## <a id='singleton_inversion'></a>Singleton and Dependency Inversion

</br>
</br>

# <a id="structural_design_patterns"></a>**STRUCTURAL DESIGN PATTERNS**

> Structural design pattern is a blueprint of how different objects and classes are combined together to form a bigger structure for achieving multiple goals altogether. 
</br>
The patterns in structural designs show how unique pieces of a system can be combined together in an extensible and flexible manner.
</br>
</br>⚠️***Use the Bridge pattern when you want to divide and organize a monolithic class that has several variants of some functionality (for example, if the class can work with various database servers).***

# <a id='BRIDGE'></a><u>Bridge</u>

> The bridge pattern is a design pattern used in software engineering that is meant to "decouple an abstraction from its implementation so that the two can vary independently", introduced by the Gang of Four.



```go
package structural_bridge

import "fmt"

type Renderer interface {
  RenderCircle(radius float32)
}

type VectorRenderer struct {

}

func (v *VectorRenderer) RenderCircle(radius float32) {
  fmt.Println("Drawing a circle of radius", radius)
}

type RasterRenderer struct {
  Dpi int
}

func (r *RasterRenderer) RenderCircle(radius float32) {
  fmt.Println("Drawing pixels for circle of radius", radius)
}

type Circle struct {
  renderer Renderer
  radius float32
}

func (c *Circle) Draw() {
  c.renderer.RenderCircle(c.radius)
}

func NewCircle(renderer Renderer, radius float32) *Circle {
  return &Circle{renderer: renderer, radius: radius}
}

func (c *Circle) Resize(factor float32) {
  c.radius *= factor
}

func main() {
  raster := RasterRenderer{}
  vector := VectorRenderer{}
  circle := NewCircle(&vector, 5)
  circle.Draw()
}

```

</br>

# <a id='COMPOSITE'></a><u>COMPOSITE</u>
> Composite is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.
</br>
⚠️ ***Use the Composite pattern when you have to implement a tree-like object structure.
</br>⚠️ Use the pattern when you want the client code to treat both simple and complex elements uniformly.***


## <a id='geometric_shapes'></a>Geometric Shapes

```go
package composite

import (
  "fmt"
  "strings"
)

type GraphicObject struct {
  Name, Color string
  Children []GraphicObject
}

func (g *GraphicObject) String() string {
  sb := strings.Builder{}
  g.print(&sb, 0)
  return sb.String()
}

func (g *GraphicObject) print(sb *strings.Builder, depth int) {
  sb.WriteString(strings.Repeat("*", depth))
  if len(g.Color) > 0 {
    sb.WriteString(g.Color)
    sb.WriteRune(' ')
  }
  sb.WriteString(g.Name)
  sb.WriteRune('\n')

  for _, child := range g.Children {
    child.print(sb, depth+1)
  }
}

func NewCircle(color string) *GraphicObject {
  return &GraphicObject{"Circle", color, nil}
}

func NewSquare(color string) *GraphicObject {
  return &GraphicObject{"Square", color, nil}
}

func main() {
  drawing := GraphicObject{"My Drawing", "", nil}
  drawing.Children = append(drawing.Children, *NewSquare("Red"))
  drawing.Children = append(drawing.Children, *NewCircle("Yellow"))

  group := GraphicObject{"Group 1", "", nil}
  group.Children = append(group.Children, *NewCircle("Blue"))
  group.Children = append(group.Children, *NewSquare("Blue"))
  drawing.Children = append(drawing.Children, group)

  fmt.Println(drawing.String())
}





```

## <a id='neural_networks'></a>Neural Networks

```go
package composite

type NeuronInterface interface {
  Iter() []*Neuron
}

type Neuron struct {
  In, Out []*Neuron
}

func (n *Neuron) Iter() []*Neuron {
  return []*Neuron{n}
}

func (n *Neuron) ConnectTo(other *Neuron) {
  n.Out = append(n.Out, other)
  other.In = append(other.In, n)
}

type NeuronLayer struct {
  Neurons []Neuron
}

func (n *NeuronLayer) Iter() []*Neuron {
  result := make([]*Neuron, 0)
  for i := range n.Neurons {
    result = append(result, &n.Neurons[i])
  }
  return result
}

func NewNeuronLayer(count int) *NeuronLayer {
  return &NeuronLayer{make([]Neuron, count)}
}

func Connect(left, right NeuronInterface) {
  for _, l := range left.Iter() {
    for _, r := range right.Iter() {
      l.ConnectTo(r)
    }
  }
}

func main() {
  neuron1, neuron2 := &Neuron{}, &Neuron{}
  layer1, layer2 := NewNeuronLayer(3), NewNeuronLayer(4)

  //neuron1.ConnectTo(&neuron2)

  Connect(neuron1, neuron2)
  Connect(neuron1, layer1)
  Connect(layer2, neuron1)
  Connect(layer1, layer2)
}
```

</br>

# <a id='DECORATOR'></a><u>DECORATOR</u>

> **Decorator** is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.
</br>
⚠️  ***Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.***
</br>
⚠️  ***Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance.***

## <a id='multiple_aggregation'></a>Multiple Aggregation

```go
package decorator

import "fmt"

type Shape interface {
  Render() string
}

type Circle struct {
  Radius float32
}

func (c *Circle) Render() string {
  return fmt.Sprintf("Circle of radius %f",
    c.Radius)
}

func (c *Circle) Resize(factor float32) {
  c.Radius *= factor
}

type Square struct {
  Side float32
}

func (s *Square) Render() string {
  return fmt.Sprintf("Square with side %f", s.Side)
}

// possible, but not generic enough
type ColoredSquare struct {
  Square
  Color string
}

type ColoredShape struct {
  Shape Shape
  Color string
}

func (c *ColoredShape) Render() string {
  return fmt.Sprintf("%s has the color %s",
    c.Shape.Render(), c.Color)
}

type TransparentShape struct {
  Shape Shape
  Transparency float32
}

func (t *TransparentShape) Render() string {
  return fmt.Sprintf("%s has %f%% transparency",
    t.Shape.Render(), t.Transparency * 100.0)
}

func main() {
  circle := Circle{2}
  fmt.Println(circle.Render())

  redCircle := ColoredShape{&circle, "Red"}
  fmt.Println(redCircle.Render())

  rhsCircle := TransparentShape{&redCircle, 0.5}
  fmt.Println(rhsCircle.Render())
}
```




## <a id='Decorator2'></a>Decorator

```go
package main

import "fmt"

/*
type Bird struct {
  Age int
}

func (b *Bird) Fly() {
  if b.Age >= 10 {
    fmt.Println("Flying!")
  }
}

type Lizard struct {
  Age int
}

func (l *Lizard) Crawl() {
  if l.Age < 10 {
    fmt.Println("Crawling!")
  }
}

type Dragon struct {
  Bird
  Lizard
}
*/

type Aged interface {
  Age() int
  SetAge(age int)
}

type Bird struct {
  age int
}

func (b *Bird) Age() int { return b.age }
func (b *Bird) SetAge(age int) { b.age = age }

func (b *Bird) Fly() {
  if b.age >= 10 {
    fmt.Println("Flying!")
  }
}

type Lizard struct {
  age int
}

func (l *Lizard) Age() int { return l.age }
func (l *Lizard) SetAge(age int) { l.age = age }

func (l *Lizard) Crawl() {
  if l.age < 10 {
    fmt.Println("Crawling!")
  }
}

type Dragon struct {
  bird Bird
  lizard Lizard
}

func (d *Dragon) Age() int {
  return d.bird.age
}

func (d *Dragon) SetAge(age int) {
  d.bird.SetAge(age)
  d.lizard.SetAge(age)
}

func (d *Dragon) Fly() {
  d.bird.Fly()
}

func (d *Dragon) Crawl() {
  d.lizard.Crawl()
}

func NewDragon() *Dragon {
  return &Dragon{Bird{}, Lizard{}}
}

func main() {
  //d := Dragon{}
  //d.Bird.Age = 10
  //fmt.Println(d.Lizard.Age)
  //d.Fly()
  //d.Crawl()

  d := NewDragon()
  d.SetAge(10)
  d.Fly()
  d.Crawl()
}
```


# <a id='FACADE'></a><u>Façade</u>

> The facade pattern (also spelled façade) is a software-design pattern commonly used in object-oriented programming.
</br> Analogous to a facade in architecture, a facade is an object that serves as a front-facing interface masking more complex underlying or structural code.
</br>
⚠️  Use the Facade pattern when you need to have a limited but straightforward interface to a complex subsystem.
</br>
⚠️  Use the Facade when you want to structure a subsystem into layers.

```go
package facade

type Buffer struct {
  width, height int
  buffer []rune
}

func NewBuffer(width, height int) *Buffer {
  return &Buffer { width, height,
    make([]rune, width*height)}
}

func (b *Buffer) At(index int) rune {
  return b.buffer[index]
}

type Viewport struct {
  buffer *Buffer
  offset int
}

func NewViewport(buffer *Buffer) *Viewport {
  return &Viewport{buffer: buffer}
}

func (v *Viewport) GetCharacterAt(index int) rune {
  return v.buffer.At(v.offset + index)
}

// a facade over buffers and viewports
type Console struct {
  buffers []*Buffer
  viewports []*Viewport
  offset int
}

func NewConsole() *Console {
  b := NewBuffer(10, 10)
  v := NewViewport(b)
  return &Console{[]*Buffer{b}, []*Viewport{v}, 0}
}

func (c *Console) GetCharacterAt(index int) rune {
  return c.viewports[0].GetCharacterAt(index)
}

func main() {
  c := NewConsole()
  u := c.GetCharacterAt(1)
}
```


</br>
</br>

# <a id='FLYWEIGHT'></a><u>FLYWEIGHT</u>

> Flyweight design pattern is used when we need to create a lot of Objects of a class.
</br> Since every object consumes memory space that can be crucial for low memory devices, such as mobile devices or embedded systems, flyweight design pattern can be applied to reduce the load on memory by sharing objects.
</br>
</br>
⚠️  Use the Flyweight pattern only when your program must support a huge number of objects which barely fit into available RAM.

## <a id='Text_Formatting'></a>Text Formatting

```go
package main

import (
  "fmt"
  "strings"
  "unicode"
)

type FormattedText struct {
  plainText  string
  capitalize []bool
}

func (f *FormattedText) String() string {
  sb := strings.Builder{}
  for i := 0; i < len(f.plainText); i++ {
    c := f.plainText[i]
    if f.capitalize[i] {
      sb.WriteRune(unicode.ToUpper(rune(c)))
    } else {
      sb.WriteRune(rune(c))
    }
  }
  return sb.String()
}

func NewFormattedText(plainText string) *FormattedText {
  return &FormattedText{plainText,
    make([]bool, len(plainText))}
}

func (f *FormattedText) Capitalize(start, end int) {
  for i := start; i <= end; i++ {
    f.capitalize[i] = true
  }
}

type TextRange struct {
  Start, End int
  Capitalize, Bold, Italic bool
}

func (t *TextRange) Covers(position int) bool {
  return position >= t.Start && position <= t.End
}

type BetterFormattedText struct {
  plainText string
  formatting []*TextRange
}

func (b *BetterFormattedText) String() string {
  sb := strings.Builder{}

  for i := 0; i < len(b.plainText); i++ {
    c := b.plainText[i]
    for _, r := range b.formatting {
      if r.Covers(i) && r.Capitalize {
        c = uint8(unicode.ToUpper(rune(c)))
      }
    }
    sb.WriteRune(rune(c))
  }

  return sb.String()
}

func NewBetterFormattedText(plainText string) *BetterFormattedText {
  return &BetterFormattedText{plainText: plainText}
}

func (b *BetterFormattedText) Range(start, end int) *TextRange {
  r := &TextRange{start, end, false, false, false}
  b.formatting = append(b.formatting, r)
  return r
}



func main() {
  text := "This is a brave new world"

  ft := NewFormattedText(text)
  ft.Capitalize(10, 15) // brave
  fmt.Println(ft.String())

  bft := NewBetterFormattedText(text)
  bft.Range(16, 19).Capitalize = true // new
  fmt.Println(bft.String())
}
```
</br>
</br>

## <a id='User_Names'></a>User Names

```go
package main

import (
  "fmt"
  "strings"
)

type User struct {
  FullName string
}

func NewUser(fullName string) *User {
  return &User{FullName: fullName}
}

var allNames []string
type User2 struct {
  names []uint8
}

func NewUser2(fullName string) *User2 {
  getOrAdd := func(s string) uint8 {
    for i := range allNames {
      if allNames[i] == s {
        return uint8(i)
      }
    }
    allNames = append(allNames, s)
    return uint8(len(allNames) - 1)
  }

  result := User2{}
  parts := strings.Split(fullName, " ")
  for _, p := range parts {
    result.names = append(result.names, getOrAdd(p))
  }
  return &result
}

func (u *User2) FullName() string {
  var parts []string
  for _, id := range u.names {
    parts = append(parts, allNames[id])
  }
  return strings.Join(parts, " ")
}

func main() {
  john := NewUser("John Doe")
  jane := NewUser("Jane Doe")
  alsoJane := NewUser("Jane Smith")
  fmt.Println(john.FullName)
  fmt.Println(jane.FullName)
  fmt.Println("Memory taken by users:",
    len([]byte(john.FullName)) +
      len([]byte(alsoJane.FullName)) +
      len([]byte(jane.FullName)))

  john2 := NewUser2("John Doe")
  jane2 := NewUser2("Jane Doe")
  alsoJane2 := NewUser2("Jane Smith")
  fmt.Println(john2.FullName())
  fmt.Println(jane2.FullName())
  totalMem := 0
  for _, a := range allNames {
    totalMem += len([]byte(a))
  }
  totalMem += len(john2.names)
  totalMem += len(jane2.names)
  totalMem += len(alsoJane2.names)
  fmt.Println("Memory taken by users2:", totalMem)
}
```
</br>
</br>

# <a id='PROXY'></a><u>PROXY</u>
> ***Proxy design pattern intent according to GoF is: Provide a surrogate or placeholder for another object to control access to it. The definition itself is very clear and proxy design pattern is used when we want to provide controlled access of a functionality.***
</br>
</br>
⚠️  Lazy initialization (virtual proxy). This is when you have a heavyweight service object that wastes system resources by being always up, even though you only need it from time to time.
</br>
</br>
⚠️  Access control (protection proxy). This is when you want only specific clients to be able to use the service object; for instance, when your objects are crucial parts of an operating system and clients are various launched applications (including malicious ones).
</br>
</br>
⚠️  Local execution of a remote service (remote proxy). This is when the service object is located on a remote server.
</br>
</br>
⚠️  Logging requests (logging proxy). This is when you want to keep a history of requests to the service object.

## <a id='Protection_Proxy'></a>Protection Proxy
```go
package proxy

import "fmt"

type Driven interface {
  Drive()
}

type Car struct {}

func (c *Car) Drive() {
  fmt.Println("Car being driven")
}

type Driver struct {
  Age int
}

type CarProxy struct {
  car Car
  driver *Driver
}

func (c *CarProxy) Drive() {
  if c.driver.Age >= 16 {
    c.car.Drive()
  } else {
    fmt.Println("Driver too young")
  }
}

func NewCarProxy(driver *Driver) *CarProxy {
  return &CarProxy{Car{}, driver}
}

func main() {
  car := NewCarProxy(&Driver{12})
  car.Drive()
}
```

## <a id='Virtual_Proxy'></a>Virtual Proxy

```go
package main

import "fmt"

type Image interface {
  Draw()
}

type Bitmap struct {
  filename string
}

func (b *Bitmap) Draw() {
  fmt.Println("Drawing image", b.filename)
}

func NewBitmap(filename string) *Bitmap {
  fmt.Println("Loading image from", filename)
  return &Bitmap{filename: filename}
}

func DrawImage(image Image) {
  fmt.Println("About to draw the image")
  image.Draw()
  fmt.Println("Done drawing the image")
}

type LazyBitmap struct {
  filename string
  bitmap *Bitmap
}

func (l *LazyBitmap) Draw() {
  if l.bitmap == nil {
    l.bitmap = NewBitmap(l.filename)
  }
  l.bitmap.Draw()
}

func NewLazyBitmap(filename string) *LazyBitmap {
  return &LazyBitmap{filename: filename}
}

func main() {
  //bmp := NewBitmap("demo.png")
  bmp := NewLazyBitmap("demo.png")
  DrawImage(bmp)
}
```


## <a id='Proxy_vs_Decorator'></a>Proxy vs Decorator

```go

```

</br>
</br>




# <a id="BEHAVIORAL_DESIGN_PATTERN"></a>**BEHAVIORAL DESIGN PATTERNS**

### **Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects.**

</br>

# <a id='CHAIN_OF_RESPONSABILITY'></a><u>CHAIN OF RESPONSABILITY</u>

> **Chain of Responsibility** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

</br>

> ⚠️  **Use the Chain of Responsibility pattern when your program is expected to process different kinds of requests in various ways, but the exact types of requests and their sequences are unknown beforehand.**
</br>
⚠️   **Use the pattern when it’s essential to execute several handlers in a particular order.**
</br>
⚠️ **Use the CoR pattern when the set of handlers and their order are supposed to change at runtime.**

## <a id='Method_Chain'></a>Method Chain

```go
package chainofresponsibility

import "fmt"

type Creature struct {
  Name string
  Attack, Defense int
}

func (c *Creature) String() string {
  return fmt.Sprintf("%s (%d/%d)",
    c.Name, c.Attack, c.Defense)
}

func NewCreature(name string, attack int, defense int) *Creature {
  return &Creature{Name: name, Attack: attack, Defense: defense}
}

type Modifier interface {
  Add(m Modifier)
  Handle()
}

type CreatureModifier struct {
  creature *Creature
  next Modifier // singly linked list
}

func (c *CreatureModifier) Add(m Modifier) {
  if c.next != nil {
    c.next.Add(m)
  } else { c.next = m }
}

func (c *CreatureModifier) Handle() {
  if c.next != nil {
    c.next.Handle()
  }
}

func NewCreatureModifier(creature *Creature) *CreatureModifier {
  return &CreatureModifier{creature: creature}
}

type DoubleAttackModifier struct {
  CreatureModifier
}

func NewDoubleAttackModifier(
  c *Creature) *DoubleAttackModifier {
  return &DoubleAttackModifier{CreatureModifier{
    creature: c }}
}

type IncreasedDefenseModifier struct {
  CreatureModifier
}

func NewIncreasedDefenseModifier(
  c *Creature) *IncreasedDefenseModifier {
  return &IncreasedDefenseModifier{CreatureModifier{
    creature: c }}
}

func (i *IncreasedDefenseModifier) Handle() {
  if i.creature.Attack <= 2 {
    fmt.Println("Increasing",
      i.creature.Name, "\b's defense")
    i.creature.Defense++
  }
  i.CreatureModifier.Handle()
}

func (d *DoubleAttackModifier) Handle() {
  fmt.Println("Doubling", d.creature.Name,
    "attack...")
  d.creature.Attack *= 2
  d.CreatureModifier.Handle()
}

type NoBonusesModifier struct {
  CreatureModifier
}

func NewNoBonusesModifier(
  c *Creature) *NoBonusesModifier {
  return &NoBonusesModifier{CreatureModifier{
    creature: c }}
}

func (n *NoBonusesModifier) Handle() {
  // nothing here!
}

func main() {
  goblin := NewCreature("Goblin", 1, 1)
  fmt.Println(goblin.String())

  root := NewCreatureModifier(goblin)

  //root.Add(NewNoBonusesModifier(goblin))

  root.Add(NewDoubleAttackModifier(goblin))
  root.Add(NewIncreasedDefenseModifier(goblin))
  root.Add(NewDoubleAttackModifier(goblin))



  // eventually process the entire chain
  root.Handle()
  fmt.Println(goblin.String())
}  
```


## <a id='Command_Query_Separation'></a>Command Query Separation
> Command-query separation (CQS) is a principle of imperative computer programming. It was devised by Bertrand Meyer as part of his pioneering work on the Eiffel programming language. It states that every method should either be a command that performs an action, or a query that returns data to the caller, but not both.



## <a id='Broker_Chain'></a>Broker Chain

```go
package main

import (
  "fmt"
  "sync"
)

// cqs, mediator, cor

type Argument int

const (
  Attack Argument = iota
  Defense
)

type Query struct {
  CreatureName string
  WhatToQuery Argument
  Value int
}

type Observer interface {
  Handle(*Query)
}

type Observable interface {
  Subscribe(o Observer)
  Unsubscribe(o Observer)
  Fire(q *Query)
}

type Game struct {
  observers sync.Map
}

func (g *Game) Subscribe(o Observer) {
  g.observers.Store(o, struct{}{})
  //                   ↑↑↑ empty anon struct
}

func (g *Game) Unsubscribe(o Observer) {
  g.observers.Delete(o)
}

func (g *Game) Fire(q *Query) {
  g.observers.Range(func(key, value interface{}) bool {
    if key == nil {
      return false
    }
    key.(Observer).Handle(q)
    return true
  })
}

type Creature struct {
  game *Game
  Name string
  attack, defense int // ← private!
}

func NewCreature(game *Game, name string, attack int, defense int) *Creature {
  return &Creature{game: game, Name: name, attack: attack, defense: defense}
}

func (c *Creature) Attack() int {
  q := Query{c.Name, Attack, c.attack}
  c.game.Fire(&q)
  return q.Value
}

func (c *Creature) Defense() int {
  q := Query{c.Name, Defense, c.defense}
  c.game.Fire(&q)
  return q.Value
}

func (c *Creature) String() string {
  return fmt.Sprintf("%s (%d/%d)",
    c.Name, c.Attack(), c.Defense())
}

// data common to all modifiers
type CreatureModifier struct {
  game *Game
  creature *Creature
}

func (c *CreatureModifier) Handle(*Query) {
  // nothing here!
}

type DoubleAttackModifier struct {
  CreatureModifier
}

func NewDoubleAttackModifier(g *Game, c *Creature) *DoubleAttackModifier {
  d := &DoubleAttackModifier{CreatureModifier{g, c}}
  g.Subscribe(d)
  return d
}

func (d *DoubleAttackModifier) Handle(q *Query) {
  if q.CreatureName == d.creature.Name &&
    q.WhatToQuery == Attack {
    q.Value *= 2
  }
}

func (d *DoubleAttackModifier) Close() error {
  d.game.Unsubscribe(d)
  return nil
}

func main() {
  game := &Game{sync.Map{}}
  goblin := NewCreature(game, "Strong Goblin", 2, 2)
  fmt.Println(goblin.String())

  {
    m := NewDoubleAttackModifier(game, goblin)
    fmt.Println(goblin.String())
    m.Close()
  }

  fmt.Println(goblin.String())
}
```

</br>
</br>

# <a id='COMMAND'></a><u>COMMAND</u>

> **In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time.**

> ⚠️  **Use the Command pattern when you want to parametrize objects with operations.**
</br>
⚠️   **Use the Command pattern when you want to queue operations, schedule their execution, or execute them remotely.**
</br>
⚠️ **Use the Command pattern when you want to implement reversible operations.**


## <a id='Command'></a>Command & <a id='Undo_Operations'></a>Undo Operations

```go
package command

import "fmt"

var overdraftLimit = -500
type BankAccount struct {
  balance int
}

func (b *BankAccount) Deposit(amount int) {
  b.balance += amount
  fmt.Println("Deposited", amount,
    "\b, balance is now", b.balance)
}

func (b *BankAccount) Withdraw(amount int) bool {
  if b.balance - amount >= overdraftLimit {
    b.balance -= amount
    fmt.Println("Withdrew", amount,
      "\b, balance is now", b.balance)
    return true
  }
  return false
}

type Command interface {
  Call()
  Undo()
}

type Action int
const (
  Deposit Action = iota
  Withdraw
)

type BankAccountCommand struct {
  account *BankAccount
  action Action
  amount int
  succeeded bool
}

func (b *BankAccountCommand) Call() {
  switch b.action {
  case Deposit:
    b.account.Deposit(b.amount)
    b.succeeded = true
  case Withdraw:
    b.succeeded = b.account.Withdraw(b.amount)
  }
}

func (b *BankAccountCommand) Undo() {
  if !b.succeeded { return }
  switch b.action {
  case Deposit:
    b.account.Withdraw(b.amount)
  case Withdraw:
    b.account.Deposit(b.amount)
  }
}

func NewBankAccountCommand(account *BankAccount, action Action, amount int) *BankAccountCommand {
  return &BankAccountCommand{account: account, action: action, amount: amount}
}

func main() {
  ba := BankAccount{}
  cmd := NewBankAccountCommand(&ba, Deposit, 100)
  cmd.Call()
  cmd2 := NewBankAccountCommand(&ba, Withdraw, 50)
  cmd2.Call()
  fmt.Println(ba)
}
```

## <a id='Composite_Command'></a>Composite Command

```go
package main

import "fmt"

var overdraftLimit = -500
type BankAccount struct {
  balance int
}

func (b *BankAccount) Deposit(amount int) {
  b.balance += amount
  fmt.Println("Deposited", amount,
    "\b, balance is now", b.balance)
}

func (b *BankAccount) Withdraw(amount int) bool {
  if b.balance - amount >= overdraftLimit {
    b.balance -= amount
    fmt.Println("Withdrew", amount,
      "\b, balance is now", b.balance)
    return true
  }
  return false
}

type Command interface {
  Call()
  Undo()
  Succeeded() bool
  SetSucceeded(value bool)
}

type Action int
const (
  Deposit Action = iota
  Withdraw
)

type BankAccountCommand struct {
  account *BankAccount
  action Action
  amount int
  succeeded bool
}

func (b *BankAccountCommand) SetSucceeded(value bool) {
  b.succeeded = value
}

// additional member
func (b *BankAccountCommand) Succeeded() bool {
  return b.succeeded
}

func (b *BankAccountCommand) Call() {
  switch b.action {
  case Deposit:
    b.account.Deposit(b.amount)
    b.succeeded = true
  case Withdraw:
    b.succeeded = b.account.Withdraw(b.amount)
  }
}

func (b *BankAccountCommand) Undo() {
  if !b.succeeded { return }
  switch b.action {
  case Deposit:
    b.account.Withdraw(b.amount)
  case Withdraw:
    b.account.Deposit(b.amount)
  }
}

type CompositeBankAccountCommand struct {
  commands []Command
}

func (c *CompositeBankAccountCommand) Succeeded() bool {
  for _, cmd := range c.commands {
    if !cmd.Succeeded() {
      return false
    }
  }
  return true
}

func (c *CompositeBankAccountCommand) SetSucceeded(value bool) {
  for _, cmd := range c.commands {
    cmd.SetSucceeded(value)
  }
}

func (c *CompositeBankAccountCommand) Call() {
  for _, cmd := range c.commands {
    cmd.Call()
  }
}

func (c *CompositeBankAccountCommand) Undo() {
  // undo in reverse order
  for idx := range c.commands {
    c.commands[len(c.commands)-idx-1].Undo()
  }
}

func NewBankAccountCommand(account *BankAccount, action Action, amount int) *BankAccountCommand {
  return &BankAccountCommand{account: account, action: action, amount: amount}
}

type MoneyTransferCommand struct {
  CompositeBankAccountCommand
  from, to *BankAccount
  amount int
}

func NewMoneyTransferCommand(from *BankAccount, to *BankAccount, amount int) *MoneyTransferCommand {
  c := &MoneyTransferCommand{from: from, to: to, amount: amount}
  c.commands = append(c.commands,
    NewBankAccountCommand(from, Withdraw, amount))
  c.commands = append(c.commands,
    NewBankAccountCommand(to, Deposit, amount))
  return c
}

func (m *MoneyTransferCommand) Call() {
  ok := true
  for _, cmd := range m.commands {
    if ok {
      cmd.Call()
      ok = cmd.Succeeded()
    } else {
      cmd.SetSucceeded(false)
    }
  }
}

func main() {
  ba := &BankAccount{}
  cmdDeposit := NewBankAccountCommand(ba, Deposit, 100)
  cmdWithdraw := NewBankAccountCommand(ba, Withdraw, 1000)
  cmdDeposit.Call()
  cmdWithdraw.Call()
  fmt.Println(ba)
  cmdWithdraw.Undo()
  cmdDeposit.Undo()
  fmt.Println(ba)

  from := BankAccount{100}
  to := BankAccount{0}
  mtc := NewMoneyTransferCommand(&from, &to, 100) // try 1000
  mtc.Call()

  fmt.Println("from=", from, "to=", to)

  fmt.Println("Undoing...")
  mtc.Undo()
  fmt.Println("from=", from, "to=", to)
}
```

## <a id='Functional_Command'></a>Functional Command

```go
package command

import "fmt"

type BankAccount struct {
  Balance int
}

func Deposit(ba *BankAccount, amount int) {
  fmt.Println("Depositing", amount)
  ba.Balance += amount
}

func Withdraw(ba *BankAccount, amount int) {
  if ba.Balance >= amount {
    fmt.Println("Withdrawing", amount)
    ba.Balance -= amount
  }
}

func main() {
  ba := &BankAccount{0}
  var commands []func()
  commands = append(commands, func() {
    Deposit(ba, 100)
  })
  commands = append(commands, func() {
    Withdraw(ba, 100)
  })

  for _, cmd := range commands {
    cmd()
  }
}
```

</br>
</br>

# <a id='INTERPRETER'></a><u>INTERPRETER</u>

> **The Interpreter design pattern is used for evaluating a determined language and return an Expression. This pattern can interpret languages such as Java, C#, SQL, or even a new one invented by us, and provide a response based on its evaluation.**


## <a id='Lexing'></a>Lexing & Parsing

```go
package main

import (
  "fmt"
  "strconv"
  "strings"
  "unicode"
)

type Element interface {
  Value() int
}

type Integer struct {
  value int
}

func NewInteger(value int) *Integer {
  return &Integer{value: value}
}

func (i *Integer) Value() int {
  return i.value
}

type Operation int

const (
  Addition Operation = iota
  Subtraction
)

type BinaryOperation struct {
  Type Operation
  Left, Right Element
}

func (b *BinaryOperation) Value() int {
  switch b.Type {
  case Addition:
    return b.Left.Value() + b.Right.Value()
  case Subtraction:
    return b.Left.Value() + b.Right.Value()
  default:
    panic("Unsupported operation")
  }
}

type TokenType int

const (
  Int TokenType = iota
  Plus
  Minus
  Lparen
  Rparen
)

type Token struct {
  Type TokenType
  Text string
}

func (t *Token) String() string {
  return fmt.Sprintf("`%s`", t.Text)
}

func Lex(input string) []Token {
  var result []Token

  // not using range here
  for i := 0; i < len(input); i++ {
    switch input[i] {
    case '+':
      result = append(result, Token{Plus, "+"})
    case '-':
      result = append(result, Token{Minus, "-"})
    case '(':
      result = append(result, Token{Lparen, "("})
    case ')':
      result = append(result, Token{Rparen, ")"})
    default:
      sb := strings.Builder{}
      for j := i; j < len(input); j++ {
        if unicode.IsDigit(rune(input[j])) {
          sb.WriteRune(rune(input[j]))
          i++
        } else {
          result = append(result, Token{
            Int, sb.String() })
          i--
          break
        }
      }
    }
  }
  return result
}

func Parse(tokens []Token) Element {
  result := BinaryOperation{}
  haveLhs := false
  for i := 0; i < len(tokens); i++ {
    token := &tokens[i]
    switch token.Type {
    case Int:
      n, _ := strconv.Atoi(token.Text)
      integer := Integer{n}
      if !haveLhs {
        result.Left = &integer
        haveLhs = true
      } else {
        result.Right = &integer
      }
    case Plus:
      result.Type = Addition
    case Minus:
      result.Type = Subtraction
    case Lparen:
      j := i
      for ; j < len(tokens); j++ {
        if tokens[j].Type == Rparen {
          break
        }
      }
      // now j points to closing bracket, so
      // process subexpression without opening
      var subexp []Token
      for k := i+1; k < j; k++ {
        subexp = append(subexp, tokens[k])
      }
      element := Parse(subexp)
      if !haveLhs {
        result.Left = element
        haveLhs = true
      } else {
        result.Right = element
      }
      i = j
    }
  }
  return &result
}

func main() {
  input := "(13+4)-(12+1)"
  tokens := Lex(input)
  fmt.Println(tokens)

  parsed := Parse(tokens)
  fmt.Printf("%s = %d\n",
    input, parsed.Value())
}
```
</br>
</br>

# <a id='ITERATOR'></a><u>ITERATOR</u>

> **Iterator design pattern in one of the behavioral pattern. 
</br>Iterator pattern is used to provide a standard way to traverse through a group of Objects. Iterator pattern is widely used in Java Collection Framework.
</br> Iterator interface provides methods for traversing through a collection.**

> ⚠️  **Use the Iterator pattern when your collection has a complex data structure under the hood, but you want to hide its complexity from clients (either for convenience or security reasons).**
</br>
⚠️   **Use the pattern to reduce duplication of the traversal code across your app.**
</br>
⚠️ **Use the Iterator when you want your code to be able to traverse different data structures or when types of these structures are unknown beforehand.**


## <a id='Iteration'></a>Iteration

```go
package main

import "fmt"

type Person struct {
  FirstName, MiddleName, LastName string
}

func (p *Person) Names() []string {
  return []string{p.FirstName, p.MiddleName, p.LastName}
}

func main() {
  p := Person{"Alexander", "Graham", "Bell"}
  for _, name := range p.Names() {
    fmt.Println(name)
  }
}

```

## <a id='Tree_Traversal'></a>Tree Traversal

```go
package iterator

import "fmt"

type Node struct {
  Value int
  left, right, parent *Node
}

func NewNode(value int, left *Node, right *Node) *Node {
  n := &Node{Value: value, left: left, right: right}
  left.parent = n
  right.parent = n
  return n
}

func NewTerminalNode(value int) *Node {
  return &Node{Value:value}
}

type InOrderIterator struct {
  Current *Node
  root *Node
  returnedStart bool
}

func NewInOrderIterator(root *Node) *InOrderIterator {
  i := &InOrderIterator{
    Current:       root,
    root:          root,
    returnedStart: false,
  }
  // move to the leftmost element
  for ;i.Current.left != nil; {
    i.Current = i.Current.left
  }
  return i
}

func (i *InOrderIterator) Reset() {
  i.Current = i.root
  i.returnedStart = false
}

func (i *InOrderIterator) MoveNext() bool {
  if i.Current == nil { return false }
  if !i.returnedStart {
    i.returnedStart = true
    return true // can use first element
  }

  if i.Current.right != nil {
    i.Current = i.Current.right
    for ;i.Current.left != nil; {
      i.Current = i.Current.left
    }
    return true
  } else {
    p := i.Current.parent
    for ;p != nil && i.Current == p.right; {
      i.Current = p
      p = p.parent
    }
    i.Current = p
    return i.Current != nil
  }
}

type BinaryTree struct {
  root *Node
}

func NewBinaryTree(root *Node) *BinaryTree {
  return &BinaryTree{root: root}
}

func (b *BinaryTree) InOrder() *InOrderIterator {
  return NewInOrderIterator(b.root)
}

func main() {
  //   1
  //  / \
  // 2   3

  // in-order:  213
  // preorder:  123
  // postorder: 231

  root := NewNode(1,
    NewTerminalNode(2),
    NewTerminalNode(3))
  it := NewInOrderIterator(root)

  for ;it.MoveNext(); {
    fmt.Printf("%d,", it.Current.Value)
  }
  fmt.Println("\b")

  t := NewBinaryTree(root)
  for i := t.InOrder(); i.MoveNext(); {
    fmt.Printf("%d,", i.Current.Value)
  }
  fmt.Println("\b")
}
```
</br>
</br>

# <a id='MEDIATOR'></a><u>MEDIATOR</u>

> **Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. 
</br>The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.**

> ⚠️  **Use the Mediator pattern when it’s hard to change some of the classes because they are tightly coupled to a bunch of other classes.**
</br>
⚠️   **Use the pattern when you can’t reuse a component in a different program because it’s too dependent on other components.**
</br>
⚠️ **Use the Mediator when you find yourself creating tons of component subclasses just to reuse some basic behavior in various contexts.**


## <a id='Chat_Room'></a>Chat Room

```go
package mediator

import "fmt"

type Person struct {
  Name string
  Room *ChatRoom
  chatLog []string
}

func NewPerson(name string) *Person {
  return &Person{Name: name}
}

func (p *Person) Receive(sender, message string) {
  s := fmt.Sprintf("%s: '%s'", sender, message)
  fmt.Printf("[%s's chat session] %s\n", p.Name, s)
  p.chatLog = append(p.chatLog, s)
}

func (p *Person) Say(message string) {
  p.Room.Broadcast(p.Name, message)
}

func (p *Person) PrivateMessage(who, message string) {
  p.Room.Message(p.Name, who, message)
}

type ChatRoom struct {
  people []*Person
}

func (c *ChatRoom) Broadcast(source, message string) {
  for _, p := range c.people {
    if p.Name != source {
      p.Receive(source, message)
    }
  }
}

func (c *ChatRoom) Join(p *Person) {
  joinMsg := p.Name + " joins the chat"
  c.Broadcast("Room", joinMsg)

  p.Room = c
  c.people = append(c.people, p)
}

func (c *ChatRoom) Message(src, dst, msg string) {
  for _, p := range c.people {
    if p.Name == dst {
      p.Receive(src, msg)
    }
  }
}

func main() {
  room := ChatRoom{}

  john := NewPerson("John")
  jane := NewPerson("Jane")

  room.Join(john)
  room.Join(jane)

  john.Say("hi room")
  jane.Say("oh, hey john")

  simon := NewPerson("Simon")
  room.Join(simon)
  simon.Say("hi everyone!")

  jane.PrivateMessage("Simon", "glad you could join us!")
}
```



# <a id='MEMENTO'></a><u>MEMENTO</u>

> The Memento design pattern defines three distinct roles:
</br> **Originator** - the object that knows how to save itself. 
</br>**Caretaker** - the object that knows why and when the Originator needs to save and restore itself. 
</br>**Memento** - the lock box that is written and read by the Originator, and shepherded by the Caretaker.

## <a id='Memento'></a>Memento

```go
package memento

import (
  "fmt"
)

type Memento struct {
  Balance int
}

type BankAccount struct {
  balance int
}

func (b *BankAccount) Deposit(amount int) *Memento {
  b.balance += amount
  return &Memento{b.balance}
}

func (b *BankAccount) Restore(m *Memento) {
  b.balance = m.Balance
}

func main() {
  ba := BankAccount{100}
  m1 := ba.Deposit(50)
  m2 := ba.Deposit(25)
  fmt.Println(ba)

  ba.Restore(m1)
  fmt.Println(ba) // 150

  ba.Restore(m2)
  fmt.Println(ba)
}
```



## <a id='Undo_and_Redo'></a>Undo and Redo


```go
package memento

import "fmt"

type Memento struct {
  Balance int
}

type BankAccount struct {
  balance int
  changes []*Memento
  current int
}

func (b *BankAccount) String() string {
  return fmt.Sprint("Balance = $", b.balance,
    ", current = ", b.current)
}

func NewBankAccount(balance int) *BankAccount {
  b := &BankAccount{balance: balance}
  b.changes = append(b.changes, &Memento{balance})
  return b
}

func (b *BankAccount) Deposit(amount int) *Memento {
  b.balance += amount
  m := Memento{b.balance}
  b.changes = append(b.changes, &m)
  b.current++
  fmt.Println("Deposited", amount,
    ", balance is now", b.balance)
  return &m
}

func (b *BankAccount) Restore(m *Memento) {
  if m != nil {
    b.balance -= m.Balance
    b.changes = append(b.changes, m)
    b.current = len(b.changes) - 1
  }
}

func (b *BankAccount) Undo() *Memento {
  if b.current > 0 {
    b.current--
    m := b.changes[b.current]
    b.balance = m.Balance
    return m
  }
  return nil // nothing to undo
}

func (b *BankAccount) Redo() *Memento {
  if b.current + 1 < len(b.changes) {
    b.current++
    m := b.changes[b.current]
    b.balance = m.Balance
    return m
  }
  return nil
}

func main() {
  ba := NewBankAccount(100)
  ba.Deposit(50)
  ba.Deposit(25)
  fmt.Println(ba)

  ba.Undo()
  fmt.Println("Undo 1:", ba)
  ba.Undo()
  fmt.Println("Undo 2:", ba)
  ba.Redo()
  fmt.Println("Redo:", ba)
}
```



# <a id='OBSERVER'></a><u>OBSERVER</u>

> The observer design pattern enables a subscriber to register with and receive notifications from a provider. 
</br>It's suitable for any scenario that requires push-based notification.
</br> The pattern defines a provider (also known as a subject or an observable) and zero, one, or more observers.

> ⚠️  **Use the Observer pattern when changes to the state of one object may require changing other objects, and the actual set of objects is unknown beforehand or changes dynamically.**
</br>
⚠️   **Use the pattern when some objects in your app must observe others, but only for a limited time or in specific cases.**


<div class='boxBorder'>

---

>> ## <u>GO TIPS</u>
## anonymous parameter
> use as anonymous parameter 
`func (o *Observable) Fire(data interface{}) {`
</br>
***data interface{}***
</br>
can be use to get undefined parameters

## Cast Interface To Struct

`type PropertyChanged struct {
  Name string
  Value interface{}
}`
</br>
`pc, ok := data.(PropertyChanged);`
</br>
`pc.Value.(int)`

---

</div>


## <a id='Observer_and_Observable'></a>Observer and Observable

```go
package observer

import (
  "container/list"
  "fmt"
)

type Observable struct {
  subs *list.List
}

func (o *Observable) Subscribe(x Observer) {
  o.subs.PushBack(x)
}

func (o *Observable) Unsubscribe(x Observer) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    if z.Value.(Observer) == x {
      o.subs.Remove(z)
    }
  }
}

func (o *Observable) Fire(data interface{}) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    z.Value.(Observer).Notify(data)
  }
}

type Observer interface {
  Notify(data interface{})
}

// whenever a person catches a cold,
// a doctor must be called
type Person struct {
  Observable
  Name string
}

func NewPerson(name string) *Person {
  return &Person {
    Observable: Observable{new(list.List)},
    Name: name,
  }
}

func (p *Person) CatchACold() {
  p.Fire(p.Name)
}

type DoctorService struct {}

func (d *DoctorService) Notify(data interface{}) {
  fmt.Printf("A doctor has been called for %s",
    data.(string))
}

func main() {
  p := NewPerson("Boris")
  ds := &DoctorService{}
  p.Subscribe(ds)

  // let's test it!
  p.CatchACold()
}
```


## <a id='Property_Observers'></a>Property Observers


```go
package observer

import (
  "container/list"
  "fmt"
)

type Observable struct {
  subs *list.List
}

func (o *Observable) Subscribe(x Observer) {
  o.subs.PushBack(x)
}

func (o *Observable) Unsubscribe(x Observer) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    if z.Value.(Observer) == x {
      o.subs.Remove(z)
    }
  }
}

func (o *Observable) Fire(data interface{}) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    z.Value.(Observer).Notify(data)
  }
}

type Observer interface {
  Notify(data interface{})
}

type Person struct {
  Observable
  age int
}

func NewPerson(age int) *Person {
  return &Person{Observable{new(list.List)}, age}
}

type PropertyChanged struct {
  Name string
  Value interface{}
}

func (p *Person) Age() int { return p.age }
func (p *Person) SetAge(age int) {
  if age == p.age { return } // no change
  p.age = age
  p.Fire(PropertyChanged{"Age", p.age})
}

type TrafficManagement struct {
  o Observable
}

func (t *TrafficManagement) Notify(data interface{}) {
  if pc, ok := data.(PropertyChanged); ok {
    if pc.Value.(int) >= 16 {
      fmt.Println("Congrats, you can drive now!")
      // we no longer care
      t.o.Unsubscribe(t)
    }
  }
}

func main() {
  p := NewPerson(15)
  t := &TrafficManagement{p.Observable}
  p.Subscribe(t)

  for i := 16; i <= 20; i++ {
    fmt.Println("Setting age to", i)
    p.SetAge(i)
  }
}
```


## <a id='Property_Dependencies'></a>Property Dependencies


```go
package observer

import (
  "container/list"
  "fmt"
)

type Observable struct {
  subs *list.List
}

func (o *Observable) Subscribe(x Observer) {
  o.subs.PushBack(x)
}

func (o *Observable) Unsubscribe(x Observer) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    if z.Value.(Observer) == x {
      o.subs.Remove(z)
    }
  }
}

func (o *Observable) Fire(data interface{}) {
  for z := o.subs.Front(); z != nil; z = z.Next() {
    z.Value.(Observer).Notify(data)
  }
}

type Observer interface {
  Notify(data interface{})
}

type Person struct {
  Observable
  age int
}

func NewPerson(age int) *Person {
  return &Person{Observable{new(list.List)}, age}
}

type PropertyChanged struct {
  Name string
  Value interface{}
}

func (p *Person) Age() int { return p.age }
func (p *Person) SetAge(age int) {
  if age == p.age { return } // no change

  oldCanVote := p.CanVote()

  p.age = age
  p.Fire(PropertyChanged{"Age", p.age})

  if oldCanVote != p.CanVote() {
    p.Fire(PropertyChanged{"CanVote", p.CanVote()})
  }
}

func (p *Person) CanVote() bool {
  return p.age >= 18
}

type ElectrocalRoll struct {
}

func (e *ElectrocalRoll) Notify(data interface{}) {
  if pc, ok := data.(PropertyChanged); ok {
    if pc.Name == "CanVote" && pc.Value.(bool) {
      fmt.Println("Congratulations, you can vote!")
    }
  }
}

func main() {
  p := NewPerson(0)
  er := &ElectrocalRoll{}
  p.Subscribe(er)

  for i := 10; i < 20; i++ {
    fmt.Println("Setting age to", i)
    p.SetAge(i)
  }
}
```

</br>
</br>

# <a id='STATE'></a><u>STATE</u>

> ### **The state pattern is a behavioral software design pattern that allows an object to alter its behavior when its internal state changes. This pattern is close to the concept of finite-state machines.**

>   ⚠️  **Use the State pattern when you have an object that behaves differently depending on its current state, the number of states is enormous, and the state-specific code changes frequently.**
</br>
 ⚠️   **Use the pattern when you have a class polluted with massive conditionals that alter how the class behaves according to the current values of the class’s fields.**
 </br>
 ⚠️   **Use State when you have a lot of duplicate code across similar states and transitions of a condition-based state machine.**

## <a id='Classic_Implementation'></a>Classic Implementation

```go
package state

import "fmt"

type Switch struct {
  State State
}

func NewSwitch() *Switch {
  return &Switch{NewOffState()}
}

func (s *Switch) On() {
  s.State.On(s)
}

func (s *Switch) Off() {
  s.State.Off(s)
}

type State interface {
  On(sw *Switch)
  Off(sw *Switch)
}

type BaseState struct {}

func (s *BaseState) On(sw *Switch) {
  fmt.Println("Light is already on")
}

func (s *BaseState) Off(sw *Switch) {
  fmt.Println("Light is already off")
}

type OnState struct {
  BaseState
}

func NewOnState() *OnState {
  fmt.Println("Light turned on")
  return &OnState{BaseState{}}
}

func (o *OnState) Off(sw *Switch) {
  fmt.Println("Turning light off...")
  sw.State = NewOffState()
}

type OffState struct {
  BaseState
}

func NewOffState() *OffState {
  fmt.Println("Light turned off")
  return &OffState{BaseState{}}
}

func (o *OffState) On(sw *Switch) {
  fmt.Println("Turning light on...")
  sw.State = NewOnState()
}

func main() {
  sw := NewSwitch()
  sw.On()
  sw.Off()
  sw.Off()
}
```


## <a id='Handmade_State_Machine'></a>Handmade State Machine

```go
package state

import (
  "bufio"
  "fmt"
  "os"
  "strconv"
)

type State int

const (
  OffHook State = iota
  Connecting
  Connected
  OnHold
  OnHook
)

func (s State) String() string {
  switch s {
  case OffHook: return "OffHook"
  case Connecting: return "Connecting"
  case Connected: return "Connected"
  case OnHold: return "OnHold"
  case OnHook: return "OnHook"
  }
  return "Unknown"
}

type Trigger int

const (
  CallDialed Trigger = iota
  HungUp
  CallConnected
  PlacedOnHold
  TakenOffHold
  LeftMessage
)

func (t Trigger) String() string {
  switch t {
  case CallDialed: return "CallDialed"
  case HungUp: return "HungUp"
  case CallConnected: return "CallConnected"
  case PlacedOnHold: return "PlacedOnHold"
  case TakenOffHold: return "TakenOffHold"
  case LeftMessage: return "LeftMessage"
  }
  return "Unknown"
}

type TriggerResult struct {
  Trigger Trigger
  State State
}

var rules = map[State][]TriggerResult {
  OffHook: {
    {CallDialed, Connecting},
  },
  Connecting: {
    {HungUp, OffHook},
    {CallConnected, Connected},
  },
  Connected: {
    {LeftMessage, OnHook},
    {HungUp, OnHook},
    {PlacedOnHold, OnHold},
  },
  OnHold: {
    {TakenOffHold, Connected},
    {HungUp, OnHook},
  },
}

func main() {
  state, exitState := OffHook, OnHook
  for ok := true; ok; ok = state != exitState {
    fmt.Println("The phone is currently", state)
    fmt.Println("Select a trigger:")

    for i := 0; i < len(rules[state]); i++ {
      tr := rules[state][i]
      fmt.Println(strconv.Itoa(i), ".", tr.Trigger)
    }

    input, _, _ := bufio.NewReader(os.Stdin).ReadLine()
    i, _ := strconv.Atoi(string(input))

    tr := rules[state][i]
    state = tr.State
  }
  fmt.Println("We are done using the phone")
}
```

## <a id='Switch-Based_State_Machine'></a>Switch-Based State Machine


```go
package state

import (
  "bufio"
  "fmt"
  "os"
  "strings"
)

type State int

const (
  Locked State = iota
  Failed
  Unlocked
)

func main() {
  code := "1234"
  state := Locked
  entry := strings.Builder{}

  for {
    switch state {
    case Locked:
      // only reads input when you press Return
      r, _, _ := bufio.NewReader(os.Stdin).ReadRune()
      entry.WriteRune(r)

      if entry.String() == code {
        state = Unlocked
        break
      }

      if strings.Index(code, entry.String()) != 0 {
        // code is wrong
        state = Failed
      }
    case Failed:
      fmt.Println("FAILED")
      entry.Reset()
      state = Locked
    case Unlocked:
      fmt.Println("UNLOCKED")
      return
    }
  }
}
```




# <a id='STRATEGY'></a><u>STRATEGY</u>

> ### **In computer programming, the strategy pattern (also known as the policy pattern) is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.**

>   ⚠️  **Use the Strategy pattern when you want to use different variants of an algorithm within an object and be able to switch from one algorithm to another during runtime.**
</br>
 ⚠️   **Use the Strategy when you have a lot of similar classes that only differ in the way they execute some behavior.**
 </br>
 ⚠️   **Use the pattern to isolate the business logic of a class from the implementation details of algorithms that may not be as important in the context of that logic.**
  </br>
 ⚠️   **Use the pattern when your class has a massive conditional statement that switches between different variants of the same algorithm.**


## <a id='Strategy'></a>Strategy

```go
package strategy

import (
  "fmt"
  "strings"
)

type OutputFormat int

const (
  Markdown OutputFormat = iota
  Html
)

type ListStrategy interface {
  Start(builder *strings.Builder)
  End(builder *strings.Builder)
  AddListItem(builder *strings.Builder, item string)
}

type MarkdownListStrategy struct {}

func (m *MarkdownListStrategy) Start(builder *strings.Builder) {

}

func (m *MarkdownListStrategy) End(builder *strings.Builder) {

}

func (m *MarkdownListStrategy) AddListItem(
  builder *strings.Builder, item string) {
  builder.WriteString(" * " + item + "\n")
}

type HtmlListStrategy struct {}

func (h *HtmlListStrategy) Start(builder *strings.Builder) {
  builder.WriteString("<ul>\n")
}

func (h *HtmlListStrategy) End(builder *strings.Builder) {
  builder.WriteString("</ul>\n")
}

func (h *HtmlListStrategy) AddListItem(builder *strings.Builder, item string) {
  builder.WriteString("  <li>" + item + "</li>\n")
}

type TextProcessor struct {
  builder strings.Builder
  listStrategy ListStrategy
}

func NewTextProcessor(listStrategy ListStrategy) *TextProcessor {
  return &TextProcessor{strings.Builder{}, listStrategy}
}

func (t *TextProcessor) SetOutputFormat(fmt OutputFormat) {
  switch fmt {
  case Markdown:
    t.listStrategy = &MarkdownListStrategy{}
  case Html:
    t.listStrategy = &HtmlListStrategy{}
  }
}

func (t *TextProcessor) AppendList(items []string) {
  t.listStrategy.Start(&t.builder)
  for _, item := range items {
    t.listStrategy.AddListItem(&t.builder, item)
  }
  t.listStrategy.End(&t.builder)
}

func (t *TextProcessor) Reset() {
  t.builder.Reset()
}

func (t *TextProcessor) String() string {
  return t.builder.String()
}

func main() {
  tp := NewTextProcessor(&MarkdownListStrategy{})
  tp.AppendList([]string{ "foo", "bar", "baz" })
  fmt.Println(tp)

  tp.Reset()
  tp.SetOutputFormat(Html)
  tp.AppendList([]string{ "foo", "bar", "baz" })
  fmt.Println(tp)
}
```

</br>
</br>


# <a id='TEMPLATE_METHOD'></a><u>TEMPLATE METHOD</u>

> ### **Template Method is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.**

>   ⚠️  **Use the Template Method pattern when you want to let clients extend only particular steps of an algorithm, but not the whole algorithm or its structure.**
</br>
 ⚠️   **Use the pattern when you have several classes that contain almost identical algorithms with some minor differences. As a result, you might need to modify all classes when the algorithm changes.**



## <a id='Template_Method'></a>Template Method


```go
package templatemethod

import "fmt"

type Game interface {
  Start()
  HaveWinner() bool
  TakeTurn()
  WinningPlayer() int
}

func PlayGame(g Game) {
  g.Start()
  for ;!g.HaveWinner(); {
    g.TakeTurn()
  }
  fmt.Printf("Player %d wins.\n", g.WinningPlayer())
}

type chess struct {
  turn, maxTurns, currentPlayer int
}

func NewGameOfChess() Game {
  return &chess{ 1, 10, 0 }
}

func (c *chess) Start() {
  fmt.Println("Starting a game of chess.")
}

func (c *chess) HaveWinner() bool {
  return c.turn == c.maxTurns
}

func (c *chess) TakeTurn() {
  c.turn++
  fmt.Printf("Turn %d taken by player %d\n",
    c.turn, c.currentPlayer)
  c.currentPlayer = (c.currentPlayer + 1) % 2
}

func (c *chess) WinningPlayer() int {
  return c.currentPlayer
}

func main() {
  chess := NewGameOfChess()
  PlayGame(chess)
}
```



## <a id='Functional_Template_Method'></a>Functional Template Method


```go
package templatemethod

import "fmt"

func PlayGame(start, takeTurn func(),
  haveWinner func()bool,
  winningPlayer func()int) {
  start()
  for ;!haveWinner(); {
    takeTurn()
  }
  fmt.Printf("Player %d wins.\n", winningPlayer())
}

func main() {
  turn, maxTurns, currentPlayer := 1, 10, 0

  start := func() {
    fmt.Println("Starting a game of chess.")
  }

  takeTurn := func() {
    turn++
    fmt.Printf("Turn %d taken by player %d\n",
      turn, currentPlayer)
    currentPlayer = (currentPlayer + 1) % 2
  }

  haveWinner := func()bool {
    return turn == maxTurns
  }

  winningPlayer := func()int {
    return currentPlayer
  }

  PlayGame(start, takeTurn, haveWinner, winningPlayer)
}
```




# <a id='VISITOR'></a><u>VISITOR</u>

> ### **Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.**

>   ⚠️  **Use the Visitor when you need to perform an operation on all elements of a complex object structure (for example, an object tree).**
</br>
 ⚠️   **Use the Visitor to clean up the business logic of auxiliary behaviors.**
 </br>
 ⚠️   **Use the pattern when a behavior makes sense only in some classes of a class hierarchy, but not in others.**



## <a id='Intrusive_Visitor'></a>Intrusive Visitor

```go
package visitor

import (
  "fmt"
  "strings"
)

type Expression interface {
  Print(sb *strings.Builder)
}

type DoubleExpression struct {
  value float64
}

func (d *DoubleExpression) Print(sb *strings.Builder) {
  sb.WriteString(fmt.Sprintf("%g", d.value))
}

type AdditionExpression struct {
  left, right Expression
}

func (a *AdditionExpression) Print(sb *strings.Builder) {
  sb.WriteString("(")
  a.left.Print(sb)
  sb.WriteString("+")
  a.right.Print(sb)
  sb.WriteString(")")
}

func main() {
  // 1+(2+3)
  e := AdditionExpression{
    &DoubleExpression{1},
    &AdditionExpression{
      left:  &DoubleExpression{2},
      right: &DoubleExpression{3},
    },
  }
  sb := strings.Builder{}
  e.Print(&sb)
  fmt.Println(sb.String())
}
```

## <a id='Reflective_Visitor'></a>Reflective Visitor


```go
package visitor

import (
  "fmt"
  "strings"
)

type Expression interface {
  // nothing here!
}

type DoubleExpression struct {
  value float64
}

type AdditionExpression struct {
  left, right Expression
}

func Print(e Expression, sb *strings.Builder) {
  if de, ok := e.(*DoubleExpression); ok {
    sb.WriteString(fmt.Sprintf("%g", de.value))
  } else if ae, ok := e.(*AdditionExpression); ok {
    sb.WriteString("(")
    Print(ae.left, sb)
    sb.WriteString("+")
    Print(ae.right, sb)
    sb.WriteString(")")
  }

  // breaks OCP
  // will work incorrectly on missing case
}

func main() {
  // 1+(2+3)
  e := &AdditionExpression{
    &DoubleExpression{1},
    &AdditionExpression{
      left:  &DoubleExpression{2},
      right: &DoubleExpression{3},
    },
  }
  sb := strings.Builder{}
  Print(e, &sb)
  fmt.Println(sb.String())
}
```


## <a id='Classic_Visitor'></a>Classic Visitor

```go
package visitor

import (
  "fmt"
  "strings"
)

type ExpressionVisitor interface {
  VisitDoubleExpression(de *DoubleExpression)
  VisitAdditionExpression(ae *AdditionExpression)
}

type Expression interface {
  Accept(ev ExpressionVisitor)
}

type DoubleExpression struct {
  value float64
}

func (d *DoubleExpression) Accept(ev ExpressionVisitor) {
  ev.VisitDoubleExpression(d)
}

type AdditionExpression struct {
  left, right Expression
}

func (a *AdditionExpression) Accept(ev ExpressionVisitor) {
  ev.VisitAdditionExpression(a)
}

type ExpressionPrinter struct {
  sb strings.Builder
}

func (e *ExpressionPrinter) VisitDoubleExpression(de *DoubleExpression) {
  e.sb.WriteString(fmt.Sprintf("%g", de.value))
}

func (e *ExpressionPrinter) VisitAdditionExpression(ae *AdditionExpression) {
  e.sb.WriteString("(")
  ae.left.Accept(e)
  e.sb.WriteString("+")
  ae.right.Accept(e)
  e.sb.WriteString(")")
}

func NewExpressionPrinter() *ExpressionPrinter {
  return &ExpressionPrinter{strings.Builder{}}
}

func (e *ExpressionPrinter) String() string {
  return e.sb.String()
}

func main() {
  // 1+(2+3)
  e := &AdditionExpression{
    &DoubleExpression{1},
    &AdditionExpression{
      left:  &DoubleExpression{2},
      right: &DoubleExpression{3},
    },
  }
  ep := NewExpressionPrinter()
  ep.VisitAdditionExpression(e)
  fmt.Println(ep.String())
}
```













