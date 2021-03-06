---
title: "Specification"
menu: "main"
weight: 20
---

## Language Specification

This is the canonical specification for the Bunny programming language. Contents below this line are copied directly from https://github.com/bunny-lang/specification.

---

<!---
  This Source Code Form is subject to the terms of the Mozilla Public
  License, v. 2.0. If a copy of the MPL was not distributed with this
  file, You can obtain one at http://mozilla.org/MPL/2.0/.
-->

### File Format

Bunny source files must be [UTF-8](https://en.wikipedia.org/wiki/UTF-8) with the `.bn` file extension.

### Atoms

An `atom` is a singular piece of data.

|**Atom** |**Semantic Meaning**|
|---------|--------------------|
|symbol   | A symbol is a word or an operator, for example `foo` is a symbol and so is `+` |
|keyword  | A keyword is like a symbol that evaluates to itself, for example `:foo` is a keyword |
|boolean  | `true` and `false` are symbols representing truthiness |
|number   | Any integer, or floating point integer for example `1`, `19`, `3.14159265` |
|character| Any letter prepended with a backslash, for example `\a` represents the first letter of the english alphabet. |

### Pairs (Cons cell)

The core data type in Bunny, a cons cell is a pair of two things. The first thing in the pair is known as the `head`, and the second thing is known as the `tail`.

```
+------+------+
| head | tail |
+------+------+
    |      |
    |      |__ this points to another pair
    |
    |__ this holds an atom or a pair
```

Syntax to build a pair is `(pair <head> <tail>)`. Dotted-pair notation is syntactic sugar, `(<head> . <tail>)`.

### Lists

Lists are arbitrarily long sequences of pairs. `()` is the empty list, or a pair with no head or tail. Lists are created with sequences of pairs where the last element is the empty list.

All three forms below are equivalent.

```lisp
(1 2 3)
(1 . (2 . (3 . '())))
(pair 1 (pair 2 (pair 3 '())))
```

Lists are always evaluated unless they are quoted. Quoting stops evaluation. Lists can be quoted with `(quote <list>)` or the syntactic sugared form, `'(<list>)`.

```lisp
(+ 1 2)
=> 3

(quote (+ 1 2))
=> (+ 1 2)

'(+ 1 2)
=> (+ 1 2)
```

Inside a quoted form, we can resume evaluation selectively. This is called unquoting. `(unquote <list>)` or syntactic sugared form, tilde `~<list>`, are used to unquote a list.

```lisp
(quote (+ 1 (unquote (+ 2 3))))
=> (+ 1 5)

;; equivalent, using syntactic sugar
'(+ ~(+ 2 3))
=> (+ 1 5)
```

Lists can be unquoted into position in the quoted template with `unquote-splice`. This is useful for when you want to flatten a list. The at-sign (`@`) is the syntax for unquote-splice. For example:

```lisp
'(1 2 ~(list 3 4))
=> (1 2 (3 4))

(quote (1 2 (unquote-splice (list 3 4))))
=> (1 2 3 4)

;; equivalent, using syntactic sugar
'(1 2 @(list 3 4))
=> (1 2 3 4)
```

`head` is a function returning the first element of a pair or a list.

```lisp
(head '(1 2 3))
=> 1
```

`tail` is a function returning the rest of the list.

```lisp
(tail '(1 2 3))
=> (2 3)
```

### Arrays and Hash Maps

Two additional native datastructures are available, arrays and hash maps. The syntax for arrays is square brackets, `[]`, `[1 2 3]`, etc. Arrays evaluate to themselves and are immutable.

```lisp
(define names ["albert" "bob" "charlie"])

;; get the length of an array
(Array.length names)
=> 3

;; search for a particular element
(Array.find names "bob")
=> 1

;; get the first item
(head names)
=> "albert"

;; get the rest of the array
(tail names)
=> ["bob" "charlie"]
```

Hash maps, like arrays, are immutable and evaluate to themselves. Hash maps are created with `{}`, and can assign arbitrary keys and values.

```lisp
(define contact {:first-name "Mister"
                 :last-name "Rabbit"
                 :number 1234567890})

;; you can lookup values for a key
(Hash.get contact :first-name)
=> "Mister"
```

### Comments

Comments in Bunny are specified with two semi-colons, `;;`.

```lisp
;; this is a comment

;; this is a comment that
;; spans multiple lines
```

In-line comments should have two whitespaces preceding.

```lisp
(+ 1 2)  ;; add one and two
       ^^
   whitespace
```

### Variables

Values can be defined globally with `(define <name> <body>)`. Mutation is possible with `(set! <name> <value>)` but discouraged in actual logic.

```lisp
(define foo 42)
(println foo)
=> 42

(set! foo (+ foo foo))
(println foo)
=> 84
```

Variables can be lexically scoped using `let` and then used within the scoped expression.

```lisp
(let (foo 42)
  (+ foo foo))
=> 84
```

Multiple bindings can be used.

```lisp
(let (foo 1 bar 2)
  (+ foo bar))
=> 3
```

`let` evaluates each binding immediately and in-order, allowing for dependent bindings.

```lisp
(let (foo 2
      bar (* foo foo))
  (+ foo bar))
=> 6
```

### Functions

Bunny has two forms of functions, anonymous functions (`lambda` or `λ`) and named functions which are just syntactic sugar for a `λ` inside a `define` form.

Anonymous functions take the form `(λ (<arguments>) (<expression>))`. Below is an example that squares a number.

```lisp
(λ (x) (* x x))
```

Since functions are values, and we use `define` to give names to values, we can use `define` and `lambda` to express a named function.

```lisp
(define <function_name>
  (λ (<arguments>)
    (<expression>)))
```

For example, we can define the function `incr` that increments a given integer.

```lisp
(define incr
  (λ (x)
    (+ x 1)))
```

And then invoke it:

```lisp
(incr 1)
=> 2
```

A special short-hand form `defun` is a convenient way to define named functions with arguments. `(defun <function_name> (<arguments>) (<expression>))`. The same `incr` function defined using the `defun` short-hand form below.

```lisp
(defun incr (x)
  (+ x 1))
```

### Conditionals

Basic conditional logic forms in Bunny are pretty similar to Scheme. Below are the forms conditionals can take with some examples.

`if` blocks take the form `(if (<condition>) (<true_expression>) (<false_expression>))`.

```lisp
(if (< 1 0)
  (println "condition met")
  (println "condition failed"))
=> "condition failed"
```

`when` is a macro that expands to `(if (<condition>) (<true_expression>) nil)`. It is preferred when there's no `else` clause needed.

```lisp
(when (< 1 2) (println "condition met"))
=> "condition met"

(when (< 3 2) (println "condition met"))  ;; nothing will be displayed in the REPL
```

`cond` blocks are a slightly more generic way to construct multiple conditions, they take the form `(cond (<conditional_0>) (<expression_0>) ... (<conditional_n>) (<expression_n>))`.

```lisp
;; Assume x is bound or supplied by a function argument, this
;; condition will return a string based on the value of x.
(cond ((< x 10) "less than 10")
      ((< x 100) "less than 100")
      (else "no conditions met"))

;; If no result expression is given, the condition block will
;; return the result of the conditional expression.
(cond ((< 1 10)))
=> true
```

`and` is simply the logical and. `or` is the logical or.

```lisp
(and (< 1 2) (< 2 3))
=> true

(and (< 2 1) (< 2 3))
=> false

(or (< 2 1) (< 1 2))
=> true

(or (< 2 1) (< 3 1))
=> false
```

`not` negates the boolean expression immediately following it.

```lisp
(not (< 1 2))
=> false
```

`unless` is a macro preferred over `(if (not ..) .. ..)` and evaluates to an equivalent `if` block with the conditional expression negated.

```lisp
(unless (even? 2)
  (println "not even")
  (println "even"))
=> "even"

;; unless can be used without a second clause making it an implicit nil
(unless (odd? 2) (println "not odd"))
=> "not odd"

(unless (odd? 3) (println "not odd"))  ;; nothing will be displayed in the REPL
```

### Loops

There are a few ways to loop over a conditional or over a sequence (as in, a list).

The `while` loop takes and evaluates a conditional beforing entering the body. The form is `(while <condition> <body>)`.

```lisp
;; infinite loop
(while true
  (println "brrr"))
```

A `for` loop loops over a sequence, allowing the user to operate on each element of that sequence. Note that this is not a very _functional_ approach, but allows for some flexibility when needed. The form is `(for (<variable> <sequence>) <body>)`.

```lisp
;; prints each name in the list
(for (name ["albert" "bob" "carl"])
  (println name))
```

Finally, the more idiomatic way of writing the above using `map`, form: `(map <function> <sequence>)`.

```lisp
;; equivalent to the for loop, but simpler and more idiomatic
(map println ["albert" "bob" "carl"])
```

### Signals and Errors

TODO...

### Concurrency

Lightweight threads are used for concurrency, also known as coroutines, green threads, and fibers. The runtime handles all concurrency tasks and exposes a simple interface with fibers. Messages can be shared across fibers using queues.

You can start a new concurrent task with `spawn`.

```lisp
(spawn
  (while true
    (sleep 10)
    (println "brr")))
```

Fibers can be exited out with `(done)`. Every fiber implicitly has a reference to its parent fiber, and as such can invoke the `done?` method to detect if the parent exited out.

```lisp
(spawn
  (while true
    (when (done?) (done))
    (println "brr")))
```

Queues can be created with `queue`.

```lisp
(define some-queue (queue))
```

Things can be added to a queue with `put`.

```lisp
(put some-queue "foo")
```

And things can be taken off a queue with `take`, which blocks until the queue has something on it.

```lisp
(let (msg (take some-queue))
  (println (format "got message: %s" msg))))
```

We can put an error on the queue as a signal to a fiber to terminate. Here's an example of a fiber that prints messages received til it sees an error.

```lisp
(let (msgs (queue))
  (spawn
    (while true
      (let (msg (take msgs))
        (when (error? msg) (done))
        (println (format "received: %s" msg))))))
  (put msgs "hello")
  (put msgs "hello again!")
  (put msgs (error "signaling an error")))

=> "received: hello"
   "received: hello again!"
```

Note that using `define` will create a queue in the global scope of the module or file and will not be garbage collected. Using `let` will clean up the queue resources once that bound variable is no longer referenced.

### Modules

Modules provide a way of organizing and grouping code together. They also provide a convenient way to distinguish between code meant to be shared or exposed (in the case of a library) and code meant for internal use only.

A module can be defined to only explicitly export a list of public functions. Otherwise, all definitions inside the module will be implicitly exported. We can define a new module using `defmodule`, and tell the language where we're implementing that module with `module`. Note that both the module definition and the implement directive can be in the same file.

```lisp
;; mod.bn

(defmodule Utilities
  (export (print-uppercase
           print-lowercase)))
```

```lisp
;; utilities.bn

(module Utilities)

(defun print-uppercase (arg)
  (let (uppercase (String.upper arg))
    (println uppercase)))
```

In another file this module can be used.

```lisp
(Utilities.print-uppercase "hello")
=> "HELLO"
```

To avoid having to use the dot notation, the module can be imported with `use`.

```lisp
(use Utilities)

(print-uppercase "hello")
=> "HELLO"
```

If a symbol is already defined leading to a name conflict, `(use <module>)` will throw an error at build time.

Alternatively, a file containing definitions _not_ in a module can be imported for use. Define in one file:

```lisp
;; helpers.bn

(defun roll-dice ()
  (Random.pick [1 2 3 4 5 6])
```

and import in another using relative path:

```lisp
;; application.bn

(import "helpers.bn")

(roll-dice)
=> 6
```

If `application.bn` already defines a symbol in `helpers.bn`, `import` will throw an error at build time.

### Macros

TODO...
