---
title: "Architecture"
menu: "main"
weight: 30
---

## Language Architecture

This page is dedicated to documenting the internals and the implementation details of the Bunny language. Once implementation is nearing completion, this page will include high-level overview of the compiler and runtime and dive deeper into the specifics.

### Stages

Language design is generally broken into a few components.

The **frontend** is responsible for taking what looks like code in a file (or from a REPL) and convert it into something (Abstract Syntax Tree, or AST) we can easily transform.

The **middle-end**, or what I'm calling the **core**, is a set of transformations made on the the AST generally consisting of some simple source-to-source transformations and potentially some optimizations.

After that, the **backend** will take the transformed AST and generate code that can then be run. This can be bytecode, assembly, binary machine code, or even another language. The result can then be run by a virtual machine in the case of bytecode, assembled in the case of assembly, run directly in the case of binary machine code, or compiled with another compiler if the end result was another language.

Below is a visual representation of the Bunny implementation.

```
+----------+   -+
|  Lexer   |    |
+----------+    |
     |          |
+----------+    |
|  Parser  |    |-- Frontend
+----------+    |
     |          |
+----------+    |
|   AST    |    |
+----------+   -+
     |
+----------+   -+
|  Pass 0  |    |
+----------+    |
     |          |-- Core Compiler (Middle-end)
+----------+    |
|  Pass N  |    |
+----------+   -+
     |
+----------+   -+
| Bytecode |    |-- Code Generation (Backend)
+----------+   -+
     |
+----------+   -+
|    VM    |    |-- Execution/Runtime
+----------+   -+
```
