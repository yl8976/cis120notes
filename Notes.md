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
  - `string_of_int x` - convert integer `x` to string `x`
- Booleans (`bool`)
  - `not` - logical negation
  - `&&` and `||` - and and or
- Strings (`string`)
  - `\n` - newline character
  - `hello ^  world` - concatenation

### Generic Comparisons (produces a bool)
- `<`, `<=`, `>`, `>=` **are used for inequality**.
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
- `failwith Error string`
- Can be used as placeholder text.
- Terminates the program, so calling it too early will stop the rest of the code from being executed.

### Commands
- `;; print_string Hello World\n`
- `;; print_endline Hello World`
- `;; print_int 3`
- Running Tests
``` ruby
let test() : bool =
  (1 + 2 + 3) = 7
;; run_test 1 + 2 + 3 test
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
- `::` constructs a non-empty list (hence called cons)
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
  - E.g. `(1, uno) : int * string`
- Tuples can be pattern matched:
``` ruby
let first (x : int * string) : int =
  begin match x with
  | (left, right) -> left
  end
```

or:

``` ruby
let first (x : int * string) : int =
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
  | _ -> failwith zip called on unequal-length lists
end
```

## Chapter 5 - User-defined Datatypes
### Defining a New Datatype
``` javascript
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
- They are analogous to enums in Java.

### Datatypes with Extra Information
``` ruby
type nucleotide =
  | A (* adenine *)
  | C (* cytosine *)
  | G (* guanine *)
  | T (* thymine *)

type experiment =
  | Missing
  | NucCount of nucleotide * int
  | CodonCount of (nucleotide * nucleotide * nucleotide) * int
```

In general:
``` ruby
type typeName =
  | Constructor1
  | Constructor2 of extraDataType
```

Example values:
``` ruby
Missing
NucCount(A, 10)
CodonCount((A, C, T), 23)
```
Note that you can also pattern match on these custom datatypes.

### Type Abbreviations
Existing types can be used to define new types:
``` javascript
type codon = nucleotide * nucleotide * nucleotide
type helix = nucleotide list
```

### Recursive Types
Types can be recursive:
``` ruby
type my_string_list =
| Nil
| Cons of string * my_string_list
```

## Chapter 6 - Binary Trees
A **binary tree** is either `Empty` or it is a `Node` consisting of a left subtree, an integer label, and a right subtree.

``` ruby
type tree =
  | Empty
  | Node of tree * int * tree
```

- The root node is at the top of the tree.
- A leaf is a node both of whose children are `Empty`.
- Any node that is not a leaf is sometimes called an internal node of the tree.
- The size of a tree is the total number of nodes in a tree.
- A tree's height is the length of the longest path from the root to any leaf.

### Traversing Trees
- In-order traversal: left child, node, right child
- Pre-order traversal: node, left child, right child
- Post order traversal: left child, right child, node


## Chapter 7 - Binary Search Trees
### The Binary Search Tree Invariant
- `Empty` is a binary search tree.
- A tree `Node(lt, x, rt)` is a binary search tree if `lt` and `rt` are both binary search trees, and every label of `lt` is less than `x` and every label of `rt` is greater than `x`.

One way to construct a binary search tree is to start with a simple binary search tree like `Empty`, which trivially satisfies the invariant, and then `insert` or `delete` nodes as desired to obtain a new binary search tree.

Example: inserting an element into a binary search tree
``` ruby
let rec insert (t: tree) (n: int) : tree =
  begin match t with
  | Empty -> Node(Empty, n, Empty)
  | Node(lt, x, rt) -> if x = n then t
                       else
                         if n < x then Node (insert lt n, x, rt)
                       else Node(lt, x, insert rt n)
```

Example: removing an element in a binary search tree
``` ruby
let rec tree_max (t : tree) : int =
  begin match t with
  | Empty -> failwith tree_max called on empty tree
  | Node(_, x, Empty) -> x
  | Node(_, _, rt) -> tree_max rt
  end

let rec delete (n : int) (t : tree) : tree =
  begin match t with
  | Empty -> Empty
  | Node(lt, x, rt) ->
      if x = n then
        begin match (lt, rt) with
        | (Empty, Empty) -> Empty
        | (Node _, Empty) -> lt
        | (Empty, Node _) -> rt
        | _ -> let m = tree_max lt in
        Node(delete m lt, m, rt) end
      else
        if n < x then Node(delete n lt, x, rt)
        else Node(lt, x, delete n rt)
  end
```

