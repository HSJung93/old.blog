---
title: "Functional Web Framework in Spring 5"
date: 2022-04-14T17:00:00+09:00
categories:
  - Memo
tags:
  - Functional
  - Spring
# header:
#   teaser: /assets/images/code.jpg
---
### Introduction
We'll use the functional framework instead a simple reactive REST application using annotation-based components

### Functional Web Framework
A new programming model where we use functions to route and handle requests. As opposed to the annotation-based model where we use annotations mappings, here we'll use HandlerFunction and RouterFunctions. The HandlerFunction represents a function that generates responses for requests routed to them:

```java
@FunctionalInterface
public interface HandlerFunction<T extends ServerResponse> {
    Mono<T> handle(ServerRequest request);
}
```

This interface is primarily a Function<Request, Response<T>>, which behaves very much like a servlet. Although, compared to a standard Servlet#service(ServletRequest req, ServletResponse res), HandlerFunction doesn't take a response as an input parameter.

RouterFunction serves as an alternative to the @RequestMapping annotation. We can use it to route requests to the handler functions. Typically, we can import the helper function RouterFunctions.route()  to create routes, instead of writing a complete router function. It allows us to route requests by applying a RequestPredicate. When the predicate is matched, then the second argument, the handler function, is returned:
```java
@FunctionalInterface
public interface RouterFunction<T extends ServerResponse> {
    Mono<HandlerFunction<T>> route(ServerRequest request);
    // ...
    public static <T extends ServerResponse> RouterFunction<T> route(RequestPredicate predicate, HandlerFunction<T> handlerFunction)
}
```
Because the route() method returns a RouterFunction, we can chain it to build powerful and complex routing schemes.

### Reactive REST Application Using Functional Web
First, we need to create routes using RouterFunction to publish and consume our reactive streams of Employees. Routes are registered as Spring beans and can be created inside any configuration class.

```java
@Bean
RouterFunction<ServerResponse> getEmployeeByIdRoute() {
  return route(GET("/employees/{id}"), 
    req -> ok().body(
      employeeRepository().findEmployeeById(req.pathVariable("id")), Employee.class));
}

@Bean
RouterFunction<ServerResponse> getAllEmployeesRoute() {
  return route(GET("/employees"), 
    req -> ok().body(
      employeeRepository().findAllEmployees(), Employee.class));
}

@Bean
RouterFunction<ServerResponse> updateEmployeeRoute() {
  return route(POST("/employees/update"), 
    req -> req.body(toMono(Employee.class))
      .doOnNext(employeeRepository()::updateEmployee)
      .then(ok().build()));
}
```

The first argument is a request predicate. Notice how we used a statically imported RequestPredicates.GET method here. The second parameter defines a handler function that'll be used if the predicate applies.

### Composing Routes
Compose the routes together in a single router function.
```java
@Bean
RouterFunction<ServerResponse> composedRoutes() {
  return 
    route(GET("/employees"), 
      req -> ok().body(
        employeeRepository().findAllEmployees(), Employee.class))
        
    .and(route(GET("/employees/{id}"), 
      req -> ok().body(
        employeeRepository().findEmployeeById(req.pathVariable("id")), Employee.class)))
        
    .and(route(POST("/employees/update"), 
      req -> req.body(toMono(Employee.class))
        .doOnNext(employeeRepository()::updateEmployee)
        .then(ok().build())));
}
```

### Testing Routes
We can use WebTestClient to test our routes. To do so, we first need to bind the routes using the bindToRouterFunction method and then build the test client instance. Let's test our getEmployeeByIdRoute:

```java
@Test
public void givenEmployeeId_whenGetEmploteeById_thenCorrectEmployee(){
  WebTestClient client = WebTestClient
    .bindToRouterFunction(config.getEmployeeByIdRoute())
    .build();

  Employee employee = new Employee("1", "Employee 1");

  given(employeeRepository.findEmployeeById("1").willReturn(Mono.just(employee)));

  clinet.get()
    .uri("/employees/1")
    .exchange()
    .expectStatus()
    .isOk()
    .expectBody(Employee.class)
    .isEqualTo(employee);
}
}
```

and similarly getAllEmployeesRoute:

```java
@Test
public void whenGetAllEmployees_thenCorrectEmployees() {
    WebTestClient client = WebTestClient
      .bindToRouterFunction(config.getAllEmployeesRoute())
      .build();

    List<Employee> employees = Arrays.asList(
      new Employee("1", "Employee 1"),
      new Employee("2", "Employee 2"));

    Flux<Employee> employeeFlux = Flux.fromIterable(employees);
    given(employeeRepository.findAllEmployees()).willReturn(employeeFlux);

    client.get()
      .uri("/employees")
      .exchange()
      .expectStatus()
      .isOk()
      .expectBodyList(Employee.class)
      .isEqualTo(employees);
}
```
We can also test our updateEmployeeRoute by asserting that our Employee instance is updated via EmployeeRepository:

```java
@Test
public void whenUpdateEmployee_thenEmployeeUpdated() {
    WebTestClient client = WebTestClient
      .bindToRouterFunction(config.updateEmployeeRoute())
      .build();

    Employee employee = new Employee("1", "Employee 1 Updated");

    client.post()
      .uri("/employees/update")
      .body(Mono.just(employee), Employee.class)
      .exchange()
      .expectStatus()
      .isOk();

    verify(employeeRepository).updateEmployee(employee);
}
```