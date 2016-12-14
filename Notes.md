# CIS 120 Notes

## Chapter 1 - Overview and Program Design
### The Four States of Program Design
1. Understand the problem.
2. Formalize the interface.
3. Write test cases.
4. Implement the required behavior.

## Chapter 2 - Introductory OCaml
### Primitives
- Integers (`int`)
  - `x / y` - integer division
  - `x mod y` - modulus (remainder)
  - `string_of_int x` - convert `x` to string `"x"`
- Booleans (`bool`)
  - `not` - logical negation
  - `&&` and `||` - "and" and "or"
- Strings (`string`)
  - `"\n"` - newline character
  - `"hello" ^ " world"` - concatenation

### Generic Comparisons (produces a bool)
- `=`, `<>`, `<`, `<=`, `>`, `>=`, `==`, `!=`

### Conditionals
- `if x then y else z`
- `else` statement is **required** in OCaml.

### Variables
- `let x : type = y in`
- `x` is called the _identifier_.
- `in` is **required** iff the variable's scope is local.
- Shadowing may occur if variable is declared again.
- The variable with the innermost scope is substituted first when evaluating the expression.

### Functions
- `let f (x: type) (y: type): returnType = z in`
- `in` is **required** iff the function's scope is local.

### Failwith
- `failwith "Error string"`
- Can be used as placeholder text.
- Terminates the program, so calling it too early will stop the rest of the code from being executed.

### Commands
- `;; print_string "Hello World\n"`
- `;; print_endline "Hello World"`
- `;; print_int 3`
- Running Tests
``` Ruby
let test() : bool =
  (1 + 2 + 3) = 7
;; run_test "1 + 2 + 3" test
```

### Miscellaneous
- OCaml is _value-oriented_, meaning that everything computes to a value.
- It is also a _strongly-typed_ programming language, meaning that every expression has a type. An expression is _well-typed_ if it has at least one type, and _ill-typed_ otherwise.

## Chapter 3 - Lists and Recursion

## Chapter 4 - Tuples and Nested Patterns

## Chapter 5 - User-defined Datatypes

## Chapter 6 - Binary Trees

## Chapter 7 - Binary Search Trees

## Chapter 8 - Generic Functions and Datatypes

## Chapter 9 - First-class Functions

## Chapter 10 - Modularity and Abstraction

## Chapter 11 - Partial Functions: Option Types

## Chapter 12 - Unit and Sequencing Commands

## Chapter 13 - Records of Named Fields

## Chapter 14 - Mutable State and Aliasing

## Chapter 15 - The Abstract Stack Machine

## Chapter 16 - Linked Structures: Queues

## Chapter 17 - Local State

## Chapter 18 - Wrapping Up OCaml: Designing a GUI Library

## Chapter 19 - Transition to Java

## Chapter 20 - Connecting OCaml to Java

## Chapter 21 - Arrays

## Chapter 22 - The Java ASM

## Chapter 23 - Subtyping, Extension and Inheritance

## Chapter 24 - The Java ASM and Dynamic Methods

## Chapter 25 - Generics, Collections, and Iteration

## Chapter 26 - Overriding and Equality

## Chapter 27 - Exceptions

## Chapter 28 - I/O

## Chapter 29 - Swing: GUI Programming in Java

## Chapter 30 - Swing: Layout and Drawing

## Chapter 31 - Interaction and Paint Demo

## Chapter 32 - Java Design Exercise: Resizable Arrays

## Chapter 33 - Encapsulation and Queues