## Chapter 8 - Generic Functions and Datatypes
### Generic Functions
- `'a` is a _type variable_, meaning that it can be instantiated to any type.
- Type variables must agree all times they are named in a function. i.e., if `'a` is mentioned twice as inputs, the inputs (whatever they end up being) must be of the same type.

## Chapter 9 - First-class Functions
- Functions _are_ values in OCaml.

Example: twice function
``` javascript
let twice (f : int -> int) (x : int) : int =
  f (f x)
```

### Anonymous Functions
``` javascript
fun (x : int) (y : int) -> x + y
```

Named version:
``` javascript
let sum : int -> int -> int = fun (x : int) (y : int) -> x + y
```

### List Transform
```
let rec transform (f: 'a -> b) (l: 'a list) : 'b list =
  begin match l with
  | [] -> []
  | h :: tl -> (f h) :: (transform f tl)
  end
```

## Chapter 10 - Modularity and Abstraction
The Set Interface
```
module type Set = sig (* A named interface called Set *)

  type 'a set (* 'a is the element type *)

  val empty : 'a set
  val add : 'a -> 'a set -> 'a set
  val union : 'a set -> 'a set -> 'a set
  val remove : 'a -> 'a set -> 'a set
  val list_to_set : 'a list -> 'a set
  val is_empty : 'a set -> bool
  val member : 'a -> 'a set -> bool
  val equal : 'a set -> 'a set -> bool
  val elements : 'a set -> 'a list
end
```

Set Implementation
```
module LSet : Set = struct
  (* inside the module we represent sets as lists *)
  (* INVARIANT: the list contains no duplicates *)

  type 'a set = 'a list

  let empty : 'a set =
    []

  let rec member (x : 'a) (s : 'a set) : bool =
    begin match s with
    | [] -> false
    | y :: rest -> x = y || member x rest
    end

  let add (x : 'a) (s : 'a set) : 'a set =
    if (member x s) then s
    else x :: s

  let rec remove (x : 'a) (s : 'a set) : 'a set =
    begin match s with
    | [] -> []
    | y :: rest ->
      if x = y then rest
      else y :: (remove x rest)
    end

  ... (* implement the rest of the operations *)

end
```

The Map Interface
```
module type Map = sig

  type ('k,'v) map

  val empty : ('k, 'v) map
  val add : 'k -> 'v -> ('k, 'v) map -> ('k, 'v) map
  val remove : 'k -> ('k, 'v) map -> ('k, 'v) map
  val mem : 'k -> ('k, 'v) map -> bool
  val get : 'k -> ('k, 'v) map -> 'v
  val entries : ('k, 'v) map -> ('k * 'v) list
  val equals : ('k, 'v) map -> ('k, 'v) map -> bool
end
```

Find the map implementation in pages 107-108 of the textbook.

## Chapter 11 - Partial Functions: Option Types
The option datatype
```
type 'a option =
| None
| Some of 'a
```

## Chapter 12 - Unit and Sequencing Commands
### Types of OCaml Commands
``` javascript
print_string : string -> unit
print_endline : string -> unit
print_int : int -> unit
run_test : string -> (unit -> bool) -> unit
```

- Can define values of type unit
- Can pattern match unit
- Unit is the result of an implicit branch

### Using ";"
- In OCaml programs (but not the top-level loop), `;` is always a separator, not a terminator. The list below collects together all the syntax combinations that use `;`:
  - `;; open Assert` open a module at the top-level of a program
  - `;; print_int 3` run a command at the top-level of a program
  - `[1; 2; 3]` separate the elements of a list
  - `e1; e2` sequence a command e1 before the expression e2
  - `{x : int; y : int}` separate the fields of a record type
  - `{x = 3; y = 4}` separate the fields of a record value
We have also seen that, when executing OCaml expressions in the top-level interactive loop, we use `;;` as a terminator to let OCaml know when to run a given expression.

