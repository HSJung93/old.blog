---
title: "개발 언어와 인코딩"
date: 2021-12-15T10:04:00+09:00
categories:
  - 블로그
tags:
  - 서비스_개발
header:
  teaser: /assets/images/slipBox.jpg
---

## 프로그래밍 언어

언어가 아주 많고, 모든 언어를 배울 수는 없다. 퍼포먼스와 생산성을 고려해야 한다. 백엔드 엔지니어에게는 퍼포먼스가 중요하지만 생산성도 점점 더 고려가 되는 추세. 이미 배운 언어를 중심으로 커리어가 쌓인다. 

### C++, C++++

어렵기로 유명하고 많이 배워야 한다. C#은 윈도우즈에만 돌아간다. 당나귀에 단 강력한 레이저 라이플로 비유된다.

### JAVA, 코틀린

한국을 장악하고 있고 전세계에서도 많이 쓰인다. JVM이 중요하다. 오라클의 저작권 문제로 구글은 코틀린으로 전환하는 추세이다. 코틀린은 자바와 100% 상호운용이 된다. 굉장히 많은 대기업들이 코틀린으로 전환하고 있고 신규 프로젝트에 많이 사용하고 있다. 자바 스프링을 사용한다면 코틀린을 사용하지 않을 수 없을 것이다. 안드로이드는 거의 자바에서 코틀린으로 옮겨갔다. 생산성이 정말 떨어진다. 

### 자바스크립트

대세이다. 백엔드를 위한 언어는 아니기에 큰 프레임워크에 사용하기는 어렵다. 콜백 지옥이 규모가 커지면 찾을 수가 없다. 배우기는 쉽지만 변화를 따라잡기가 어렵다. 리액트가 그 중에서도 대세이다. 

### Python

인기와 생산성 측면에서 굉장하다.

### Go

고루틴이라는 강력한 멀티코어 처리. 배포가 쉽다. exe로 떨어진다. 제너릭의 부재로 박싱, 언박싱이 불필요하게 일어난다. 저수준 시스템 개발은 가비지 컬렉션과 고루틴을 위한 무거운 런타임 때문에 불가능하다. 안전하지 않은 타입 케스팅을 유발한다. 템플릿도 매크로도 없어 코드 반복이 많아진다. 도커, 드랍박스, 우버에서 사용되는데 한국에서 레퍼런스가 부족하다. 함수형 특징을 가진 언어이다. 

### 루비

아름다운 언어지만 배포(버전 관리)가 너무 편하다.

### Flutter

필요할 때 그 때 빠르게 배우면 되는 언어. 

## Encoding

* 아스키 코드: A-65, a-97, enter-13, space-32, esc-27
* 윈도우: 완성형, 맥: 조합형
* 유니 코드: EUC-KR, CP949, UTF-8(아스키 코드, 유니코드 결합), UTF-16(리눅스의 기본)
* Byte Order Mark: 유니코드 파일이 시작되는 첫부분에 보이지 않게 2-3 바이트의 문자열을 추가하는 것: UTF-8(EF BB BF)
* Normalization Forms: 문자열을 표시하는 방식: 윈도우즈와 리눅스는 NFC, 맥은 NFD: 