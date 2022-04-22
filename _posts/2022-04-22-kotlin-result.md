---
title: "Functions in Kotlin Result"
date: 2022-04-20T15:47:00+09:00
categories:
  - Memo
tags:
  - Kotlin
  - Result
  - fold
# header:
#   teaser: /assets/images/code.jpg
---
### Extension Functions

#### getOrElse: if failure, execute the {} block.

`fold` returns the result of onSuccess for the encapsulated value if this instance represents success or the result of onFailure function for the encapsulated Throwable exception if it is failure.

```kotlin
fun <R, T> Result<T>.fold(
  onSuccess: (value: T) -> R,
  onFailure: (exception: Throwable) -> R
): R
```

`getOrElse` returns the encapsulated value if this instance represents success or the result of onFailure for the encapsulated Throwable exception if it is failure.

```kotlin
fun <R, T : R> Result<T>.getOrElse(
  onFailure: (exception: Throwable) -> R
): R
```

#### list.getOrElse 

`getOrElse` provides safe access to elements of a collection. It takes an index and a function that provides the default value in cases when the index is out of bound.

```kotlin
val list = listOf(0, 10, 20)
println(list.getOrElse(1) { 42 })    // 10
println(list.getOrElse(10) { 42 })   // 42
```

```kotlin
val map = mutableMapOf<String, Int?>()
println(map.getOrElse("x") { 1 })       // 1

map["x"] = 3
println(map.getOrElse("x") { 1 })       // 3

map["x"] = null
println(map.getOrElse("x") { 1 })       // 1
```

#### getOrDefault: if failure return the input value.

`getOrDefault` returns the encapsulated value if this instance represents success or the defaultValue if it is failure.

This function is a shorthand for `getOrElse { defaultValue }`

```
fun <R, T : R> Result<T>.getOrDefault(defaultValue: R) : R
```

#### getOrThrow: if failure throw the exception

`getOrThrow` returns the encapsulated value if this instance represents success or throws the encapsulated Throwable exception if it is failure.

This function is a shorthand for `getOrElse { throw it }`

```kotlin
fun <T> Result<T>.getOrThrow(): T
```

