# Note: Build your own programming language

## Why should you?
Push you understanding of computer system. Knowledge stack includes, CPU/Memory, OS, processes, threads, stack, in-memory data structures. On the math side, the stack includes category theory, type system, algebras, graph thoery, complexity analysis. 

## Trade offs
Lots of interesting ideas in programming language, the author suggests pick a small number of ideas and go deep on them. 

One of the trade-off is complexity to parse vs complexity to execute. As an example, LISP or functional languages are easy to parse (due to the fact that it is how computating works), but hard to execute on a modern computer (functional language is not how computer works.)

## Design
Spend more time design semantics rather than syntax. 
Semantics includes:

1. type system
2. language organizing unit: module, pacakages, libraries or more language specific unit like Rust Crates etc.
3. exception handling
4. tail recursion optimization/ loops allowed?
5. built-in vs standard library
6. memory allocation: user specific vs GC

(I have never heard of APL/J TBH. Sounds like they would be suitable for data oriented systems.)

## Implementation
### Test Drive
write simple applications in the proposed language semantics and see how it works/ where need to be imporved.

### Build an interpreter/compiler
 [Crafting Interpreters](https://craftinginterpreters.com/contents.html) by Robert Nystrom and [Let’s Build A Simple Interpreter](https://ruslanspivak.com/lsbasi-part1/) by Ruslan Spivak.
 

## Other considerations
1. Optimization 
2. foreign function interface
3. JIT compilation



