---
title: "Kotlin associateBy Function"
date: 2022-09-01T017:11:00+09:00
categories:
  - Memo
tags:
  - kotlin
  - functions
  - associateBy
# header:
#   teaser: /assets/images/code.jpg
---

```kotlin
inline fun <T, K, V> Sequence<T>.associateBy(
    keySelector: (T) -> K,
    valueTransform: (T) -> V
): Map<K, V>
```

`associateBy` Returns a Map containing the elements from the given sequence indexed by the key returned from keySelector function applied to each element. If any two elements would have the same key returned by keySelector the last one gets added to the map. The returned map preserves the entry iteration order of the original sequence.

```kotlin
data class Person(val firstName: String, val lastName: String) {
    override fun toString(): String = "$firstName $lastName"
}

val scientists = listOf(Person("Grace", "Hopper"), Person("Jacob", "Bernoulli"), Person("Johann", "Bernoulli"))

val byLastName = scientists.associateBy { it.lastName }

// Jacob Bernoulli does not occur in the map because only the last pair with the same key gets added
println(byLastName) // {Hopper=Grace Hopper, Bernoulli=Johann Bernoulli}
```

Also Returns a Map containing the values provided by valueTransform and indexed by keySelector functions applied to elements of the given sequence.

```kotlin
data class Person(val firstName: String, val lastName: String)

val scientists = listOf(Person("Grace", "Hopper"), Person("Jacob", "Bernoulli"), Person("Johann", "Bernoulli"))

val byLastName = scientists.associateBy({ it.lastName }, { it.firstName })

// Jacob Bernoulli does not occur in the map because only the last pair with the same key gets added
println(byLastName) // {Hopper=Grace, Bernoulli=Johann}
```