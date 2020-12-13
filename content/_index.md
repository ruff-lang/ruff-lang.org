## What

Bunny (_bunny_, not _bunny lang_) is a simple, practical, and fun [^1] general purpose dynamic programming language.

## Why

The motivation is to produce a concrete language specification with wish-list features from it's inspirations and to bundle the a set of developer-friendly features into a cohesive and enjoyable [^1] language.

It is inspired by classic Lisps like [Common Lisp](https://common-lisp.net/) and [Scheme](https://schemers.org/), modern Lisps like [Clojure](https://clojure.org/), dynamic and general purpose languages like [Ruby](https://www.ruby-lang.org/en/) and [Python](https://www.python.org/), and the elegant concurrency of [Go](https://golang.org/).

## How

Below is a list of features planned for the first stable release of Bunny which together _should_ satisfy [^1] the question of 'how'.

- **Minimal and Modern Syntax**, elegant [^1] and expressive.
- **Functional**, as in functions are values.
- **Tail-Call Optimized** [^2], recursion without the overhead.
- **Garbage-Collected**.
- **Hygienic Macros** [^3].
- **Continuations** [^4], as a first-class language feature.
- **Concurrent**, CSP with fibers and channels [^5].
- **Batteries Included**, to be a complete and productive tool.
- **Integrated Tooling**, ships as a single binary with all tools.

[^1]: Ultimately, this comes down to personal preference. Relevant [video](https://www.youtube.com/watch?v=Hgd2F2QNfEE).
[^2]: https://en.wikipedia.org/wiki/Tail_call
[^3]: https://en.wikipedia.org/wiki/Hygienic_macro
[^4]: https://en.wikipedia.org/wiki/Continuation#First-class_continuations
[^5]: https://en.wikipedia.org/wiki/Communicating_sequential_processes
