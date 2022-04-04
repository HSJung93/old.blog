---
title: "Kotlin의 with와 apply 함수"
date: 2022-04-04T12:47:00+09:00
categories:
  - Memo
tags:
  - kotlin
  - with
  - apply
# header:
#   teaser: /assets/images/code.jpg
---

## `with` Function

### AS-IS
```kotlin
fun alphabet(): String{
  val result = StringBuilder()
  for (letter in 'A'..'Z') {
    result.append(letter)
  }
  result.append("\nNow I know the alphabet!")
  return result.toString()
}
```

### TO-BE
```kotlin
fun alphabet() = with(StringBuilder()){
  for (letter in 'A'...'Z'){
    append(letter)
  }
  append("\nNow I Know the alphabet!")
  toString()
}
```
with 함수의 반환 값은 람다의 반환 값이 된다. with 함수의 내부를 살펴보자. with 함수는 T 타입을 인자로 받고 R 타입을 리턴하는 함수이다. 

```kotlin
@kotlin.internal.InlineOnly
public inline fun <T, R> with(receiver: T, block: T.() -> R): R {
  contract {
    callsInPlace(block, InvocationKind.EXACTLY_ONCE)
  }
  return receiver.block()
}
```

두번째 인자로 받은 람다 `T.() -> R` 이 마치 첫번째로 받은 인자 타입이 receiver `T`인 확장 함수처럼 동작 하고 있다. 

## `apply` Function
with 함수는 람다의 반환 값이 with의 반환 값이 된다. 하지만, 람다는 람다대로 수행하고 실제 receiver가 함수의 리턴 값이 되길 바랄 때가 있는데 이럴 때 apply 함수를 이용한다.

```kotlin
fun alphabetUsingApply() = StringBuilder().apply {
  for (letter in 'A'..'Z'){
    append(letter)
  }
  append("\nNow I Know the alphabet!")
}.toString()
```

### `with` vs `apply`
* `with` : 람다의 마지막 결과가 with 함수의 반환값이 된다. with의 두번째 인자 람다의 반환은 특정 객체를 리턴한다.
* `apply` : 람다는 람다대로 수행하고, receiver 객체 자기자신을 반환한다. apply의 인자인 람다의 반환은 Unit(void)이다. `apply` 함수 내부를 살펴보자.

```kotlin
@kotlin.internal.InlineOnly
public inline fun <T> T.apply(block: T.() -> Unit): T {
  contract {
    callsInPlace(block, InvocationKind.EXACTLY_ONce)
  }
  block()
  return this
}
```

apply 함수 내부를 보면 apply를 호출한 인스턴스 타입이 receiver인 확장 함수 람다를 인자로 받는다. 해당 람다의 반환은 Unit이다. 실제 코드 내부에서는 해당 람다를 수행하고 자기 자신(this)을 리턴하고 있다. 예컨대, 다음과 같이 `StringBuilder`를 만들어주는 것과 `toString`을 호출하는 것을 대신해주는 `buildString`을 사용할 수 있다.

```kotlin
fun alphabetUsingBuildString() = buildString {
  for (letter in 'A'..'Z'){
    append(letter)
  }
  append("\nNow I Know the alphabet!")
}

@kotlin.internal.InlineOnly
public inline fun buildString(builderAction: StringBuilder.() -> Unit): String =
StringBuilder().apply(builderAction).toString()
```