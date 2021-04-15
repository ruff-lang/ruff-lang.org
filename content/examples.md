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

### Interactive Hello, `<name>`!

```
(println "What is your name?")

# readline will wait for user input
(let ((name (read_line)))
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

### Simple Webserver

TODO...
