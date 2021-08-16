---
title: "Enum class란"
date: 2021-08-12T22:ㄹㄹ:00+09:00
categories:
  - 코드
tags:
  - enum
  - class
  - java
header:
  teaser: /assets/images/code.jpg
---

Enum은 열거형으로 불리며, 서로 연관된 상수들의 집합을 의미합니다. 주로 인터페이스나 클래스로 상수를 정의하는 것을 보완합니다. 

기존에는 다음과 같이 클래스 내부에  final static을 이용하여 상수를 정의했었지만, 이와 같은 방식은 정적 변수의 상수들이 너무 많아질 경우 코드의 가독성이 떨어지는 문제가 있습니다.

```java
public class Discount{
    public static final int GOLD_MEMBER = 1;
    public static final int SILVER_MEMBER = 2;

    public static void main(String[] args) {
        int grade = GOLD_MEMBER;
        switch(grade){
            case GOLD_MEMBER;
            System.out.println(30+"% 할인");
            break;
            case SILVER_MEMBER;
            System.out.println(10+"% 할인");
            break;
        }
    }
}
```

반면 enum을 이용하여 코드를 다시 쓰면, 인스턴스 생성과 상속을 방지하여 상수값 타입에 대하여 안전하며 switch문을 사용하지 않아도 됩니다. enum 클래스를 구현하면 유일하게 하나의 인스턴스가 생성되어 사용됩니다.

```java
enum Member{
    GOLD_MEMBER(30), SILVER_MEMBER(10);
    int discount;
    Member(int discount){
        this.discount = discount;
    }
    public String getDiscount(){
        return discount + "% 할인";
    }
}

public class Discount{

    public static void main(String[] args) {
        Member mb = Member.GOLD_MEMBER;
        System.out.println(mb);
        String str = mb.getDiscount();
        System.out.println(str);
    }
}
```

덧붙여 `values()`와 `valueOf()`, `ordinal()`과 같은 메소드를 사용할 수 있습니다.