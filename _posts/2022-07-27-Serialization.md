---
title: "Serialization"
date: 2022-07-26T09:08:00+09:00
categories:
  - Memo
tags:
  - Serialization
# header:
#   teaser: /assets/images/code.jpg
---

Serialization is the process of converting data used by an application to a format that can be transferred over a network or stored in a database or a file. In turn, deserialization is the opposite process of reading data from an external source and converting it into a runtime object. Some data serialization formats, such as JSON and protocol buffers are particularly common.

`kotlinx.serialization` provides sets of libraries for all supported platforms – JVM, JavaScript, Native – and for various serialization formats – JSON, CBOR, protocol buffers, and others. All Kotlin serialization libraries belong to the `org.jetbrains.kotlinx`: group. Their names start with `kotlinx-serialization-` and have suffixes that reflect the serialization format. Platform-specific artifacts are handled automatically; you don't need to add them manually. Use the same dependencies in JVM, JS, Native, and multiplatform projects.

Apply the Kotlin serialization Gradle plugin `org.jetbrains.kotlin.plugin.serialization` (or kotlin(“plugin.serialization”) in the Kotlin Gradle DSL).

```kotlin
plugins {
    kotlin("jvm") version "1.7.10"
    kotlin("plugin.serialization") version "1.7.10"
}
```

Add the JSON serialization library dependency

```kotlin
dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.2")
}
```

First, make a class serializable by annotating it with `@Serializable.`

```kotlin
import kotlinx.serialization.Serializable

@Serializable
data class Data(val a: Int, val b: String)
```

You can now serialize an instance of this class by calling `Json.encodeToString().`

```kotlin
import kotlinx.serialization.Serializable
import kotlinx.serialization.json.Json
import kotlinx.serialization.encodeToString

@Serializable
data class Data(val a: Int, val b: String)

fun main() {
   val json = Json.encodeToString(Data(42, "str"))
}
```

You can also serialize object collections, such as lists, in a single call.

```kotlin
val dataList = listOf(Data(42, "str"), Data(12, "test"))
val jsonList = Json.encodeToString(dataList)
```

To deserialize an object from JSON, use the decodeFromString() function:

```kotlin
import kotlinx.serialization.Serializable
import kotlinx.serialization.json.Json
import kotlinx.serialization.decodeFromString

@Serializable
data class Data(val a: Int, val b: String)

fun main() {
   val obj = Json.decodeFromString<Data>("""{"a":42, "b": "str"}""")
}
```