## Chapter 13 - Records of Named Fields
Example
``` javascript
type rgb = { r : int; g : int; b : int }
let red : rgb = { r = 255; g = 0; b = 0 }
let cyan = { blue with g = 255 }

type employee = {
  name : string;
  age : int;
  salary : int;
  division : string
}
```


## Chapter 14 - Mutable State and Aliasing
Example
``` javascript
type state = { mutable count : int }

let global : state = { count = 0 }

let rec tree_max3 (t : a tree) : a =
  global.count <- global.count + 1; (* update the count *)
  begin match t with
  | Empty -> failwith "tree_max called on empty tree"
  | Node(_, x, Empty) -> x
  | Node(_, _, rt) -> tree_max3 rt
  end
```

## Chapter 15 - The Abstract Stack Machine
There are three basic parts of the abstract stack machine model:
  - The _workspace_ keeps track of the expression or command that the computer is currently simplifying. As the program evaluates, the contents of the workspace change to reflect the progress being made by the computation.
  - The _stack_ keeps track of a sequence of bindings that map identifiers to their values. New bindings are added to the stack when the `let` expression is simplified. Later, when an identifier is encountered during simplification, its associated value can be found by looking in the stack. The stack also keeps track of partially simplified expressions that are waiting for the results of function calls to be computed.
  - The _heap_ models the computer’s memory, which is used for storage of non-primitive data values. It specifies (abstractly) where data structures reside, and shows how they reference one another.

The _stack_ only contains two types of values: **primitives** and **references to the _heap_**.

The _heap_ contains three types of data:
  1. A _cell_ is labeled by a datatype constructor (such as `Cons` or `Nil`) and contains a sequence of constructor arguments. Constructor names also take up some space in the heap.
  2. A _record_ contains a value for each of its fields. Their field names **don't** take up space in the heap.
  3. A _function_, which is just an anonymous function value of the form `(fun (x1:t1) ... (xn:tn) -> e)`.

Variables in the stack are looked up recent-first. i.e., it is done in a last-in-first-out (LIFO) manner.

- _Structural equality_, written =, is the most appropriate equality operator for comparing immutable data structures. This operator recursively traverses the structure of data to determine whether its arguments are equal.

- _Referential equality_, written ==, is the most appropriate equality for mutable data structures. This operator only looks at heap locations, so equates fewer things than structural equality.

## Chapter 16 - Linked Structures: Queues
The Queue Interface
```
module type QUEUE = sig
  (* type of the data structure *)
  type 'a queue

  (* Make a new, empty queue *)
  val create : unit -> 'a queue

  (* Determine if the queue is empty *)
  val is_empty : 'a queue -> bool

  (* Add a value to the tail of the queue *)
  val enq : 'a -> 'a queue -> unit

  (* Remove the head value and return it (if any) *)
  val deq : 'a queue -> 'a
end
```

Queue Implementation
```
module Q : QUEUE = struct

  type 'a qnode = {
    v : 'a;
    mutable next : 'a qnode option;
  }

  type 'a queue = {
  mutable head : 'a qnode option;
  mutable tail : 'a qnode option;
  }

  let enq (x : 'a) (q : 'a queue) : unit =
    let newnode = {v=x; next=None} in
    begin match q.tail with
    | None ->
      (* Note that the invariant tells us that q.head is also None *)
      q.head <- Some newnode;
      q.tail <- Some newnode
    | Some n ->
      n.next <- Some newnode;
      q.tail <- Some newnode
    end

  let deq (q : 'a queue) : 'a =
    begin match q.head with
    | None ->
      failwith "deq called on empty queue"
    | Some n ->
      q.head <- n.next;
      if n.next = None then q.tail <- None;
      n.v
    end

  let length (q : 'a queue) : int =
    let rec loop (no : 'a qnode option) (len : int) : int =
      begin match no with
      | None -> len
      | Some n -> loop n.next (1 + len) (* Using iteration *)
      end
    in
    loop q.head 0

  let print (q:'a queue) (string_of_element:'a -> string) : unit =
    let rec loop (no: 'a qnode option) : unit =
      begin match no with
      | None -> ()
      | Some n -> print_endline (string_of_element n.v);
      loop n.next
      end
    in
    print_endline "--- queue contents ---";
    loop q.head;
    print_endline "--- end of queue -----"

  let to_list (q: 'a queue) : 'a list =
    let rec loop (no: 'a qnode option) (l:'a list) : 'a list =
      begin match no with
      | None -> List.rev l
      | Some n -> loop n.next (n.v::l)
      end
    in loop q.head []

  let sum (q: int queue) : int =
    let rec loop (no: int qnode option) (sum:int) : int =
      begin match no with
      | None -> sum
      | Some n -> loop n.next (sum + n.v)
      end
    in loop q.head 0

end
```

