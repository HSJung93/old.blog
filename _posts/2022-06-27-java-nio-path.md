---
title: "Java NIO Path"
date: 2022-06-27T11:34:00+09:00
categories:
  - Memo
tags:
  - Java
  - NIO
# header:
#   teaser: /assets/images/code.jpg
---

A Java Path instance represents a path in the file system. A path can point to either a file or a directory. A path can be absolute or relative. An absolute path contains the full path from the root of the file system down to the file or directory it points to. A relative path contains the path to the file or directory relative to some other path.

## Creating a Path Instance

- create a `Path` instance using a static method in the `Paths` class(`java.nio.file.Paths`) named `Paths.get()`.
- use the `Path` interface and the `Paths` class.
- The `Paths.get()` method is a factory method for `Path` instances.

```java
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathExample {

    public static void main(String[] args) {

        Path path = Paths.get("c:\\data\\myfile.txt");

    }
}
```

### Creating an Relative Path

- A relative path is a path that points from one path(the base path) to a directory or file.

```java
Path projects = Paths.get("d:\\data", "projects");

Path file     = Paths.get("d:\\data", "projects\\a-project\\myfile.txt");
```
