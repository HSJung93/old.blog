---
title: "Redis SCARD SADD"
date: 2022-06-24T10:56:00+09:00
categories:
  - Memo
tags:
  - Redis
# header:
#   teaser: /assets/images/code.jpg
---

## 1. SCARD key

Returns the set cardinality(number of elements) of the set stored at key.

### 1.1. Time Complexity

- O(1)

### 1.2. Return

Integer reply: the cardinality(number of elements) of the set, or 0 if key does not exist.

### 1.3. Examples

```redis
SADD myset "Hello"
// 1
SADD myset "World"
// 1
SCARD myset
// 2
```

## 2. SADD key member [member ...]

Add the specified members to the set stored at key. Specified members that are already a member of this set are ignored. If key does no exist, a new set is created before adding the specified members.

An error is returned when the value stored at key is not a set.

### 2.1. Time Complexity

- O(1) for each element added, so O(N) to add N elements when the command is called with multiple arguments

### 2.2. Return

- Integer reply: the number of elements that were added to the set, not including all the elements already present in the set.

### 2.3. Examples

```redis
SADD myset "Hello"
// 1
SADD myset "World"
// 1 
SMEMBERS myset
// 1) "Hello"
// 2) "World"
```

## 3. SMEMBERS key

-Returns all the members of the set value stored at key.
-This has the same effect as running SINTER with one argument key.

### 3.1. Time complexity

- O(N) where N is the set cardinality

### 3.2. Return

- Array reply: all elements of the set.

## Reference

<https://redis.io/commands/scard/>
