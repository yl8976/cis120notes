# CIS 120 Notes

## Chapter 1 - Overview and Program Design
### The Four Stages of Program Design
1. Understand the problem.
2. Formalize the interface.
3. Write test cases.
4. Implement the required behavior.

## Chapter 2 - Introductory OCaml
### Primitives (a.k.a. atomic values)
- Integers (`int`)
  - `x / y` - integer division
  - `x mod y` - modulus (remainder)
  - `string_of_int x` - convert integer `x` to string `"x"`
- Booleans (`bool`)
  - `not` - logical negation
  - `&&` and `||` - "and" and "or"
- Strings (`string`)
  - `"\n"` - newline character
  - `"hello" ^ " world"` - concatenation

### Generic Comparisons (produces a bool)
- `<`, `<=`, `>`, `>=` are used for inequality.
- `=` and  `<>` are used for **structural equality**.
- `==` and `!=` are used for **referential equality**.

### Conditionals
- `if x then y else z`
- `else` statement is **required** in OCaml.

### Variables
- `let x : type = y in`
- `x` is called the _identifier_.
- `in` is **required** if and only if the variable's scope is local.
- Shadowing may occur if variable is declared again.
- The variable with the innermost scope is substituted first when evaluating the expression.

### Functions
- `let f (x: type) (y: type): returnType = z in`
- `in` is **required** if and only if the function's scope is local.

### Comments
- `(* Insert comment here *)`

### Failwith
- `failwith "Error string"`
- Can be used as placeholder text.
- Terminates the program, so calling it too early will stop the rest of the code from being executed.

### Commands
- `;; print_string "Hello World\n"`
- `;; print_endline "Hello World"`
- `;; print_int 3`
- Running Tests
``` ruby
let test() : bool =
  (1 + 2 + 3) = 7
;; run_test "1 + 2 + 3" test
```

### Importing Files
- `;; open FileName`

### Miscellaneous
- OCaml is _value-oriented_, meaning that everything computes to a value.
- It is also a _strongly-typed_ programming language, meaning that every expression has a type. An expression is _well-typed_ if it has at least one type, and _ill-typed_ otherwise.

## Chapter 3 - Lists and Recursion
### Lists
- Lists are just ordered sequences of data values.
- Lists are either:
  - `[]` - the empty list (sometimes called `nil`)
  - `v :: tail` - a value `v` followed by `tail`, a list of the remaining elements
- `::` constructs a non-empty list (hence called "cons")
  - Side note: `::` is therefore of type `'a -> 'a list -> 'a list` (I think?)
- `[1; 2; 3]` and `1 :: 2 :: 3 :: []` are equivalent. The former is just shorthand for the latter.
- You can have lists of lists.

### Pattern Matching on Lists
Example 1: Simple Pattern Matching
``` ruby
let l : int list = [1;2;3]
let is_l_empty : bool =
  begin match l with
  | [] -> true
  | x :: tl -> false
  end
```

Example 2: Recursive Pattern Matching
``` ruby
let rec length (l : int list) : int =
  begin match l with
  | [] -> 0
  | x :: tl -> 1 + (length tl)
  end
```
(The `rec` keyword must be used if the function is recursive (i.e. calls on itself)).

Example 3: Example 2 Generalized
``` ruby
let rec f (l : ... list) ... : ... =
  begin match l with
  | [] -> ...
  | (hd :: rest) -> ... hd ... (f rest) ...
  end
```

## Chapter 4 - Tuples and Nested Patterns
### Tuples
- Tuples are good when values are **not** all of the same type.
- Tuple types are denoted by the infix `*` character.
  - E.g. `(1, "uno") : int * string`
- Tuples can be pattern matched:
``` ruby
let first (x:int * string) : int =
  begin match x with
  | (left, right) -> left
  end
```

or:

``` ruby
let first (x:int * string) : int =
  let (left, right) = x in left
```

- `()` is the empty tuple and has type `unit`.

### Nested Patterns
- Patterns are matched _in the other they appear_.
- Cases must be _exhaustive_; otherwise you may get a `Match_failure` error.
- Wildcards (represented by `_`) can be used as either:
  - a catch-all case, or
  - as a component of a match case that you don't need, e.g. the head of a list if you only use the tail.

Example using nested patterns and wildcards:
``` ruby
let rec zip (l1 : int list) (l2 : string list) : (int * string) list =
begin match (l1, l2) with (* note the tuple here! *)
  | ([], []) -> []
  | (x :: xs, y :: ys) -> (x,y) :: (zip xs ys)
  | _ -> failwith "zip called on unequal-length lists"
end
```

## Chapter 5 - User-defined Datatypes
``` ruby
type day =
  | Sunday
  | Monday
  | Tuesday
  | Wednesday
  | Thursday
  | Friday
  | Saturday
```
- In OCaml, user-defined types must be lowercase, and the constructors for such types must be uppercase identifiers.


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
