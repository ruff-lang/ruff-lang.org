## What

Bunny (_bunny_, not _bunny lang_) is a simple, practical, and fun[^1] general purpose programming language designed to be user friendly and productive.

It is inspired by classic Lisps like [Common Lisp](https://common-lisp.net/) and [Scheme](https://schemers.org/), modern Lisps like [Clojure](https://clojure.org/), dynamic languages like [Ruby](https://www.ruby-lang.org/en/) and [Python](https://www.python.org/), the concurrency patterns of [Go](https://golang.org/), and the module system of [OCaml](https://ocaml.org/).

Here's a small example, computing the `nth` [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number):
```
(defun fibonacci (n)
  (if (< n 2)
    1
    (+ (fib (- n 1))
       (fib (- n 2)))))
```

## Why

My motivation is to produce a concrete language specification with wish-list features from its inspirations and to bundle this set features into a cohesive and enjoyable[^1] language.

Most importantly, this is a passion project and a vehicle for me to explore new found curiosities in the world of programming language theory and design.

## How

Below is a list of features planned for the first stable release of Bunny which together _should_ satisfy[^1] the question of 'how'.

- **Minimal and Modern Syntax**, elegant[^1] and expressive.
- **Functional**, as in functions are values.
- **Garbage-Collected**, effort free memory management.
- **Tail-Call Optimized**, recursion without the overhead.
- **Hygienic Macros**, enabling the full power/fun of macros.
- **Concurrent**, lightweight with fibers and queues for message passing.
- **Batteries Included**, to be a complete and productive tool.

## When

Bunny's [specification](https://bunny-lang.org/specification/) is in early design and iteration phase. I began research towards the end of 2020, and while a stable release is rather far I aim to have work to show by the end of 2021.

[^1]: this obviously comes down to personal preference, and yours might differ.

