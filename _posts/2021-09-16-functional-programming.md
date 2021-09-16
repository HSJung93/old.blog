---
title: "함수형 프로그래밍이란"
date: 2021-09-16T10:30:00+09:00
categories:
  - 코드
tag:
  - 함수형 프로그래밍
  - 자바 스크립트
header:
  teaser: /assets/images/code.jpg
toc: true
toc_sticky: true
---
이 포스팅은 2016년 JSUnconf에서의 Anjana Vakil의 강연 내용에 기반하였습니다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/e-5obm1G_FY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 함수형 프로그래밍과 명령형 프로그래밍
함수형 프로그래밍은 프로그래밍 패러다임, 코딩 스타일, 사고방식의 하나이다. 객체형 프로그래밍에 비하여 디버그와 유지 관리가 더 쉽다. 

함수형 프로그래밍은 모든 프로그램을 함수로 표현한다. 함수는 인풋을 받아서 아웃풋으로 내보낸다. 다음은 함수형이 아닌 방식으로 "안녕하세요, 저는 회성입니다."를 띄우는 방법이다.

```javascript
var name = "회성입니다.";
var greeting = "안녕하세요, 저는 ";
console.long(greeting+name);
```

변수를 지정해서 name이라고 하고, 콘솔에 기록한다. 이는 명령형 프로그래밍 방식이다. 코드간의 순서가 존재한다. 함수형 프로그래밍 방식으로는 같은 일을 다음과 같이 한다. 

```javascript
function greet(name){
    return "안녕하세요, 저는 " + name;
}

greet("회성입니다.");
```

매개 변수(파라매터) name을 취하는 greet 함수를 만들어서, "안녕하세요, 저는" 이라는 스트링을 붙여서 반환한다. 

## 순수 함수
함수형 프로그래밍은 주어진 인풋에서 아웃풋을 계산해서 반환하지 않는 부작용이 일어나는 피해서 순수 함수만 사용한다. 반환하지 않고 프린트하는 함수나, 인풋이 아니라 전역 변수에 의존하는 함수는 대표적인 부작용의 사례이다. 인풋만으로 아웃풋을 도출한다. 순수하지 않은 경우는 예컨대 다음과 같다.

```javascript
var name = "회성입니다.";
function greet(){
    console.log("안녕하세요, 저는 " + name);
}
```

## 고차 함수
고차 함수는 함수를 인풋으로 받고 함수를 아웃풋으로 반환하는 함수이다. 함수를 일종의 객체로 생각해서, 함수 내에 함수가 있는 구조를 만들 수 있다. 다른 패러다임에서 사용하는 몇가지 요령을 안쓰기 위해서는 고차 함수가 필요하다. 

```javascript
function makeAdjectifier(adjective){
    return function (string) {
        return adjective + " " + string;
    };
}

var coolifier = makeAdjectifier("cool");
coolifier("conference");
```

## 반복문을 피한다.
함수형 프로그래밍에서는 for나 while문을 피하고 map이나 reduce, filter과 같은 고차함수를 사용한다. 이런 고차함수는 인풋으로 원하는 방식의 리스트만이 아니라 함수도 받는다. 자바스크립트는 리스트의 프로토타입에 map, reduce 등의 빌트인 함수를 제공한다. map과 reduce를 샌드위치에 비유한다면 다음과 같이 작동한다.

![map-reduce-sandwich](/assets/images/map-reduce-sandwich.png)

## 변형성을 피한다.
데이터를 변형성을 피하여 이뮤터블 데이터를 사용한다. 변형이란 객체 위치 변경을 사용한다. 이뮤터블 데이버는 위치를 변경할 수 없는 데이터다. 예컨대 다음의 코드는 잘못된, 비함수형 변형을 하고 있다.

```javascript
var rooms = ["H1", "H2", "H3"];
rooms[2] = "H4";
rooms;
```
원래 있던 것을 바꾸어서 여전히 변수 rooms에 저장하고 있다. 이렇게 객체 지향적으로 접근하면 의도치 않게 뭔가를 바꿀 수 있다. 헷갈릴수가 있다. rooms를 바꾸었지만 코드의 다른 부분에서 rooms가 바뀐것을 몰랐다면 버그를 찾기 어려울 수 있다. 이곳의 rooms는 맞는데 뒤에 rooms는 틀리는 상황이 발생한다. 더 나은 방법은 모든 데이터를 불변 데이터로 생각하는 것이다.

```javascript
var rooms = ["H1", "H2", "H3"];
Var newRooms = rooms.map(function (rm) {
    if (rm == "H3") { return "H4"; }
    else { return rm; }
});
newRooms; 
rooms;
```
위의 코드에서는 map 함수를 사용해서 새로운 newRooms 변수를 만들었다. rooms는 불변 데이터로 취급해서 변하지 않았고 그대로다. 

## 영속(Persistent) 데이터 구조
불변성의 문제는 배열 같은 것이 불변한다고 했을 때 계속 사본이 생긴다는 점이다. 리스트에서 하나의 요소를 바꾸고 싶어도 배열 전체를 새로 만들어야 한다. 객체가 크고 복잡해지면 효율이 떨어지게 된다. 이와 같은 문제를 해결하기 위한 함수형 프로그래밍의 전략이 영속 데이터 구조이다. Phil Bagwell이 이상적인 해쉬트리라는 논문의 이론으로 시작했고, Clojure 언어를 개발한 Rich Hickey가 Bagwell의 아이디어로 데이터 구조를 도입했다. 

기존 구조를 재사용하는 구조 공유을 통하여, 기존 버전과 새 버전을 부분적으로 공유해 벡터에서 뭔가를 추가, 변경, 이동하는 작업을 효율적으로 한다. 예컨대 리스트의 각 요소를 트리의 리프노드로 두고, 변경할 요소의 노드를 제거한 뒤 새로운 요소의 노드를 연결한다. 그러면 기존 구조를 재사용하여 이뮤터블 데이터를 업데이트하는데에 시간과 공간을 아낄 수 있다. 자바스크립트 라이브러리 중에서는, 데이터 구조를 네이티비를 가지고 있는 ClojureScript를 사용해서 자바스크립트에 포팅하는 Mori나 facebook에서 발표한 Immutable.js가 유명하다.