### The Queue Invariant
A data structure of type `'a queue` satisfies the queue invariants if (and only if), either
  1. `both` `head` and `tail` are `None`, or,
  2. `head` is `Some n1` and `tail` is `Some n2`, and
    - `n2` is reachable by following `next` pointers from `n1`
    - `n2.next` is `None`

### Iteration and Tail Call Optimization
- A function call that would result in pushing an empty workspace is said to be in a _tail call_ position.
- The ASM optimizes such functions as described above— it doesn’t push the (empty) workspace, and it eagerly pops off stack bindings. This process is called _tail call optimization_, and it is frequently used in functional programming.
-  It effectively turns _recursion_ into _iteration_. Imperative programming languages include `for` and `while` loops that are used to iterate a fragment of code multiple times. The essence of such iteration is that only a **constant amount of stack space is used** and that parts of the program state are updated each time around the loop, both to accumulate an answer and to determine when the loop should finish.
- Using a `loop` using tail calls in OCaml is **equivalent** to writing a `while` loop in Java.
- Infinite loops created within functions that use iteration may loop silently, as it does not create stack overflow errors.

## Chapter 17 - Local State

``` javascript
type counter = {
  incr : unit -> unit;
  decr : unit -> unit;
  get : unit -> int;
  reset : unit -> unit;
}

let mk_counter () : counter =
  let ctr : state = {count = 0} in
  {
  incr = (fun () -> ctr.count <- ctr.count + 1);
  decr = (fun () -> ctr.count <- ctr.count - 1);
  get = (fun () -> ctr.count);
  reset = (fun () -> ctr.count <- 0);
  }

let ctr1 : counter = mk_counter () in
let ctr2 : counter = mk_counter () in
;; ctr1.incr ();
;; ctr2.incr ();
;; ctr1.incr ();
let ans : int = ctr1.get ()
```

### `'a refs`
`type 'a ref = {mutable contents : 'a}`
`ref e` means `{contents = e}`
`!e` means `e.contents`
`e := v` means `e.contents <- v`

## Chapter 18 - Wrapping Up OCaml: Designing a GUI Library

## Chapter 19 - Transition to Java
### Interfaces in Java
``` java
public interface Displaceable {
  public int getX ();
  public int getY ();
  public void move(int dx, int dy);
}

public void moveItALot(Displaceable s) {
  s.move(3,3);
  s.move(100, 1000);
  s.move(s.getX(), s.getY());
}

public class Point implements Displaceable {
  private int x, y;

  public Point(int x0, int y0) {
    x = x0;
    y = y0;
  }

  public int getX() { return x; }
  public int getY() { return y; }

  public void move(int dx, int dy) {
    x = x + dx;
    y = y + dy;
  }
}

class ColorPoint implements Displaceable {
  private Point p;
  private Color c;

  ColorPoint (int x0, int y0, Color c0) {
    p = new Point(x0,y0);
    c = c0;
  }

  public void move(int dx, int dy) {
    p.move(dx, dy);
  }

  public int getX() { return p.getX(); } // Delegates the implementation to the Point object p

  public int getY() { return p.getY(); }

  public Color getColor() { return c; }
}
```

## Chapter 20 - Connecting OCaml to Java
### Java Primitives
``` java
int // standard integers
byte, short, long // other flavors of integers
char // unicode characters
float, double // floating-point numbers
boolean // true and false
```

