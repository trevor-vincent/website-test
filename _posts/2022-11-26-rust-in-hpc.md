---
title: "Rust in HPC?"
date: 2022-11-26T13:34:30-04:00
categories:
  - blog
tags:
  - high performance computing
  - rust
  - hpc
---

While Rust is a modern low-level language with a great package manager, memory safety and other nice-to-have features, there are still huge practical problems with adopting it for high performance computing (HPC) use cases. In this short post I list a few points you should consider before writing an HPC library in Rust.

1) There is a massive HPC ecosystem surrounding three languages: Fortran, C and C++. Of these, C++ is definitely dominating next-generation HPC frameworks (Kokkos, SYCL, HPX, Charm++, Taskflow,...) as well as the standard stuff (usually C/C++) like OpenMP, MPI, CUDA, HIP, etc. Not to mention most of the big numerical HPC libraries are written in the C/C++/Fortran languages. The most important thing here is AFAIK not a single next-generation (e.g.  distributed, asynchronous, task-based) HPC library written for Rust.

2) All of Rust's memory safety features go out the window when you start GPU programming. In fact all GPU code is immediately 'unsafe' in Rust terms. While there are bindings in Rust for GPU work, they are currently in an unstable state and therefore not production ready.

3) Rust has not been shown in any reproducible or objective way to be 'faster', in fact most benchmarks I've seen tend to favor C, but language speed wars are often not very objective, so the main point here is that there is no speed boost by adopting Rust over a traditional low-level language.

4) Rust is not much easier than C++ to develop code in. In fact, you could perhaps argue it is harder, but, regardless, It is low-level and therefore it is hard like most other low-level languages. The compiler errors may be nicer, but you pay for that in other ways.

 Currently for HPC scientific libraries the way to go IMHO, is to write a python frontend interface pybinded to a backend in C++. The C++ backend provides the HPC capabilities. This is a extremely flexible setup that  can support any hardware, be it commodity-grade or supercomputer. Furthermore, it is also a well-trodden path and offers support to users that are only familiar with python. I'm looking forward to seeing more HPC support for Rust in the future and perhaps at some point it will join the big three or replace them.
