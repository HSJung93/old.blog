---
title: "Spring Webflux Filters"
date: 2022-04-14T10:25:00+09:00
categories:
  - Memo
tags:
  - Spring
  - Webflux
  - Filters
# header:
#   teaser: /assets/images/code.jpg
---
### Endpoints
We have to create some endpoints first. One for each method: annotation-based and functional-based.

Let's start with the annotation-based controller:
```java
@GetMapping(path = "/users/{name}")
public Mono<String> getName(@PathVariable String name) {
    return Mono.just(name);
}
```

For the functional endpoint we have to create a handler first:
```java
@Component
public class PlayerHandler {
    public Mono<ServerResponse> getName(ServerRequest request) {
        Mono<String> name = Mono.just(request.pathVariable("name"));
        return ok().body(name, String.class);
    }
}
```

And also a router configuration mapping:
```java
@Bean
public RouterFunction<ServerResponse> route(PlayerHandler playerHandler) {
    return RouterFunctions
      .route(GET("/players/{name}"), playerHandler::getName)
      .filter(new ExampleHandlerFilterFunction());
}
```

### Types of WebFlux Filters
The WebFlux framework provides two types of filters: WebFilters and HandlerFilterFunctions.

The main difference between them is that WebFilter implementations work for all endpoints and HandlerFilterFunction implementations will only work for Router-based ones.

#### WebFilter
We'll implement a WebFilter to add a new header to the response. As a result, all responses should have this behavior:
```java
@Component
public class ExampleWebFilter implements WebFilter {
 
    @Override
    public Mono<Void> filter(ServerWebExchange serverWebExchange, 
      WebFilterChain webFilterChain) {
        
        serverWebExchange.getResponse()
          .getHeaders().add("web-filter", "web-filter-test");
        return webFilterChain.filter(serverWebExchange);
    }
}
```

#### HandlerFilterFunction
For this one, we implement a logic that sets the HTTP status to FORBIDDEN when the “name” parameter is equal to “test”.
```java
public class ExampleHandlerFilterFunction 
  implements HandlerFilterFunction<ServerResponse, ServerResponse> {
 
    @Override
    public Mono<ServerResponse> filter(ServerRequest serverRequest,
      HandlerFunction<ServerResponse> handlerFunction) {
        if (serverRequest.pathVariable("name").equalsIgnoreCase("test")) {
            return ServerResponse.status(FORBIDDEN).build();
        }
        return handlerFunction.handle(serverRequest);
    }
}
```

### Testing

In WebFlux Framework there's an easy way to test our filters: the WebTestClient. It allows us to test HTTP calls to our endpoints.

Here are examples of the annotation-based endpoint:
```java
@Test
public void whenUserNameIsBaeldung_thenWebFilterIsApplied() {
    EntityExchangeResult<String> result = webTestClient.get()
      .uri("/users/baeldung")
      .exchange()
      .expectStatus().isOk()
      .expectBody(String.class)
      .returnResult();

    assertEquals(result.getResponseBody(), "baeldung");
    assertEquals(
      result.getResponseHeaders().getFirst("web-filter"), 
      "web-filter-test");
}

@Test
public void whenUserNameIsTest_thenHandlerFilterFunctionIsNotApplied() {
    webTestClient.get().uri("/users/test")
      .exchange()
      .expectStatus().isOk();
}
```

And for the functional endpoint:
```java
@Test
public void whenPlayerNameIsBaeldung_thenWebFilterIsApplied() {
    EntityExchangeResult<String> result = webTestClient.get()
      .uri("/players/baeldung")
      .exchange()
      .expectStatus().isOk()
      .expectBody(String.class)
      .returnResult();

    assertEquals(result.getResponseBody(), "baeldung");
    assertEquals(
      result.getResponseHeaders().getFirst("web-filter"),
      "web-filter-test");
} 

@Test 
public void whenPlayerNameIsTest_thenHandlerFilterFunctionIsApplied() {
    webTestClient.get().uri("/players/test")
      .exchange()
      .expectStatus().isForbidden(); 
}
```