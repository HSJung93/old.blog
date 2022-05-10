---
title: "Testing Spring Filters Without Pain"
date: 2022-05-06T15:34:00+09:00
categories:
  - Memo
tags:
  - Spring
  - Filters
  - Kotlin
# header:
#   teaser: /assets/images/code.jpg
---

The Spring framework has grown and changed at a massive pace over the last years. It has evolved from XML configured beans to annotation based beans, from synchronous to a non-blocking and reactive programming paradigm, from application server deployments to stand-alone microservice deployments.

The `org.springframework.web.filter.GenericFilterBean` is the base class you should extend when creating your own filters. It simplifies the filter initialisation by being bean-friendly and reduces the number of methods to implement from the servlet Filter interface to just one, the classic doFilter(ServletRequest request, ServletResponse response, FilterChain chain). This method gives you the opportunity to observe the request or response and take action accordingly, for example, vetoing the rest of the filter chain.

Imagine that you have to create a http filter that injects the email from the user who initiated the request, into each controller of your app. The filter will get the user id from the query parameters, call a helper service to get the email for that user and finally pass this information down the filter chain.

Since http requests are immutable, the only possible way to pass additional information down the filter chain is by setting attributes to the request. Setting a request attribute from the filter is straightforward, but the contract of how to use the attribute in the controller is a bit loose, so you might want to capture this well in your filter test.

Before writing the test, let’s start by defining a simple skeleton for our base filter class:

```java
@Component
class UserFilter(private val userService: UserService) : GenericFilterBean() {
      override fun doFilter(request: ServletRequest, response: ServletResponse, chain: FilterChain) {
         // implementation goes here
      }
}
```

Notice that the filter implements the GenericFilterBean mentioned above and takes as parameter a handy UserService, which will allow us to obtain the email from a given user id. This service is obviously defined by ourselves, and for this example we are going to use an interface and forget about its actual implementation.

The signature for the UserService interface is the following:

```java
interface UserService {
      fun getUserEmail(userId: String): String
}
```

If we think about the implementation of the filter, these are the steps we need to follow in the code:

1. extract the userId from the query parameters
2. find the email associated to the user id via the userService
3. set the email as a an attribute to the request
4. continue down the filter chain

This implementation captures the steps outlined above:

```java
override fun doFilter(request: ServletRequest, response: ServletResponse, chain: FilterChain) {
      val userId = request.getParameter("userId") // 1
      val userEmail = userService.getUserEmail(userId) // 2
      request.setAttribute("userEmail", userEmail) // 3
      chain.doFilter(request, response) // 4
}
```

## Test approach 1: Death by 1,000 mocks
The first way to test that a filter is doing its job is by observing the interactions it plays with the objects that are given to it (in this case the request, response and chain) and then verify that it interacts as expected by calling the right methods of the objects it has references to. *We will see that this way of testing is very hairy and highly ineffective!*

```kotlin
@Test
fun `user filter adds the user email in a request attribute`() {
      // mock the request, response and chain:
      val request = mock(ServletRequest::class.java)
      val response = mock(ServletResponse::class.java)
      val chain = mock(FilterChain::class.java)

      // stub the ServletRequest.getParameter method to return “13” each time we ask for the userId value in the query parameter string:
      Mockito.`when`(request.getParameter("userId")).thenReturn("1")

      //  stub our userService and return the email of an arbitrary user each time a user with an id 13 comes along:
      val userService: UserService = Mockito.mock(UserService::class.java)
      Mockito.`when`(userService.getUserEmail("13")).thenReturn("han.solo@rebelalliance.com")

      // Now we create the filter and we pass the userService that will return Han Solo’s email whenever a user id of 13 is present:
      val userFilter = UserFilter(userService)

      // now invoke the doFilter method providing the arguments mocked above:
      userFilter.doFilter(request, response, chain)

      // verify the interaction with the mocked objects
      verify(request).setAttribute("userEmail", "han.solo@rebelalliance.com")
      verify(chain).doFilter(request, response)
      verifyNoMoreInteractions(chain)
      verifyZeroInteractions(response)
}
```
What just happened here? We have just stepped into mock hell and we need to get out of there quickly. So how can we escape mock hell? There are better ways to test a http filter. Spring provides awesome tools to test the web layer.

## Test approach 2: Using a real controller in the test to interact with the filter
Let’s go back to our test class and create a private controller that will return, in the body of the response, the request attribute value that the filter is supposed to inject into the request object:

```kotlin
@RestController
private class TestController {
   @GetMapping("/test")
   fun test(@RequestAttribute userEmail: String): String = userEmail
}
```

The first step to escape from the mock hell created before is to remove all the contents from the previous test. The only thing we are going to keep is stubbing the UserService. 

The next step is to use the MockMvcBuilders class from Spring and add the controller just created, along with the filter:



```kotlin
@Test
fun `user filter adds the user email in a request attribute`() {
      // Here it makes sense to stub the retrieval of the user email and delegate this to a mock.
   val userService: UserService = Mockito.mock(UserService::class.java)

      // The next step is to use the MockMvcBuilders class from Spring and add the controller just created, along with the filter:
   Mockito.`when`(userService.getUserEmail("1")).thenReturn("han.solo@rebelalliance.com")
   val mockMvc = MockMvcBuilders
           .standaloneSetup(TestController())
           .addFilter<StandaloneMockMvcBuilder>(UserFilter(userService))
           .build()

      // Finally, we call this mockMvc instance with a URL that maps to the test controller and with a query parameter including the userId with a value of “13”.
   mockMvc
           .perform(MockMvcRequestBuilders.get("/test?userId=1"))
           .andExpect(status().isOk)
           .andExpect(content().string("han.solo@rebelalliance.com"))
}
```
 
If you add a parameter to the controller function with the annotation @RequestAttribute, your controller will extract that parameter injected from the filter and you will be able to consume it.

You could argue that this is not a unit-test because we are testing it with different layers of the application, like the controller defined in the test. I prefer not to obsess too much about which layer I am testing the code on, but rather ask myself whether I am accurately capturing the essence of the class I am testing and making sure I don’t end up in mock hell as option 1 leads to.

https://tanzu.vmware.com/content/blog/testing-spring-filters-without-pain#:~:text=The%20first%20way%20to%20test,objects%20it%20has%20references%20to.