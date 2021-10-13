---
title: "스프링 부트의 ApplicationContext"
date: 2021-10-13T19:23:00+09:00
categories:
  - 메모
tags:
  - 디스패처 서블릿
  - 스프링부트
header:
  teaser: /assets/images/slipBox.jpg
---

# 2가지 Application Context

스프링은 아파치와 같이 정적인 요청을 제안하지 않는다. 스프링에서는 외부에서 request가 들어오면 서블릿을 요청한다. 따라서 톰캣에서 request를 받으면 가장 먼저 web.xml에서 스프링 내부에 들어가기 위해서 **순서대로** 두가지 일을한다.

1. 컨텍스트 로더 리스너 생성
2. 디스패처 서블릿 구동

## 컨텍스트 로더 리스너

1. 컨텍스트 로더 리스너는 스레드별로 사용하는 것이 아니라, **모든 스레드가 공통으로 사용할 객체**(예를 들어서 DB 관련)를 메모리에 올린다(=스캔한다.).
   1. 이를 위해서 **root-ApplicationContext를 실행**한다.
      1. @Service, @Repository 등을 스캔하여 DB관련 객체들을 생성한다.

## 디스패처 서블릿

1. 디스패처 서블릿이 **컴포넌트 스캔**을 한다.
   1. 디스패처 서블릿은 **servlet-applicationContext를 실행**한다.
      1. 뷰 리졸버, 핸들러매핑, 컨트롤러, 인터셉터, 멀티파트 리졸버 객체를 생성하고, 웹과 Controller, RestController를 스캔한다.
      2. servlet-applicationContext에서는 root-ApplicationContext가 로드한 객체를 참조할 수 있지만 반대는 불가능하다.
   2. 특정 패키지 이하 자바 파일을 전부 스캔해서, 클래스 파일을 메모리에 올리고 new를 하는 것이 아니라 IoC를 한다.
   3. 무엇을 메모리에 올려야 할지를 스프링에서 어노테이션으로 정해두었다. @Controller, @RestController, @Configuration, @Repository, @Service 등등
2. 디스패처 서블릿은 주소 분배를 한다.

## 디스패처 서블릿은 스프링부트의 어디에 있는가?

디스패처 서블릿은

스프링 부트에서는
`org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration`
에 존재한다.

스프링 프레임워크의 `org.springframework.web.servlet.DispatcherServlet`에 존재한다.

## IoC란?

1. 컴포넌트 스캔으로 객체들이 생성되면 IoC 컨테이너에서 관리한다.
2. 수많은 객체들이 ApplicationContext에 등록되는 것이 IoC이다.
3. 나중에 필요한 곳에서 ApplicationContext에 접근해서 필요한 객체를 가져온다.
4. ApplicationContext는 싱글톤으로 관리되기 때문에 어디에서 접근하든 동일한 객체임을 보장한다.

## ApplicationContext vs Bean Factory

1. @Configuration 어노테이션이 있는 클래스 안에 객체를 리턴하는 메소드는 @Bean으로 메모리에 띄울 수 있다.
2. 그런데 필요할 때 getBean()이라는 메소드를 통하여 호출할 수 있다. 즉 lazy-loading이 가능하다.
