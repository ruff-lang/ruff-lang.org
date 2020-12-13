**Bunny** (_bunny_, not _bunny lang_) aims to be a simple, practical, and fun general purpose dynamic programming language. It is inspired by a collection of languages including classic Lisp dialects like **Common Lisp** and **Scheme**, modern Lisp-inspired languages like **Clojure**, dynamic and general purpose languages like **Ruby** and **Python**, and elegant concurrent languages like **Go**.

The motivation behind the specification is to produce a concrete language design with wish-list features from it's inspirations. The goal is to bundle the most developer-friendly features into a cohesive and enjoyable language.

## Language Features

- **Minimal and Modern Syntax**, aiming to be elegant, expressive, and readable.
- **Functional**, as in functions are values.
- **Tail-Call Optimized**, recursion without the overhead.
- **Garbage-Collected**, using a mark-and-sweep algorithm.
- **Hygienic Macros**.
- **Continuations**, as a first-class language feature (`call/cc`).
- **Concurrent**, in the spirit of Go with coroutines and channels (built on top of continuations).
- **Batteries Included**, to be a practical and productive tool (see also: https://github.com/bunny-lang/stdlib).
- **Integrated Tooling**, `bunny` ships as a single binary with all the tools needed to be productive.
