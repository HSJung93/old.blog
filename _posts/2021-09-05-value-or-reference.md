---
title: "Value냐 Reference냐 그것이 문제로다"
date: 2021-09-05T13:04:00+09:00
categories:
  - 코드
tag:
  - Call by value
  - Call by reference
  - copy
header:
  teaser: /assets/images/code.jpg
---

python에서 string은 list와 유사한 방법으로 다룰 수 있지만, list와 달리 item assignment는 지원하지 않는다. immutable이기 때문이다. 파이썬의 자료형은 mutable이거나 immutable이다.

```python
l = [1, 2, 3]
l[0] = 7
print("가능")

s = "123"
s[0] = "7"
print("불가능")
```

mutable인 리스트는 새 변수에 값을 할당한 뒤 새 변수의 값을 바꾸면 원래 변수도의 값도 바뀐다. mutable한 객체는 메모리 주소값(Reference)을 할당하기 때문이다.

immutable인 문자열은 새 변수에 값을 할당할 때까지만 메모리 주소값을 할당하다가, 새 변수의 값을 바꾸면 재할당이 이루어져서 메모리 주소값도 변경된다. immutable이기 때문이다.

```python
l1 = [1, 2, 3]
l2 = l1
id(l1)
id(l2)

print("다른 값 할당")
l2[0] = 7
print(l1)
print(l2)


s1 = "123"
s2 = s1
id(s1)
id(s2))

print("다른 값 할당")
s2 = "723"
print(s1)
print(s2)
```

따라서 mutable인 리스트의 경우 원래 변수의 값만 복사하고, 원래 변수를 바꾸고 싶지 않으면 슬라이싱을 통하여 새로운 값을 할당해야 한다.

특이한 점은 슬라이싱을 통하여 전체 리스트에 새로운 메모리 주소를 할당하면서도, 원래 리스트 안의 값들은 기존 메모리 주소를 그대로 사용한다는 것이다. 이를 shallow copy라 하며 copy 모듈의 copy 메소드 또한 shallow copy다.

```python
ll1 = [[1, 2], [3, 4]]
ll2 = ll1[:]

import copy 
ll3 = copy.copy(ll1)
ll4 = copy.deepcopy(ll1)

id(ll1)
id(ll2)
id(ll3)
id(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])
```

중첩 리스트의 경우 안의 리스트도 mutalble이므로 값을 바꾸고 싶을 때 조심해야 한다. 안의 리스트의 값을 재할당하는 경우는 메모리 주소도 변경되어 문제가 되지 않지만, append() 등의 메소드로 값을 변경하면 shallow copy된 변수들의 값도 같이 변화한다!

코딩 테스트를 보다보면 이중 리스트를 만들 때 *를 사용하지 않고 for 문을 이용하여 리스트 컴프리헨션을 수행하는데 이 shallow copy 때문이다. 

```python
ll1[0] = [5, 6]

print(ll1)
print(ll2)
print(ll3)
print(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])

ll1[0].append(7)

print(ll1)
print(ll2)
print(ll3)
print(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])

ll2[0].append(8)

print(ll1)
print(ll2)
print(ll3)
print(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])

ll3[0].append(9)

print(ll1)
print(ll2)
print(ll3)
print(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])
```
ll1, ll2, ll3는 값이 같이 변화하는 반면, ll4는 독립적임을 확인할 수 있다. copy 모듈의 deepcopy로 내부의 값까지 새롭게 메모리 주소를 할당할 수 있기 때문이다. 

```python
ll4[0].append(10)

id(ll1)
id(ll2)
id(ll3)
id(ll4)

id(ll1[0])
id(ll2[0])
id(ll3[0])
id(ll4[0])
```

자바에서 인스턴스를 복사 할 때도 shallow copy가 일어납니다. 인스턴스를 저장하는 stack에서 heap의 같은 메모리 주소를 참조기 때문입니다. deep copy를 하고 싶을 때에는 여러 방법이 있지만, 새롭게 객체를 생성하여 값을 가져오는 방법이 직관적입니다. 

```java

public static void main(String[] args) {

    public class Programmer {
            
        String name;
        Integer year;

        public Programmer(String name, Integer year) {
            this.name = name;
            this.money = money;
        }

        public void getName() {
            return this.name;
        }

        public void getYear() {
            return this.year;
        }

        public void setName(String name){
            this.name = name;
        }
        
        public void setYear(Integer year){
            this.name = year;
        }

        public void updateYear(Integer year){
            this.year += year;
        }
    }

    public static void shallowCopy() {

        Programmer page = new Programmer("Page", 10);
        Programmer zuckerberg = page;

        zuckerberg.setName("Zuckerberg");
        zuckerberg.updateYear(1);

        System.out.println(page);
        System.out.println(zuckerberg);

    }

    public static void deepCopy() {
        
        Programmer torvald = new Programmer();
        torvald.setName("Torvald");
        torvald.setYear(page.getYear());

        torvald.updateYear(20);

        System.out.println(page);
        System.out.println(torvald);

    }

}
```
