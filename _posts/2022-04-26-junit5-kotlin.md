---
title: "JUnit 5 for Kotlin Developers"
date: 2022-04-26T15:47:00+09:00
categories:
  - Memo
tags:
  - Kotlin
  - JUnit5
# header:
#   teaser: /assets/images/code.jpg
---

## 2. Simple JUnit 5 Tests
Note that the imports required are different, and *we do assertions using the Assertions class instead of the Assert class.*

## 3. Advanced Assertions
JUnit 5 adds some advanced assertions for working with lambdas.

### 3.1. Asserting Exceptions
JUnit 5 adds an assertion for when a call is expected to throw an exception. *We can test that a specific call — rather than just any call in the method — throws the expected exception.* We can even assert on the exception itself.

```kotlin
@Test
fun `Dividing by zero should throw the DivideByZeroException`() {
    val exception = Assertions.assertThrows(DivideByZeroException::class.java) {
        calculator.divide(5, 0)
    }

    Assertions.assertEquals(5, exception.numerator)
}
```

### 3.2. Multiple Assertions
JUnit 5 adds the ability to perform multiple assertions at the same time, and it’ll evaluate them all and report on all of the failures. In Kotlin, we need to handle this slightly differently. The function actually takes a varargs parameter of type Executable.

```kotlin
fun `The square of a number should be equal to that number multiplied in itself`() {
    Assertions.assertAll(
        Executable { Assertions.assertEquals(1, calculator.square(1)) },
        Executable { Assertions.assertEquals(4, calculator.square(2)) },
        Executable { Assertions.assertEquals(9, calculator.square(3)) }
    )
}
```

### 3.3. Suppliers for True and False Tests
Kotlin allows us to pass in a lambda in the same way that we saw above for testing exceptions. We can also pass in method references. This is especially useful when testing the return value of some existing object like we do here using `List.isEmpty`:

```kotlin
@Test
fun `isEmpty should return true for empty lists`() {
    val list = listOf<String>()
    Assertions.assertTrue(list::isEmpty)
}
```

### 3.4. Suppliers for Failure Messages
In some cases, we want to provide our own error message to be displayed on an assertion failure instead of the default one. Often these are simple strings, but sometimes we may want to use a string that is expensive to compute. In JUnit 5, we can provide a lambda to compute this string, and it is only called on failure instead of being computed upfront.

```kotlin
@Test
fun `3 is equal to 4`() {
    Assertions.assertEquals(3, 4) {
        "Three does not equal four"
    }
}
```

## Data-Driven Tests
### 4.1. TestFactory Methods
The easiest way to handle data-driven tests is by using the @TestFactory annotation. This replaces the @Test annotation, and the method returns some collection of DynamicNode instances — normally created by calling DynamicTest.dynamicTest.
```kotlin
@TestFactory
fun testSquares() = listOf(
    DynamicTest.dynamicTest("when I calculate 1^2 then I get 1") { Assertions.assertEquals(1,calculator.square(1))},
    DynamicTest.dynamicTest("when I calculate 2^2 then I get 4") { Assertions.assertEquals(4,calculator.square(2))},
    DynamicTest.dynamicTest("when I calculate 3^2 then I get 9") { Assertions.assertEquals(9,calculator.square(3))}
)
```
We can easily build our list by performing some functional mapping on a simple input list of data:
```kotlin
@TestFactory
fun testSquares() = listOf(
    1 to 1,
    2 to 4,
    3 to 9,
    4 to 16,
    5 to 25)
    .map { (input, expected) ->
        DynamicTest.dynamicTest("when I calculate $input^2 then I get $expected") {
            Assertions.assertEquals(expected, calculator.square(input))
        }
    }
```