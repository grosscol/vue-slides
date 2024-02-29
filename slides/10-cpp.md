---
author: Colin Gross
title: Return to C++
date: 2024-02-29
---

# C++14 and C++17
New to me features

## Uniform initialization
Old ways

```c++
int a;
int b=3;
int c(4);
```

Uniform ways
```c++
int a{};
int b{3};
int c{4};
```

## Auto
A keyword that used to be redundant.

```c++
vector<int> gen_numbers(){ /*...*/ }

int main(){
  // explicit type
  vector<int> x_numbers = gen_numbers();

  // let the compiler figure it out
  auto a_numbers = gen_numbers();
}
```

## std::filesystem
Convenient way of building paths in the standard libarary.

```c++
#include<filesystem>
std::filesystem::path dir{TEST_DATA_DIR};
std::filesystem::path file{"structvar_sample_input.vcf"};
std::filesystem::path path{test_data_dir / test_data_file};
```

## std::move

> std::move is used to indicate that an object t may be "moved from", i.e. allowing the efficient transfer of resources from t to another object.

- Makes it possible to "steal" resources like streams or connections.
- Avoids a lot of uneccissary copies.
- Complilers will use it where appropriate.

## Odds and ends
Small things that caught my eye

```c++
// Instead of char
std::byte foo;

// A smarter thing that could be one of three types
std::variant<BCF_BT_INT32, BCF_BT_INT16, BCF_BT_INT8> fmt_value;

// Ruby-ish namespaces
namespace Foo::Bar::Baz { /*...*/ }
```
