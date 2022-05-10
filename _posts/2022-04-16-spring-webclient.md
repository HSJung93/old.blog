---
title: "Spring WebClient"
date: 2022-04-16T10:25:00+09:00
categories:
  - Memo
tags:
  - Spring
  - WebClient
  - Test
# header:
#   teaser: /assets/images/code.jpg
---
## Working with the WebClient
### What is the WebClient?
WebClient is an interface representing the main entry point for performing web requests. It was created as part of the Spring Web Reactive module, and will be replacing the classic RestTemplate in these scenarios. In addition, the new client is a reactive, non-blocking solution that works over the HTTP/1.1 protocol. It's important to note that even though it is, in fact, a non-blocking client and it belongs to the spring-webflux library, the solution offers support for both synchronous and asynchronous operations, making it suitable also for applications running on a Servlet Stack. Finally, the interface has a single implementation, the DefaultWebClient class, which we'll be working with.

### Creating with the WebClient
There are three options to choose from. The first one is creating a WebClient object with default settings. The second option is to initiate a WebClient instance with a given base URI. The third option (and the most advanced one) is building a client by using the DefaultWebClientBuilder class, which allows full customization. 
```java
WebClient client = WebClient.create();

WebClient client = WebClient.create("http://localhost:8080");

WebClient client = WebClient.builder()
  .baseUrl("http://localhost:8080")
  .defaultCookie("cookieKey", "cookieValue")
  .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE) 
  .defaultUriVariables(Collections.singletonMap("url", "http://localhost:8080"))
  .build();
```

Oftentimes, the default HTTP timeouts of 30 seconds are too slow for our needs, to customize this behavior, we can create an HttpClient instance and configure our WebClient to use it.

We can:
* set the connection timeout via the ChannelOption.CONNECT_TIMEOUT_MILLIS option
* set the read and write timeouts using a ReadTimeoutHandler and a WriteTimeoutHandler, respectively
* configure a response timeout using the responseTimeout directive

```java
HttpClient httpClient = HttpClient.create()
  .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 5000)
  .responseTimeout(Duration.ofMillis(5000))
  .doOnConnected(conn -> 
    conn.addHandlerLast(new ReadTimeoutHandler(5000, TimeUnit.MILLISECONDS))
      .addHandlerLast(new WriteTimeoutHandler(5000, TimeUnit.MILLISECONDS)));

WebClient client = WebClient.builder()
  .clientConnector(new ReactorClientHttpConnector(httpClient))
  .build();
```

Note that while we can call timeout on our client request as well, this is a signal timeout, not an HTTP connection, a read/write, or a response timeout; it's a timeout for the Mono/Flux publisher.

### Making a request
First, we need to specify an HTTP method of a request by invoking method(HttpMethod method). Or calling its shortcut methods such as get, post, and delete:
```java
UriSpec<RequestBodySpec> uriSpec = client.method(HttpMethod.POST);

UriSpec<RequestBodySpec> uriSpec = client.post();
```

> Note: although it might seem we reuse the request spec variables (WebClient.UriSpec, WebClient.RequestBodySpec, WebClient.RequestHeadersSpec, WebClient.ResponseSpec), this is just for simplicity to present different approaches. These directives shouldn't be reused for different requests, they retrieve references, and therefore the latter operations would modify the definitions we made in previous steps.

The next step is to provide a URL. Once again, we have different ways of doing this. We can pass it to the uri API as a String or use a UriBuilder Function or as a java.net.URL instance:
```java
RequestBodySpec bodySpec = uriSpec.uri("/resource");

RequestBodySpec bodySpec = uriSpec.uri(
  uriBuilder -> uriBuilder.pathSegment("/resource").build());

RequestBodySpec bodySpec = uriSpec.uri(URI.create("/resource"));
```
Keep in mind that if we defined a default base URL for the WebClient, this last method would override this value.

Then we can set a request body, content type, length, cookies, or headers if we need to. For example, if we want to set a request body, there are a few available ways. Probably the most common and straightforward option is using the bodyValue method or by presenting a Publisher (and the type of elements that will be published) to the body method:
```java
RequestHeadersSpec<?> headersSpec = bodySpec.bodyValue("data");

RequestHeadersSpec<?> headersSpec = bodySpec.body(
  Mono.just(new Foo("name")), Foo.class);
```

Alternatively, we can make use of the BodyInserters utility class. For example, let's see how we can fill in the request body using a simple object as we did with the bodyValue method. Similarly, we can use the BodyInserters#fromPublisher method if we are using a Reactor instance. 
```java
RequestHeadersSpec<?> headersSpec = bodySpec.body(
  BodyInserters.fromValue("data"));

RequestHeadersSpec headersSpec = bodySpec.body(
  BodyInserters.fromPublisher(Mono.just("data")),
  String.class);
```

