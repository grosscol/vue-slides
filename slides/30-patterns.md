---
author: Colin Gross
title: Return to C++
date: 2024-03-01
---

# Fallibe Constructors
E.g. needs to open a file or use a library(htslib) in the constructor

  - Might fail
  - Might leave a leaky mess
  - Forces caller to know about it and deal with it

## Factory Pattern
LLVM project uses a static builder method that returns the thing or an Error object.
Does not leave you with an unitialized object or a bad state.

```c++
class Reader {
public:
  static Expected<Foo> Create(path P) {
    Error Err = Error::success();
    Foo Reader{P, Err};
    if (Err)
      return std::move(Err);
    return std::move(F);
  }

private:
  Foo(path P, Error &Err) {
		/* check and reset Err
       Acquire resources to build object. */
  }
};
```

## Another Dependency?
Would need the LLVM support libary which can be [embedded](https://llvm.org/docs/CMake.html#embedding-llvm-in-your-project)

> From LLVM 3.5 onwards the CMake build system exports LLVM libraries as importable CMake targets. 

Small build config footprint. 

```cmake
# llvmorg-17.0.6
FetchContent_Declare(
    llvm
    GIT_REPOSITORY https://github.com/llvm/llvm-project.git
    GIT_TAG bab01aead7d7a34436bc8e1639b90227374f079e 
    GIT_SHALLOW TRUE
    SOURCE_SUBDIR llvm
)

llvm_map_components_to_libnames(LLVM_LIBRARIES support)
```