Unlike OCaml, some of Java's operations are **overloaded**; i.e., they can be applied to multiple types. It will also automatically convert numeric types to another:
``` java
4 / 3 == 1
4.0 / 3.0 == 1.3333333333333333
4 / 3.0 == 1.3333333333333333
```
### Primitive Operations in OCaml vs. Java
|  OCaml             |  Java               | Description         |
|:------------------:|:-------------------:|:------------------- |
|  `=` `==`          |  `==`               | Equality Test       |
|  `<>` `!=`         |  `!=`               | Inequality          |
|  `<` `<=` `>` `>=` |  `<` `<=` `>` `>=`  | Comparisons         |
|  `+`               |  `+`                | Addition            |
|  `-`               |  `-`                | Subtraction         |
|  `/`               |  `/`                | Division            |
|  `*`               |  `*`                | Multiplication      |
|  `mod`             |  `%`                | Remainder (Modulus) |
|  `not`             |  `!`                | Logical "not"       |
|  `&&`              |  `&&`               | Logical "and"       |
|  &#124;&#124;      |  &#124;&#124;       | Logical "or"        |

### Dynamic vs. Static Methods
- Dynamic dispatch is when the method invocation “dispatches” to some version of a method depending on the _dynamic_ class of the object.
  - A dynamic method's behavior can't be determined until the program is actually run.
- On the other hand, which `static` methods (and fields) are invoked when called can be determined at _compile time_.
  - Use the class name to call on a `static` method (e.g. if `C` is a class, and `m()` is a `static` method within `C`, use `C.m()`).
  - A `static` method **cannot access non-static fields or call non-static methods** associated with the class.
  - `static` methods are good for implementing functions that **don't depend on any objects' states**.

## Chapter 21 - Arrays
The largest legal array index for a given array `a[]` is always `a.length - 1`.

Array elements are mutable:
``` java
a[i] = v;
```

Instantiating a new array:
``` java
int[] arr = new int[10];
```

Or, if you know the values already:
``` java
int[] arr = { 1, 2, 3 };
Point[] pointArray = { new Point(1,3), new Point(5,4) };
```

### For and While Loops
``` java
for (int i = 1; i < 5; i++) { // loop guard and index update
  i;
}

while (i < 5) { // loop guard
  i;
  i++; // loop index update
}
```

### 2-D Arrays
Conventionally, 2-D arrays go _row then column_: `arr[row][column]`.

Iterate over 2-D arrays by the following:
``` java
for (int r = 0; r < arr.length; r++){
// use the length of the inner array
  for (int c = 0; c < arr[r].length; c++) {
    arr[r][c] = ...
  }
}
```

Some notes about arrays:
  - Make sure you don't use aliases when storing objects inside arrays.
  - The array length is _never_ mutable.
  - The array's elements are _always_ mutable.

## Chapter 22 - The Java ASM
### Differences between the OCaml and Java ASMs
- Almost everything, including variables stored on the _stack_, is **mutable**.
- _Heap_ values include **only arrays and objects**. Java does **not** include lists, options, tuples, other datatypes, records or first-class functions.
- Java includes a special reference called `null`.
- Method bodies are stored in an auxiliary component called a class table.

### The Java ASM
- Only the name of the class and field members are part of the heap; the constructors and methods are stored in the class table.
- Mutable fields are indicated by **bolded boxes**.

## Chapter 23 - Subtyping, Extension and Inheritance
- Java is also a strongly-typed language; every expression can be given a type.
- A Java class is _also_ a type.
- Both _interfaces_ and _classes_ can implement multiple interfaces.
- An _interface_ can extend multiple interfaces, but a _class_ can **only extend one other class** (otherwise it extends `Object`, implicitly).
- An extended class does not, by default, have access to the superclass's `private` methods and fields; however, the `protected` keyword can be used to make a method or field visible within a class and all of its subclasses, no matter where they are defined.
- The `super` keyword, which can be invoked as a method, is used to call a superclass's constructor. (It is implicitly called to extend the superclass `Object` if the class does not explicitly extend another class.)
- Class hierarchies form trees, while interfaces don't necessarily.
- The `Object` class provides the `toString` (object to `String` representation) and `equals` (structural equality) methods.

### Static Types and Dynamic Classes
- The static type determines which methods can be used.
- The dynamic type determines which method will be invoked at run time.
- The dynamic type must be equal to or be a subtype of the static type (unless the static type is an interface or an abstract class, in which case the dynamic type must be a class that is a subtype of the static type).

