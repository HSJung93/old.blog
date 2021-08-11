---
title: "빌더 패턴(Builder Pattern)이란 무엇이며 왜 쓰는가?"
date: 2021-08-11T18:38:00+09:00
categories:
  - code
tags:
  - java
  - builder pattern
  - getter setter
header:
  teaser: /assets/images/code.jpg
toc: true
toc_sticky: true
---

자바를 통한 백엔드 서버 구현에 익숙해지는 한창입니다. 빌더 패턴(Builder Pattern)에 대하여 제대로 알고 싶은 충동과 필요를 느끼게 되었습니다. 이제는 서버 개발할 때 lombok의 어노테이션 한번으로 많은 것이 해결되어 버리긴 하지만, 흐리슬쩍 넘어가기에는 너무나 신경이 쓰입니다.

## 점층적 생성자 패턴
빌더 패턴은 객체 생성을 위한 기법입니다. 이해를 돕기 위해 먼저 "점층적 생성자 패턴"을 살펴봅시다. 다음의 코드는 `Griffindor` 클래스를 점층적 생성자 패턴으로 만든 예시입니다. 

```java
public class Griffindor{
    private final String name;
    private final String sex;
    private final String hobby;

    public Griffindor(String name) {
        this(name, "성별 비공개", "취미 비공개");
    }

    public Griffindor(String name, String sex) {
        this(name, sex, "취미 비공개");
    }

    public Griffindor(String name, String sex, String hobby) {
        this.name = name;
        this.sex = sex
        this.hobby = hobby;
    }
}

Griffindor harry = new Griffindor("해리", "남성", "퀴디치");
Griffindor hermione = new Griffindor("헤르미온느", "여성");
Griffindor ron = new Griffindor("론");
```
점층적 생성자 패턴은 필드의 비공개 값이 많은 경우 생성자의 인자를 받지 않고도 생성자를 호출할 수 있다는 장점이 있습니다. 하지만 인자가 추가되는 일이 발생하면 코드를 수정하기 어렵고, 호출 코드만 봐서는 의미를 알기 어려운 일이 생깁니다. `harry` 객체의 "퀴디치"가 무엇을 의미하는지 알기 어려운 것처럼 말이죠.

## 자바 빈 패턴
반면 `setter` 메소드를 이용하는 자바 빈 패턴에 따라 클래스를 정의하면, 클래스를 생성하는 코드가 보다 읽기 좋습니다. 

```java
public class Griffindor{
    private String name = 1;
    private String sex = 0;
    private String hobby = 0;

    public Griffindor() {}

    public void setName(String name) {
        this.name = name;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public void setHobby(String hobby) {
        this.hobby = hobby;
    }
}

Griffindor harry = new Griffindor();
harry.setName("해리");
harry.setSex("남성");
harry.setHobby("퀴디치");

Griffindor hermione = new Griffindor();
hermione.setName("헤르미온느");
hermione.setSex("여성");

Griffindor ron = new Griffindor();
ron.setName("론");
```
이전보다 생성 코드에서 인자 각각의 의미를 알기가 쉽습니다. 또한 각각의 생성자를 만들 필요 없이 `public Griffindor() {}` 만으로도 `Griffindor` 클래스를 만들었습니다. 그러나 객체 생성을 위하여 여러번의 호출이 필요하고 변경 불가능(immutable) 클래스를 만들 수가 없는 동시에, 스레드 안전성을 확보하는 것이 또다른 문제가 됩니다.

## 빌더 패턴
이제 빌더 패턴을 통하여 `Griffindor` 클래스를 만들어 봅시다.

```java
public class Griffindor{
    private final String name;
    private final String sex;
    private final String hobby;

    public static class Builder {

        private final String name;

        private String sex = 0;
        private String hobby = 0;

        public Builder(String name) {
            this.name = name;
        }

        public Builder sex(String val) {
            sex = val;
            return this;
        }

        public Builder hobby(String val) {
            hobby = val;
            return this;
        }

        public Griffindor build() {
        return new Griffindor(this);
        }
    }

    private Griffindor(Builder builder){
        name = builder.name;
        sex = builder.sex;
        hobby = builder.hobby;
    }


}

Griffindor.Builder builder = new Griffindor.Builder("해리");
builder.sex("남성");
builder.hobby("퀴디치");
Griffindor harry = builder.build();
// 혹은 다음과 같은 방식으로 객체를 생성할 수 있습니다.
Griffindor hermione = new Griffindor.Builder("헤르미온느").sex("여성").build();
Griffindor ron = new Griffindor.Builder("론").build();
```
이처럼 빌더 패턴을 통해 클래스를 생성하면 `setter` 메소드없이 변경 불가능 객체(immutable)를 만들 수 있습니다. 각 인자의 의미를 알기 쉽고, 한 번의 호출로 객체를 생성하기도 합니다. 또다른 장점으로는 `build()` 메소드로 잘못된 입력 값을 검증할 수도 있습니다.

빌더 패턴으로 클래스에서 생성을 담당하는 부분을 표현을 담당하는 부분과 명확히 구분한 결과, 동일한 생성 과정으로도 복잡하고 다양한 객체들을 표현할 수 있게 되었습니다.

이 포스팅은 [기계인간 John Grib][builder-pattern1]님과 [ifContinue][builder-pattern2]님과 [크레온][builder-pattern3]님의 포스팅에 많은 도움을 받았습니다.

[builder-pattern1]: https://johngrib.github.io/wiki/builder-pattern/
[builder-pattern2]: https://ifcontinue.tistory.com/7
[builder-pattern3]: https://creon.tistory.com/333