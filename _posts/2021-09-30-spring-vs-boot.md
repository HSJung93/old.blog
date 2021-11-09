---
title: "스프링 vs 스프링 부트"
date: 2021-09-30T14:09:00+09:00
categories:
  - 메모
tags:
  - 스프링
  - 스프링부트
header:
  teaser: /assets/images/slipBox.jpg
---

## 스프링부트는 스프링에 비해서 무엇이 달라졌나?

1. 편리한 의존성 관리 & 자동 권장 버전
  - 스프링에서는 모든 dependency를 버전까지 정확하게 입력해야 했으나, 스프링부트에서는 -starter-로 편리하게 입력
  - 설정 또한 WEB-INF/jsp가 아니라 application.properties나 yml 파일로 작성가능. e.g. 타임리프 설정

2. 내장 서버로 간단한 배포 서버 구축
  - 서버 구동 시간이 절반 가까이 단축
  - jetty도 가능
  - 내장 서블릿 컨테이너 덕분에 jar 파일로 빌드 후 간단 배포 가능

3. 스프링 시큐리티, Data JPA 등의 다른 스프링 프레임워크 요소들을 쉽게 사용
