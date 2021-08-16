---
title: "직렬화(Serialization)와 역직렬화(Deserialization)"
date: 2021-08-15T21:21:15+09:00
categories:
  - 메모
tags:
  - serialization
  - deserialization
header:
  teaser: /assets/images/code.jpg
---

직렬화: 객체 형태의 데이터를 연속적인 형태의 데이터로 변환하는 것. 객체 형태의 데이터를 통째로 파일로 저장하거나 스트림을 통해 전송하고 싶을 때 사용한다. 

역직렬화: 직렬화된 데이터(파일)을 역으로 객체 형태로 변환하는 것. 저장된 파일을 읽거나 전송된 스트림 데이터를 읽어 객체 형태로 복원한다.

자바에서는 `Serializable` 인터페이스를 `implements`하여 해당 클래스를 직렬화한다. 보안 상의 이유로 클래스의 필드 일부를 제외하고 싶다면 `transient`로 지정해주면 된다.