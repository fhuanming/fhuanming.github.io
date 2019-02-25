---
title: C++ Partial Template Specialization
date: 2019-02-24 15:00:00
tags: c++ partial_template_specialization template_metaprogramming
---

最近读到一段代码，大致如下：

```c++
template <typename Curr, typename... Rest>
struct GetNext {
    // using type = void;
};

template <typename Curr, typename N, typename... Rest>
struct GetNext<Curr, N, Rest...> {
    // using type = something_else;
};
```

由于对C++ Template Metaprogramming 不是很熟悉， 觉得在用GetNext的时候，两个申明都可能会匹配上，那么compiler怎么知道选择哪个呢。

<!-- more -->

经过一番查找，我才是道这是Partial Template Specialization（偏特化）。举个例子

```c++
template<typename T1, Typename T2>
struct Test {}; // primary template

template<typename T>
struct Test<T, T> {}; // partial specialization where T1 = T2.
```

除了偏特化，还有全特化explicit (full) template specialization. 例子如下：

```c++
template<typename T1, Typename T2>
struct Test {}; // primary template

template<>
struct Test<int, int> {};
```

那么在有多个偏特化的情况下，当实例化的时候，compiler到底是如何做出选择的呢？ Rule如下：

1. If only one specialization matches the template arguments, that specialization is used

1. If more than one specialization matches, partial order rules are used to determine which specialization is more specialized. The most specialized specialization is used, if it is unique (if it is not unique, the program cannot be compiled)

1. If no specializations match, the primary template is used

For more detail, please see [cppreference](https://en.cppreference.com/w/cpp/language/partial_specialization).