This class also offers other intuitive functions to cover more advanced scenarios. For instance, in case we have to send multipart requests:
```java
LinkedMultiValueMap map = new LinkedMultiValueMap();
map.add("key1", "value1");
map.add("key2", "value2");
RequestHeadersSpec<?> headersSpec = bodySpec.body(
  BodyInserters.fromMultipartData(map));
```

All these methods create a BodyInserter instance that we can then present as the body of the request. The BodyInserter is an interface responsible for populating a ReactiveHttpOutputMessage body with a given output message and a context used during the insertion. A Publisher is a reactive component in charge of providing a potentially unbounded number of sequenced elements. It is an interface too, and the most popular implementations are Mono and Flux.

After we set the body, we can set headers, cookies, and acceptable media types. Values will be added to those that have already been set when instantiating the client. Also, there is additional support for the most commonly used headers like “If-None-Match”, “If-Modified-Since”, “Accept”, and “Accept-Charset”. Here's an example of how these values can be used:
```java
ResponseSpec responseSpec = headersSpec.header(
    HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
  .accept(MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML)
  .acceptCharset(StandardCharsets.UTF_8)
  .ifNoneMatch("*")
  .ifModifiedSince(ZonedDateTime.now())
  .retrieve();
```

### Handling the response
The final stage is sending the request and receiving a response. We can achieve this by using either the `exchangeToMono/exchangeToFlux` or the retrieve method. The `exchangeToMono` and `exchangeToFlux` methods allow access to the ClientResponse along with its status and headers. While the `retrieve` method is the shortest path to fetching a body directly:

```java
Mono<String> response = headersSpec.exchangeToMono(response -> {
  if (response.statusCode()
    .equals(HttpStatus.OK)) {
      return response.bodyToMono(String.class);
  } else if (response.statusCode()
    .is4xxClientError()) {
      return Mono.just("Error response");
  } else {
      return response.createException()
        .flatMap(Mono::error);
  }
});

Mono<String> response = headersSpec.retrieve()
  .bodyToMono(String.class);
```
It's important to pay attention to the ResponseSpec.bodyToMono method, which will throw a WebClientException if the status code is 4xx (client error) or 5xx (server error).

### Spring Webflux by example
Similar to RestTemplate and AsyncRestTemplate, in the WebFlux stack, Spring adds a WebClient to perform HTTP requests and interact with HTTP APIs. The following is a simple example of using WebClient to send a GET request to the /posts URI and retrieve posts.
```java
WebClient client = WebClient.create("http://localhost:8080");
client
	.get()
	.uri("/posts")
	.exchange()
	.flatMapMany(res -> res.bodyToFlux(Post.class))
	.log()
	.subscribe(post -> System.out.println("post: " + post));
```


## Working with the WebTestClient
The WebTestClient is the main entry point for testing WebFlux server endpoints. It has a very similar API to the WebClient, and it delegates most of the work to an internal WebClient instance focusing mainly on providing a test context. The DefaultWebTestClient class is a single interface implementation. 

The client for testing can be bound to a real server or work with specific controllers or functions. To complete end-to-end integration tests with actual requests to a running server, we can use the bindToServer method:
```java
WebTestClient testClient = WebTestClient
  .bindToServer()
  .baseUrl("http://localhost:8080")
  .build();
```

We can test a particular RouterFunction by passing it to the bindToRouterFunction method:

```java
RouterFunction function = RouterFunctions.route(
  RequestPredicates.GET("/resource"),
  request -> ServerResponse.ok().build()
);

WebTestClient
  .bindToRouterFunction(function)
  .build().get().uri("/resource")
  .exchange()
  .expectStatus().isOk()
  .expectBody().isEmpty();
```

The same behavior can be achieved with the bindToWebHandler method, which takes a WebHandler instance:

```java
WebHandler handler = exchange -> Mono.empty();
WebTestClient.bindToWebHandler(handler).build();
```

A more interesting situation occurs when we're using the bindToApplicationContext method. It takes an ApplicationContext and analyses the context for controller beans and `@EnableWebFlux` configurations.

If we inject an instance of the ApplicationContext, a simple code snippet may look like this:

```java
@Autowired
private ApplicationContext context;

WebTestClient testClient = WebTestClient.bindToApplicationContext(context)
  .build();
```

A shorter approach would be providing an array of controllers we want to test by the bindToController method. Assuming we've got a Controller class and we injected it into a needed class, we can write:

```java
@Autowired
private Controller controller;

WebTestClient testClient = WebTestClient.bindToController(controller).build();
```

After building a WebTestClient object, all following operations in the chain are going to be similar to the WebClient until the exchange method (one way to get a response), which provides the WebTestClient.ResponseSpec interface to work with useful methods like the expectStatus, expectBody, and expectHeader:

```java
WebTestClient
  .bindToServer()
    .baseUrl("http://localhost:8080")
    .build()
    .post()
    .uri("/resource")
  .exchange()
    .expectStatus().isCreated()
    .expectHeader().valueEquals("Content-Type", "application/json")
    .expectBody().jsonPath("field").isEqualTo("value");
```