---
author: Colin Gross
title: Return to C++
date: 2024-03-01
---

# CMake
  - Build system
  - Modules for common uses
  - Widely adopted

## Replacing Cget
  - Fewer build dependencies
  - Cmake modules cover use cases:
    - htslib
    - libboost

## No In-Source Builds
Build artifacts no longer welcome amongst source files.

```cmake
if ( ${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR} )
  message(FATAL_ERROR 
    "In-source builds not allowed.\n"
    "Please run from CMake from the build directory, or use -B flag."
    "After removing CMakeCache.txt (rm CMakeCache.txt)\n"
    " cd ./build\n"
    " cmake --build ..\n"
    "### OR ###\n"
    " cmake -S . -B ./build\n"
    " cmake --build ./build")
```

## Getting other CMake Projects
Idioms for getting other cmake projects.

```cmake
FetchContent_Declare(
  boost
  GIT_REPOSITORY https://github.com/boostorg/boost.git
  GIT_SHALLOW TRUE
  GIT_TAG boost-1.80.0)

FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/0359...d395.zip
)
```

## Building htslib
Not a cmake project (uses make and autoconf)

```cmake
ExternalProject_Add(
  htslib
  PREFIX ${htslib_PREFIX}
  URL https://github.com/samtools/htslib/.../htslib-1.19.1.tar.bz2
  CONFIGURE_COMMAND autoreconf -i && \
    ./configure --without-libdeflate --prefix=${htslib_PREFIX}
  BUILD_COMMAND ${MAKE_COMMAND} lib-static
  INSTALL_COMMAND ${MAKE_COMMAND} \
    install prefix=${htslib_INSTALL}
```

## Finding System Libs
The ones we need already have modules maintained by CMake.

```cmake
include(FindZLIB)
include(FindBZip2)
include(FindLibLZMA)
include(FindCURL)
include(FindOpenSSL)
```
