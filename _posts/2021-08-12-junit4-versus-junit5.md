---
title: "JUnit4 vs JUnit5"
date: 2021-08-12T21:18:00+09:00
categories:
  - 메모
tags:
  - java
  - junit
header:
  teaser: /assets/images/code.jpg
---

JUnit4 에서 Junit5로의 변화를 정리해 보았습니다.

* build.gradle 변화
  * `   ` =>> `test{ useJUnitPlatform()}` 추가해야 합니다.
* 어노테이션 및 패키지 변화 
  * `@Test` : `org.junit.Test` =>> `org.junit.jupiter.api.Test`
  * `@RunWith` =>> `@ExtendWith`: `org.junit.runner.RunWith` ->> `org.junit.jupiter.api.extension.ExtendWith`
  * `SpringRunner` =>> `SpringExtension`: `org.springframework.test.context.junit4.SpringRunner` =>> `org.springframework.test.context.junit.jupiter.SpringExtension`
  * `@After` =>> `@AfterEach`: `org.junit.After` =>> `org.junit.jupiter.api.AfterEach`
  * `@Before` =>> `@BeforeEach`: `org.junit.Before` =>> `org.junit.jupiter.api.BeforeEach`