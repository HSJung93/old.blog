---
title: "Clean Code"
date: 2022-03-27T14:17:00+09:00
categories:
  - Memo
tags:
  - clean_code
# header:
#   teaser: /assets/images/code.jpg
---

## 3. Functions

### Small
- This implies that the blocks within if statements, else statements, while statements, and so on should be on line long. Probably that line should be a function call. Not only does this keep the enclosing function small, but it also adds documentary value because the function called within the block can have a nicely descriptive name. 
- This also implies that function should not be large enough to hold nested structures. Therefore, the indent level of a function should not be greater than one or two. 


### Function Arguments
- One input argument is the next best thing to no arguments. `SetupTeardownIncluder.render(pageData)` is pretty easy to understand. Clearly we are going to render the data in the `pageData` object.
- Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. 

### Command Query Separation
- Functions should either do something or answer something, but not bot. Either your function should change the state of an object, or it should return some information about that object.

### How Do You Write Functions Like This?
- When I write functions, they come out long and complicated. They have lots of indenting and nested loops. They have long argument lists. The names are arbitrary, and there is duplicated code. But I also have a suite of unit tests that cover every one of those clumsy lines of code.
- So then I massage and refine that code, splitting out functions, changin names, eliminating duplication. I shrink the methods and reorder them. Sometimes I break out whole classes, all the while keeping the tests passing.

### Conclusion
- Master programmers think of systems as stories to be told rather than programs to be written.