## Chapter 24 - The Java ASM and Dynamic Methods
Invoking a constructor:
  - allocates space for a new object in the heap,
  - includes slots for _all_ fields of _all_ ancestors in the class tree,
  - creates a pointer to the class (this is the object's dynamic type),
  - calls its superclass constructor _first_, even if it's not explicitly done with `super`,
  - and runs the constructor body after pushing parameters and `this` onto the stack.

Fields start with a "sensible" default - `0` for numeric values and `null` for references.

### Dynamic Dispatch (in more detail)
- When an object's method is invoked, as in `o.m()`, the code that runs is determined by `o`’s dynamic class.
- The dynamic class, which is just a pointer to a class, is included in the object structure in the heap.
- If the method is inherited from a superclass, determining the code for `m` might require searching up the class hierarchy via pointers in the class table.
- Once the code for `m` has been determined, a binding for `this` is pushed onto the stack. The `this` pointer is used to resolve field accesses and method invocations inside the code.
- This process is called dynamic dispatch.

### Miscellaneous Notes
- `static` fields are stored in the _Class Table_.
  - The best use for `static` fields are for constants (e.g. `Math.PI`).
- `static` methods do not access to a `this` pointer while executing. This means they cannot refer to fields in a class or call non-static methods directly. However, they can still create a new object and call the non-static methods using that object.

## Chapter 25 - Generics, Collections, and Iteration
### Generics
- When subtyping with generics, the type parameter must be _invariant_. i.e., Even if `B` is a subtype of `A`, `Queue<B>` is not necessarily a subtype of `Queue<A>`.

### Collections
- `Collection` is extended by `List`, `Deque`, and `Set` interfaces. `Map` is on its own.
  - The `Collection` interface's `contains` and `remove` methods take in `Object` objects since they only use the `.equals` method to check for equality.
- `LinkedList`s are like deques.
- `ArrayList`s and `ArrayDeque`s are like resizable arrays.
- `TreeSet` and `TreeMap` are BST-based implementations.
- `HashSet` and `HashMap` are hashing-based implementations.

### Iterating Over Collections
- All `Collection`s (_and arrays!_) are subtypes of `Iterable`.
- `Iterable` objects provides access to an `Iterator` for the object.

The `Iterable<E>` interface:
``` java
interface Iterable<E> {
  public Iterator<E> iterator();
}
```

The `Iterator<E>` interface:
``` java
interface Iterator<E> {
  public boolean hasNext();
  public E next();
  public void delete(); // optional
}
```

Using `Iterator`s
``` java
List<Book> shelf = ... // create a list of Books

// iterate through the elements on the shelf
Iterator<Book> iter = shelf.iterator();
while (iter.hasNext()) {
  Book book = iter.next();
  catalogue.addInfo(book);
  numBooks = numbooks++;
}
```

Alternatively, you can use a for-each loop:
``` java
// iterate through the elements on a shelf
for (Book book : shelf) {
  catalogue.addInfo(book);
  numBooks = numbooks++;
}
```

### All	collections	use	`equals`
- Defaults to `==` (reference equality)
- Override `equals` to create structural equality
- Should be: `false` for distinct instance classes
- An equivalence relation: reflexive, symmetric, transitive

### `HashSets`/`HashMaps` use `hashCode`
- Override when `equals` is overridden
- Should be compatible with `equals`
- Should try to "distribute" the values uniformly
- Iterator not guaranteed to follow element order

### Ordered collections (`TreeSet`, `TreeMap`) need to implement `Comparable<Object>`
- Override `compareTo`
- Should implement a total order
- Strongly recommended to be compatible with `equals` (i.e. `o1.equals(o2)` exactly when `o1.compareTo(o2) == 0`)

## Chapter 26 - Overriding and Equality
Consider the following:
``` java
class C {
  public void printName() {
    System.out.println("I'm a " + getName());
  }

  public String getName() {
    return "C";
  }
}

class E extends C {
  public String getName() {
    return "E";
  }
}

// in main
C c = new E();
c.printName();
// The string "I'm a E" is printed to the console
```

- `"I'm a E"` is printed because the **dynamic class of the method determines every method invocation**.
- Class authors can prevent subclasses from overriding their methods by using the `final` modifier. This prevents subclasses from overriding that particular method.
- `equals` is a method that is frequently overridden, usually when the class represents immutable values (e.g. the `Point` and `String` classes).

### Conditions for Equality
- It is reflexive: for any non-`null` reference value `x`, `x.equals(x)` should return true.
- It is symmetric: for any non-`null` reference values `x` and `y`, `x.equals(y)` should return true if and only if `y.equals(x)` returns true.
- It is transitive: for any non-`null` reference values `x`, `y`, and `z`, if `x.equals(y)` returns true and `y.equals(z)` returns true, then `x.equals(z)` should return true.
- It is consistent: for any non-`null` reference values `x` and `y`, multiple invocations of `x.equals(y)` consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
- For any non-`null` reference value `x`, `x.equals(null)` should return false.

### Using `instanceof`
``` java
Point p = new Point(1,2);
Object o1 = p;
Object o2 = "hello";
p instanceof Point == true
o1 instanceof Point == true
o2 instanceof Point == false
p instanceof Object == true
null instanceof Object == false // null is not an instance of any class
```

### Example Use of Overriding `.equals`
``` java
@Override
  public boolean equals(Object o) {
  if (o == null) return false;
  if (this == o) return true;
  if (!(getClass() == o.getClass())) return false;
  Point that = (Point) o;
  if (x != that.x) return false;
  if (y != that.y) return false;
  return true;
}
```

## Chapter 27 - Exceptions
- Exceptions that are subtypes of `Exception` but not `RuntimeException` are called checked or declared.
- Subtypes of `RuntimeException` are unchecked and do not need to be declared.
    - `NullPointerException`
    - `IndexOutOfBoundsException`
    - `IllegalArgumentException`
- Use declared exceptions for libraries, where documentation and usage enforcement are critical.
- Use undeclared exceptions in client code to facilitate more flexible development.

## Chapter 28 - I/O
- The stream abstraction represents a communication channel with the outside world.
- Data items are read from or written to a stream one at a time.
- At the lowest level, a stream is a sequence of binary numbers.

### `InputStream` and `OutputStream`
- Abstract classes that provide basic operations for the Stream class hiearchy
- Read and write `int` values that represent _bytes_ in the range `0-255`
- `-1` represents no more data

### `BufferedInputStream`
- Presents the same interface to clients, but internally reads many bytes at once to a buffer.
- Result: lower overhead

### Standard Java Streams
- Byte streams
- `System.in` - standard input (keyboard)
- `System.out` - standard output (display)
- `System.err` - standard error (display)

### `Reader` and `Writer`
- Read and write `int` values that represent _unicode characters_
- Read and returns an integer in the range 0 to 65535 (16 bits)
- `-1` represents no more data
- Requires an encoding
- Note that `System.in`, `System.out`, `System.err` are byte streams, so need to be wrapped in an InputStreamReader / PrintWriter
if you need unicode console I/O

## Chapter 29 - Swing: GUI Programming in Java
### Terminology Overview
|                    |  OCaml                                              |  Java                                            |
|:------------------:|:---------------------------------------------------:|:------------------------------------------------:|
| Graphics Context   | `Gctx.gctx`                                         | Graphics                                         |
| Widget Type        | `Widget.widget`                                     | `JComponent`                                     |
| Basic Widgets      | button, label, checkbox                             | `JButton`, `JLabel`, `JCheckBox`                 |
| Container Widgets  | hpair, vpair                                        | `JPanel`, Layouts                                |
| Events             | event                                               | `ActionEvent`, `MouseEvent`, `KeyEvent`          |
| Event Listener     | mouse_listener, mouseclick_listener (event -> unit) | `ActionListener`, `MouseListener`, `KeyListener` |

### `JComponent`
- Subclasses override methods
    - `paintComponent` - displays component
    - `getPreferred` - calculates size of component
    - events handled by listeners

### Inner Classes
- Also called "dynamic nested classes"
- Classes can be members of other classes
- Can refer to instance variables and methods of the outer classes
- Inner class instances cannot be created independently of a containing class instance
    ``` java
    Outer a = new Outer();
    Outer.Inner b = a.new Inner();

    Outer.Inner b = (new Outer()).new Inner();
    ```
- Anonymous inner classes: define a class and create an object from it all at once, inside a method
    - no constructors allowed
    - static type of the expression is the interface/superclass used to create it
    - dynamic type of created object is anonymous; cannot be referred to
    - Java equivalent of OCaml first-class functions

## Chapter 30 - Swing: Layout and Drawing

## Chapter 31 - Interaction and Paint Demo

## Chapter 32 - Java Design Exercise: Resizable Arrays

## Chapter 33 - Encapsulation and Queues

## Miscellaneous
### Hash Sets and Hash Maps
- Combines advantage of arrays (efficient random access to its elements) with the advantage of a
map data structure (arbitrary keys, not just integer indices)
- Creates an index into an array by hashing the data in the key to turn it into the integer
    - `hashCode` method maps key data to ints
    - Unlike array indices, hashCodes might not be unique
- Solution to collisions: using an array of _buckets_
    - Each bucket stores the mappings for keys that have the same hash
    - Each bucket is itself a map from keys to values (implemented by a linked list or binary search tree)
    - Buckets can't use hashing to index the values - instead they use key equality
- To lookup a key in the Hash Map:
    1. Find the right bucket by indexing the array through the key's hashing
    2. Search through the bucket to find the value associated with the key
- When you override `equals` must also override `hashCode` in a consistent way
   - if `o1.equals(o2) == true` then `o1.hashCode() == o2.hashCode()` (note that converse is not necessarily true)
- Computing hashes: "smear" data throughout all the bits of the resulting integer

HashMap Performance
- Depends on workload
- If `hashCode` is good, buckets are small

### Threads and Synchronization
- Java programs can be _multithreaded_: more than one "thread" of control operating simultaneously
   - Useful when one program needs to do multiple things simultaneously
- A `Thread` object can be created from any class that implements the `Runnable` interface
    - `start`: launch the thread
    - `join`: wait for the thread to finish
- ASM
    - Each thread has own workspace and stack
    - All threads share a common heap
    - Threads can communicate via shared references
- Java provides the `synchronized` keyword
    - Only one thread at a time can be active in a synchronized method
    - Careful use can rule-out races
    - Tradeoff: less concurrency means worse performance
- Need _thread safe_ libraries
    - `java.util.concurrent` has `BlockingQueue` and `ConcurrentMap`
    - help rule out synchronization errors
    - Note: Swing is _not_ thread safe!
- Locks: objects that act as synchronizers for blocks of code
    - deadlock: cyclic dependency on synchronization
- Read-only data structures are immune to race conditions

### Garbage Collections
- Some languages use manual memory management, but Java uses garbage collection (woot!)
   - Manual memory management is cumbersome and error prone
   - Garbage collection has many advantages, including:
       - Language runtime system determines when an allocated chunk of memory will no longer be used and free it automatically.
       - Extremely convenient and safe
   - However, GC does have costs on performance and predictability
       - Lots of dead objects might affect performance
       - Space leak: global data structures can have references to "zombie" objects that won't be used, but are still reachable

Approaching GC:
- Problem can be approached by freeing memory that can't be reached from any _root_ references
    - _root_ reference - one that might be directly accessible from the program (not in heap)  
    - examples include references in the stack and global static fields
    - if an object can be reached by traversing points from a root, it is _live_
    - safe to reclaim heap allocations not reachable from a root (garbage or dead objects)

Reference Counting:
- Each heap object tracks how many references point to it
- When reference count goes to 0, reclaim that space and decrement counts for objects pointed to by that object
- Problem: cyclic data, which will never decrement to 0 (space leak)
   - Option 1: Require programmers to explicitly null-out references
   - Option 2: Periodically run GC to collect cycles
   - Option 3: Require programmers to distinguish "weak pointers" from "strong pointers"
       - If all references to an object are "weak", then object can be freed

Mark and Sweep:
- Mark
    - Start from roots
    - Do depth-first traversal
- Sweep
    - Walk over all allocated objects and check for marks
    - Unmarked objects are reclaimed
    - Marked objects have their marks cleared
    - Optional: compact all live objects by moving them adjacent to one another

Copying Garbage Collection
- Traverse over live objects in active region (_from-space_), copying them to the idle region (_to-space_)
- After copying all reachable data, switch roles of the from-space and the to-space
- All dead objects in old from-space are discarded
- Side effect: all live objects are compacted together
