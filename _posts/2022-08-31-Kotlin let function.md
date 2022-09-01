---
title: "Kotlin let Function"
date: 2022-08-30T017:11:00+09:00
categories:
  - Memo
tags:
  - kotlin
  - functions
# header:
#   teaser: /assets/images/code.jpg
---

Technically, functions are interchangeable in many cases.

## let

**The context object** is available as an argument(`it`). **The return value** is the lambda result.

`let`can be used to invoke one or more functionson result of call chains.

```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter{ it > 3 }.let { 
  println(it) 
  // and more function calls if needed
}
// [5, 4, 4]
```

If the code block contains a single function with `it` as an argument, you can use the method reference(`::`) instead of the lambda:

```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter{ it > 3 }.let(::println)
// [5, 4, 4]
```

`let` is often used for executing a code block only with non-null values.

