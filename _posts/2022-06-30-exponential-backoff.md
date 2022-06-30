---
title: "Multipart form data"
date: 2022-06-30T11:34:00+09:00
categories:
  - Memo
tags:
  - exponential backoff
# header:
#   teaser: /assets/images/code.jpg
---

## Exponential backoff

Exponential backoff is an algorithm that uses feedback to multiplicatively decrease the rate of some process, in order to gradually find an acceptable rate. An exponential backoff algorithm is a form of closed-loop control system that reduces the rate of a controlled process in response to adverse events. Each time an adverse event is encountered, the rate of the process is reduced by some multiplicative factor. 

## Spring Framework 5.3.21

`org.springframework.util.backoff.ExponentialBackOff`

Implementation of `BackOff` that increases the back off period for each retry attempt. When the interval has reached the `max interval`, it is no longer increased. Stops retrying once the `max elapsed time` has been reached.

Note that the default max elapsed time is `Long.MAX_VALUE`. Use `setMaxElapsedTime(long)` to limit the maximum length of time that an instance should accumulate before returning `BackOffExecution.STOP`.