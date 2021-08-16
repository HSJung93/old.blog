---
title: "자바 메모리 관리와 가비지 컬렉션"
date: 2021-08-10T14:40:00+09:00
categories:
  - 메모
tags:
  - JVM
  - JGC
header:
  teaser: /assets/images/code.jpg
---
자바 메모리 관리에 대하여 정리한 포스팅입니다. [yaboong][main]님의 기념비적인 포스팅에서 많은 도움을 받았습니다. 

## 자바 메모리의 스택 영역에서는
* 힙 영역에 생성된 객체 타입 자료의 참조값이 할당된다. 
* 원시 타입의 자료가 실제 값(참조 값이 아니라)과 함께 할당된다.
  * 원시 타입: byte, short, int, long, double, float, boolean, char
* 지역 변수들을 스코프에 따른 가시성을 가진다.
  * 전역 변수가 아닌 지역 변수가 함수 내에서 스택에 할당된 경우, 다른 함수에서 지역 변수에 접근할 수 없다. 함수의 중괄호가 `}` 실행되면 내부에서 선언한 모든 지역 변수들은 스택에서 pop되어 사라진다. 
* 각 스레드는 자신만의 스택을 가진다. 

## 자바 메모리의 힙 영역에서는
* 모든 객체 타입은 힙 영역에 생성된다. 
  * 대부분의 객체는 크기가 크고, 서로 다른 코드 블럭에서 공유되는 경우가 많다.
  * 객체 타입: Integer, String, ArrayList 등
* 몇 개의 스레드가 존재하든 단 하나의 힙 영역만 존재한다. 
* 힙 영역 안의 객체 타입의 자료는 함수 내부에서 변경되더라도 함수 호출이 종료된 시점에 변경 내역이 반영된다. 
  * 단 불변(immutable) 객체의 경우에는, 어떤 연산을 수행할 때마다 기존 오브젝트를 변경하는 것이 아니라 새로운 오브젝트를 생성하고 힙 영역에 새롭게 할당한다. 이 과정에 GC가 개입한다. 
    * 불변 객체: Integer, Character, Byte, Boolean, Long, Double, Float, Short 등 Wrapper class에 해당하는 객체들

## 자바 가비지 콜렉터는
* 프로그래머는 힙을 사용할 수 있는 만큼 자유롭게 사용하고, 더이상 사용되지 않는 오브젝트들은 가비지 컬렉션을 담당하는 프로세스가 자동으로 메모리에서 제거하도록 한다. 
* Unreachable 객체를 우선적으로 메모리에서 제거하여 메모리 공간을 확보한다. 
  * Unreachable 객체: 스택에서 도달할 수 없는 힙 영역의 객체
* 마크와 스윕 과정으로 가비지 콜렉팅을 한다. 
  * 마크: 스택의 모든 변수를 스캔하면서 각각 어떤 오브젝트를 참조하고 있는지 찾는다.
    * Stop the world: 마크 작업을 위하여 모든 스레드를 중단하는 것
  * 스윕: 마크되어 있지 않은 모든 객체들을 힙에서 제거한다. 
* `System.gc()`를 호출하여 명시적으로 가비지 콜렉션이 일어나도록 코드를 삽입할 수는 있지만 모든 스레드가 중단되기 때문에 코드단에서 호출하지 말아야 한다. 
* 자바 8부터 클래스들은 모두 힙이 아닌 네이티브 메모리를 사용하는 Metaspace에 할당된다. 

## 참고 링크
* [자바 메모리 관리 - 스택과 힙][yaboong-blog-memory-management]
* [자바 메모리 관리 - 가비지 컬렉션][yaboong-blog-garbage-collection]


[main]: https://yaboong.github.io/
[yaboong-blog-memory-management]: https://yaboong.github.io/java/2018/05/26/java-memory-management/
[yaboong-blog-garbage-collection]: https://yaboong.github.io/java/2018/06/09/java-garbage-collection/