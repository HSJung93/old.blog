---
title: "데이터베이스 테이블간의 조인 시 매핑들"
date: 2021-08-23T18:32:00+09:00
categories:
  - 메모
tags:
  - OneToOne
  - OneToMany
  - ManyToOne
  - ManyToMany
header:
  teaser: /assets/images/code.jpg
---
데이터베이스의 테이블 간의 조인 시에는 4가지 매핑이 존재한다.
1. `@OneToOne`: 사용자(user)와 사용자 상세정보(user_detail)를 테이블로 빼놓을 수 있다. 
2. `@OneToMany`: 하나의 사용자와 여러 게시판(board)를 작성하는 경우 
3. `@ManyToOne`: 여러개의 보드가 한 사용자에 작성이 된다.
4. `@ManyToMany`: 하나의 사용자가 여러 권한을 가질 수 있고, 하나의 권한(role)을 가지고 있는 여러 사용자가 존재한다.
   * 사용자, 권한, 둘을 연결하는 매핑 테이블(user_role)까지 3가지 테이블로 연결한다.
   * 매핑 테이블의 각 column은 사용자와 권한 테이블의 각 id를 외래 키 항목을 통하여 연결한다.