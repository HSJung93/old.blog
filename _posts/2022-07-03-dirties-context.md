---
title: "DirtiesContext Annotation"
date: 2022-07-03T13:15:00+09:00
categories:
  - Memo
tags:
  - spring
  - annotation
  - DirtiesContext
# header:
#   teaser: /assets/images/code.jpg
---

### @DirtiesContext

@DirtiesContext is a Spring testing annotation. It indicates the associated test or class modifies the ApplicationContext. It tells the testing framework to close and recreate the context for later tests. We can annotate a test method or an entire class. By setting the MethodMode or ClassMode, we can control when Spring marks the context for closure. If we place @DirtiesContext on a class, the annotation applies to every method in the class with the given ClassMode.

### Testing Without Clearing the Spring Context

Let's say we have a User:

```java
public class User {
    String firstName;
    String lastName;
}
```

We also have a very simple UserCache:

```java
@Component
public class UserCache {

    @Getter
    private Set<String> userList = new HashSet<>();

    public boolean addUser(String user) {
        return userList.add(user);
    }

    public void printUserList(String message) {
        System.out.println(message + ": " + userList);
    }

}
```

We create an integration test to load up and test the full application:

```java
@TestMethodOrder(OrderAnnotation.class)
@ExtendWith(SpringExtension.class)
@SpringBootTest(classes = SpringDataRestApplication.class)
class DirtiesContextIntegrationTest {

    @Autowired
    protected UserCache userCache;
    
    ...
}
```

The first method, addJaneDoeAndPrintCache, adds an entry to the cache. After adding a user to the cache, it prints the contents of the cache:

```java
@Test
@Order(1)
void addJaneDoeAndPrintCache() {
    userCache.addUser("Jane Doe");
    userCache.printUserList("addJaneDoeAndPrintCache");
}
// addJaneDoeAndPrintCache: [Jane Doe]
```

Next, printCache prints the user cache again. It contains the name added in the previous test:

```java
@Test
@Order(2)
void printCache() {
    userCache.printUserList("printCache");
}
// printCache: [Jane Doe]
```

Let's say a later test was relying on an empty cache for some assertions. The previously inserted names may cause undesired behavior.

### Using @DirtiesContext

Now we'll show @DirtiesContext with the default MethodMode, AFTER_METHOD. This means Spring will mark the context for closure after the corresponding test method completes. The addJohnDoeAndPrintCache test method adds a user to the cache. We have also added the @DirtiesContext annotation, which says the context should shut down at the end of the test method:

```java
@DirtiesContext(methodMode = MethodMode.AFTER_METHOD)
@Test
@Order(3)
void addJohnDoeAndPrintCache() {
    userCache.addUser("John Doe");
    userCache.printUserList("addJohnDoeAndPrintCache");
}
// addJohnDoeAndPrintCache: [John Doe, Jane Doe]
```

Finally, printCacheAgain prints the cache again:

```java
@Test
@Order(4)
void printCacheAgain() {
    userCache.printUserList("printCacheAgain");
}
// printCacheAgain: []
```

### Other Supported Test Phases

#### Class Level

1. BEFORE_CLASS: Before current test class
2. BEFORE_EACH_TEST_METHOD: Before each test method in the current test class
3. AFTER_EACH_TEST_METHOD: After each test method in the current test class
4. AFTER_CLASS: After the current test class

#### Method Level

1. BEFORE_METHOD: Before the current test method
2. AFTER_METHOD: After the current test method
