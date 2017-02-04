# CIS 120 Notes

## True-False Guide
Answers to some True-False questions from the old final exams


**T** **F**
### Fall 2012
1. **T** - When you override the `equals` method of a class, you should be sure to override the `hashCode` method compatibly.
2. **F** - If `A` is a subtype of `B`, then `Set<A>` is a subtype of `Set<B>`.
3. **T** - When using the Java IO libraries, one should generally wrap a `FileReader` object inside a `BufferedReader` to prevent significant performance problems.
4. **F** - It is possible to create an object of type `Set<int>`.
5. **F** - An object's `static` type is always a subtype of its `dynamic` type.
6. **F** - The value **null** can be assigned to a variable of any type.
7. **T** - A method with the following declaration definitely will not throw an `IOException` to a calling context.
``` ruby
public void m() {...}
```
8. **T** - The `@Override` annotation prevents accidental overloading of a method.
9. **F** - In some cases, dynamic dispatch of a method invocation requires the Java ASM to search the entire stack to find the appropriate code to run next.
10. **F** - It is not possible to call a method declared as `static` from within a non-`static` method.

### Spring 2015: Java Swing Programming
1. **T** - The type `MyPanel` is a subtype of `Object`.
2. **F** - The new object created on line 27 is an instance of `MyPanel` (SP15 - Appendix D).
3. **T** - The instance variables `x` and `y` on lines 23 and 24 can only be modified by methods of the `MyPanel` class or the methods of any inner classes of `MyPanel`.
4. **T** - The dynamic class of f (declared on line 13) is `JFrame`.
5. **T** - The `mouseMoved` method on line 28 is called by the Swing event loop in reaction to the user moving the mouse in the main window of the application.
6. **F** - The `paintComponent` method on line 42 is only invoked once, at the start of the application.
7. **F** - A `JPanel` cannot be added to another `JPanel` using the add method.
8. **F** - If the user replaces the code on line 31 (the call to `repaint()`) with `paintComponent (new Graphics ())`, then the behavior of the application would not change.
9. **T** - The anonymous class defined on line 27 implements or inherits all members of the `MouseMotionListener` interface.
10. **F** - The method `createAndShowGUI` cannot be invoked without an instance of class `GUI`.
