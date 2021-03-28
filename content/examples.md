---
title: "Examples"
menu: "main"
weight: 10
---

## Examples

This page contains code examples of Bunny, listed out in increasing complexity.

### Hello, World!

Printing to standard output is a pretty common thing for languages to do. Here's the classic `Hello World` program.

```
# println appends a newline character and prints to stdout
(println "Hello, World!")
```

Below is a variant that will ask the user for input.

```
(println "What is your name?")

# readline will wait for user input
(let ((name readline))
  (println (format "Hello, %s!" name)))
```

### Factorial

Below is a tail-recursive implementation of the factorial function:

```
(defun factorial (n)
  (fact_tail_rec n 1))

(defun fact_tail_rec (n acc)
  (if (= n 0)
    acc
    (fact_tail_rec (- n 1) (* n acc))))
```

### Fibonacci Sequence

Function to compute the `nth` number in the [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number).

```
(defun fib (n)
  (if (< n 2)
    1
    (+ (fib (- n 1))
       (fib (- n 2)))))
```

### Simple Webserver

TODO...
