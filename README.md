# CQ

A specification for C-like interfaces to quantum computers.

## What is CQ?

CQ is a specification for a C-like interface to quantum computers. It is _not_ a library, or a DSL, or a framework, but you could use it to implement such things. In fact you are encouraged to do so. 

Although the interface is C-like, we don't insist that implementations are written in C. You could certainly write a compatible implementation in C++, or even in python or Fortran (if you must).

CQ is an open specification, and contributions are welcomed. For more details see the [contribution guidelines](CONTRIBUTING.md).

## Why?

There are really two questions to answer here.

### Why specify an interface to quantum computers?

We believe standard APIs are fundamentally useful. They allow application developers to reason about their applications, without needing to worry about the specific underlying technology. If they use the same interface, the same source code can be recompiled and rerun elsewhere, enabling code portability. This is a Good Thing :tm:.

### Why C-like?

Okay, so why not specify a RESTful API? That's a very good question, with a simple answer -- we're interested in the long-term view of quantum computers as tightly-coupled accelerators. For _[reasons](https://www.mpi-forum.org/)_, C and Fortran remain the ubiquitous programming languages across HPC. C++ is increasingly prevalent, especially in accelerator programming, but it's easier to access a C interface from C++ than the other way round so a C-like interface seemed to us to be the obvious starting place.

This does not mean we expect everyone to program quantum computers in C. We expect CQ implementations to act as a performant intermediate layer between higher level abstractions and programming models, and lower-level QHDLs like [OpenQASM](https://openqasm.com/) and [QIR](https://www.qir-alliance.org/).

## Structure of this Repository

- cq: The core CQ specification, for interfacing with quantum hardware.
  - datatypes: Basic and complex datatypes which should be provided by a CQ implementation.
  - operators: Operators which act on CQ datatypes.
  - functions: Functions which act on CQ datatypes.
- sim-ext: The CQ simulation extensions, for interfacing with classical simulations of quantum information.