---
title: "Redis SCARD SADD"
date: 2022-06-26T10:56:00+09:00
categories:
  - Memo
tags:
  - Redis
# header:
#   teaser: /assets/images/code.jpg
---

## `SCARD key`
Returns the set cardinality(number of elements) of the set stored at key.

### Time Complexity: O(1)

### Return
Integer reply: the cardinality(number of elements) of the set, or 0 if key does not exist.

### Examples
```
SADD myset "Hello"
// 1
SADD myset "World"
// 1
SCARD myset
// 2
```

## `SADD key member [member ...]`
Add the specified members to the set stored at key. Specified members that are already a member of this set are ignored. If key does no exist, a new set is created before adding the specified members.

An error is returned when the value stored at key is not a set.
### Time Complexity: O(1) for each element added, so O(N) to add N elements when the command is called with multiple arguments
### Return
Integer reply: the number of elements that were added to the set, not including all the elements already present in the set.

### Examples 
```
SADD myset "Hello"
// 1
SADD myset "World"
// 1 
SMEMBERS myset
// 1) "Hello"
// 2) "World"
```

## `SMEMBERS key`
Returns all the members of the set value stored at key.

This has the same effect as running SINTER with one argument key.
### Time complexity: O(N) where N is the set cardinality.
### Return
Array reply: all elements of the set.

https://redis.io/commands/scard/