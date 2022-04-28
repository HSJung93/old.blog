---
title: "What Is Undertow?"
date: 2022-04-28T13:03:00+09:00
categories:
  - Memo
tags:
  - Server
  - Undertow
# header:
#   teaser: /assets/images/code.jpg
---
## Undertow
Undertow is a flexible performant web server written in java, providing both blocking and non-blocking API's based on NIO. Undertow has a composition based architecture that allows you to build a web server by combining small single purpose handlers.

## Show me the code
```java
public class HelloWorldServer {

    public static void main(final String[] args) {
        Undertow server = Undertow.builder()
                .addHttpListener(8080, "localhost")
                .setHandler(new HttpHandler() {
                    @Override
                    public void handleRequest(final HttpServerExchange exchange) throws Exception {
                        exchange.getResponseHeaders().put(Headers.CONTENT_TYPE, "text/plain");
                        exchange.getResponseSender().send("Hello World");
                    }
                }).build();
        server.start();
    }
}
```