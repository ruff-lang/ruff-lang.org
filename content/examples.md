---
title: "Examples"
menu: "main"
weight: 10
---

## Examples

This page contains code examples of Bunny, listed out in increasing complexity.

### Hello, World!

Printing to standard output is a pretty common thing for languages to do. Here's the classic `Hello World` program.

```lisp
;; println appends a newline character and prints to stdout
(println "Hello, World!")
```

### Interactive Hello, `<name>`!

```lisp
(println "What is your name?")

;; readline will wait for user input
(let ((name (read-line)))
  (println (format "Hello, %s!" name)))
```

### Factorial

Below is a tail-recursive implementation of the factorial function:

```lisp
(defun factorial (n)
  (defun fact-tail-rec (n acc)
    (if (= n 0)
      acc
      (fact-tail-rec (- n 1) (* n acc))))
  (fact-tail-rec n 1)))
```

### Simple Webserver

TODO...
