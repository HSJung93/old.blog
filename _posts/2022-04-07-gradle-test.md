---
title: "What is Webhook?"
date: 2022-04-07T09:24:00+09:00
categories:
  - Memo
tags:
  - gradle
  - test
# header:
#   teaser: /assets/images/code.jpg
---
### Table of Contents
* Test execution
* Test filtering
* Test reporting
* Test detection
* Test grouping

### The basics
All JVM testing revolves around a single task type: `Test`. This runs a collection of test cases using any supported test library — JUnit, JUnit Platform or TestNG — and collates the results. You can then turn those results into a report via an instance of the `TestReport` task type.

* In order to operate, the `Test` task type requires just two pieces of information:
* Where to find the compiled test classes (property: `Test.getTestClassesDirs()`)

The execution classpath, which should include the classes under test as well as the test library that you’re using (property: `Test.getClasspath()`)

When you’re using a JVM language plugin — such as the `Java Plugin` — you will automatically get the following:

* A dedicated `test` source set for unit tests
* A `test` task of type Test that runs those unit tests

All you need to do in most cases is configure the appropriate compilation and runtime dependencies and add any necessary configuration to the `test` task. The following example shows a simple setup that uses JUnit 4.x and changes the maximum heap size for the tests' JVM to 1 gigabyte:

```kotlin
dependencies {
    testImplementation("junit:junit:4.12")
}

tasks.test {
    useJUnit()

    maxHeapSize = "1G"
}
```

The Test task has many generic configuration options as well as several framework-specific ones that you can find described in `JUnitOptions`, `JUnitPlatformOptions` and `TestNGOptions`.

### Test execution
Gradle executes tests in a separate ('forked') JVM, isolated from the main build process. This prevents classpath pollution and excessive memory consumption for the build process. It also allows you to run the tests with different JVM arguments than the build is using.

You can control how the test process is launched via several properties on the Test task, including the following:

#### `maxParallelForks` — default: 1
You can run your tests in parallel by setting this property to a value greater than 1. This may make your test suites complete faster, particularly if you run them on a multi-core CPU. When using parallel test execution, make sure your tests are properly isolated from one another. Tests that interact with the filesystem are particularly prone to conflict, causing intermittent test failures. Your tests can distinguish between parallel test processes by using the value of the `org.gradle.test.worker` property, which is unique for each process.

#### `forkEvery` - default:0
This property specifies the maximum number of test classes that Gradle should run on a test process before its disposed of and a fresh one created.

#### `ignoreFailures` - default: false
If this property is `true`, Gradle will continue with the project’s build once the tests have completed, even if some of them have failed. Note that, by default, the `Test` task always executes every test that it detects, irrespective of this setting.

#### `failFast` —  (since Gradle 4.6) default: false
Set this to `true` if you want the build to fail and finish as soon as one of your tests fails.

### Test filtering
It’s a common requirement to run subsets of a test suite, such as when you’re fixing a bug or developing a new test case. Gradle provides two mechanisms to do this:

* Filtering (the preferred option)

* Test inclusion/exclusion

Filtering supersedes the inclusion/exclusion mechanism.

The command-line option is especially useful to execute a single test method. When you use `--tests`, be aware that the inclusions declared in the build script are still honored.