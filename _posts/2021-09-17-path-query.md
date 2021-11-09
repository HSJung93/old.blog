---
title: "Path Variable 과 Query Parameter"
date: 2021-09-16T18:53:00+09:00
categories:
  - 메모
tag:
  - GET
  - Path Variable
  - Query Parameter
header:
  teaser: /assets/images/slipBox.jpg
toc: true
toc_sticky: true
---

## Path Variable
* 경로를 변수로 사용한다.
* url에 /로 구분된다.
* resource를 식별할 때 보다 적합하다. 
* 존재하지 않는 id가 들어왔을 때, 서버로 데이터를 넘겨 에러 핸들링을 하는 것보다, 페이지가 없다는 404 에러를 발생시키는 것이 낫다. 

## Query Parameter
* 경로 뒤에 데이터를 함께 제공한다.
* ? 이후에 key와 value 쌍으로 이루어진 query string이 주어진다. 
* 필터링이나 정렬을 해야할 때에 보다 적합하다.
* 관련된 글이 존재하지 않는 경우, 404 에러를 발생시키는 것보다는, 빈 리스트를 반환하여 존재하지 않음을 나타내는 것이 더 낫다. 