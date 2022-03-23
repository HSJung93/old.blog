---
title: "Reactive Programming with Reactor 3"
date: 2022-03-23T17:31:00+09:00
categories:
  - Memo
tags:
  - reactive
# header:
#   teaser: /assets/images/code.jpg
---

# Introduction
Reactive Programming is a new paradigm in which you use declarative code (in a manner that is similar to functional programming) in order to build asynchronous processing pipelines. This is important in order to be more efficient with resources and increase an application's capacity to serve large number of clients, *without the headache of writing low-level concurrent or and/or parallelized code*. Reactive Programming is an alternative to the more limited ways of doing asynchronous code in the JDK: namely Callback based APIs and Future. It also facilitates composition, which in turn makes asynchronous code more readable and maintainable.

In reactive stream sequences, the source `Publisher` produces data. But by default, it does nothing until a `Subscriber` has registered (*subscribed*), at which point it will push data to it.

Reactor adds the concept of *operators*, which are chained together to describe what processing to apply at each stage to the data. Applying an operator returns a new intermediate `Publisher` (in fact it can be thought of as both a Subscriber to the operator upstream and a Publisher for downstream). The final form of the data ends up in the final `Subscriber` that defines what to do from a user perspective.

# Flux Instances 
A Flux<T> is a Reactive Streams Publisher, augmented with a lot of operators that can be used to generate, transform, orchestrate Flux sequences. It can emit 0 to n <T> elements (onNext event) then either completes or errors (onComplete and onError terminal events). If no terminal event is triggered, the Flux is infinite. Static factories on Flux allow to create sources, or generate them from several callbacks types. Instance methods, the operators, let you build an asynchronous processing pipeline that will produce an asynchronous sequence. Each Flux#subscribe() or multicasting operation such as Flux#publish and Flux#publishNext will materialize a dedicated instance of the pipeline and trigger the data flow inside it.

# Mono Instances
A `Mono<T>` is a Reactive Streams `Publisher`, also augmented with a lot of operators that can be used to generate, transform, orchestrate Mono sequences. It is a specialization of `Flux` that can emit at most 1 `<T>` element: a Mono is either valued (complete with element), empty (complete without element) or failed (error). Like for `Flux`, the operators can be used to define an asynchronous pipeline which will be materialized anew for each `Subscription`. Note that some API that change the sequence's cardinality will return a `Flux` (and vice-versa, APIs that reduce the cardinality to 1 in a `Flux` return a `Mono`).

