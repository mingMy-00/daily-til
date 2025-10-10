# Java에서 Pass by Value vs Pass by Reference, 제대로 이해하기

프로그래밍하다 보면 `Java는 값으로 넘기는 거야? 참조로 넘기는 거야?` 라는 질문을 한 번쯤은 들어봤을 것.

물론 저도 제대로 알고 있지 않았었기 때문에 

정리를 해봤습니다. 

## 1. Pass by Value (값에 의한 호출)

메서드에 변수를 넘길 때, **변수 값의 복사본**을 전달하는 방식입니다.

즉, **원본 데이터는 절대 바뀌지 않습니다.**

주로 기본형 타입(primitive)에서 사용되죠.

```java
public class ValueExample {
    public static void change(int num) {
        num = 100; // 지역 변수 num만 바뀜
    }

    public static void main(String[] args) {
        int x = 5;
        change(x);
        System.out.println(x); // 출력: 5 (원본 불변)
    }
}
```

➡️ 여기서 `x`와 `num`은 서로 **완전히 독립적인 공간입니다.**

`num`이 바뀌어도 `x`는 그대로죠.

![스크린샷 2025-09-13 오후 6.20.22.png](/Java/img/PassBy/passBy%20(2).png)

Java는 지역변수를 모두 Stack영역에 할당합니다.

따라서 

```jsx
 int x = 5;
```

여기서 스택 프레임에 x : 5라는 값이 생기게 됩니다. 

![스크린샷 2025-09-13 오후 6.22.04.png](/Java/img/PassBy/passBy%20(4).png)

change method를 호출 하면, 값이 복사되어 num이라는 새로운 스택 프레임에 값 5만 복사하여 생성이 됩니다. 

![스크린샷 2025-09-13 오후 6.21.26.png](/Java/img/PassBy/passBy%20(3).png)

change method 내에서 num의 값만 변경이 되고 x에는 아무런 영향을 주지 않습니다.

change method가 끝나면 stack을 다시 pop 하여 정리를 합니다.

즉, 마지막에 

```jsx
 System.out.println(x); // 출력: 5 (원본 불변)
```

할 때는 

![스크린샷 2025-09-13 오후 6.20.22.png](/Java/img/PassBy/passBy%20(2).png)

---

다음과 같이 정리가 됩니다.

객체일 때는 어떤가요 객체에서는 **객체 참조값(reference value)을 복사해서 전달**합니다.

### 예제 2: 객체 타입

```java
import java.util.*;

public class ObjectExample {
    public static void change(List<String> list) {
        list.add("B"); // 참조된 객체 수정
        list = new ArrayList<>(); // 새로운 객체 할당
        list.add("C");
    }

    public static void main(String[] args) {
        List<String> myList = new ArrayList<>();
        myList.add("A");

        change(myList);
        System.out.println(myList); // [A, B]
    }
}
```

다음과 같은 코드가 있다면

![스크린샷 2025-09-13 오후 6.32.52.png](/Java/img/PassBy/passBy%20(5).png)

우선,  myList 객체가 생성이 되고 Stack 영역에는 주소값만 갖고 있습니다.

그리고 myList.add(’A’)를 하면 참조하고 있는 Heap 영역의 객체에 값이 할당됩니다.

change method 호출 뒤 

![스크린샷 2025-09-13 오후 6.36.13.png](/Java/img/PassBy/passBy%20(6).png)

change method에서는 주소 값만 복사해서 넘기고, 같은 주소값이기 때문에 같은 객체를 참조 합니다.

![스크린샷 2025-09-13 오후 6.36.50.png](/Java/img/PassBy/passBy%20(7).png)

따라서 add(’B’) 하면 같은 주소를 참조하기 때문에 값이 변합니다.

그러나, 

```jsx
list = new ArrayList<>(); // 새로운 객체 할당
list.add("C");
```

![스크린샷 2025-09-13 오후 6.39.53.png](/Java/img/PassBy/passBy%20(8).png)

다음과 같은 경우, 새로운 객체 자체를 할당하기 때문에 list가 참조하고 있는 객체의 주소값이 변경이됩니다.

이 상태로 add(’C’) 하면 

참조하는 Heap영역 데이터가 바뀌었기 때문에

![스크린샷 2025-09-13 오후 6.40.36.png](/Java/img/PassBy/passBy%20(1).png)

새로운 주소값을 가진 객체에 C 라는 값이 추가가 됩니다.

따라서 

```jsx
 System.out.println(myList); // [A, B]
```

이랬을 때 0x1242 주소값을 가진 객체의 값이 출력되어 A,B만 출력이 되는 겁니다.

따라서, Java는 Primitive 타입이나, Object 타입이나 모두 Pass by value입니다.

## 2. Pass by Reference (참조에 의한 호출)

참조 전달은 **변수의 주소(참조값)를 전달**하는 방식입니다.

- 메서드 안에서 값을 바꾸면 **원본도 바로 바뀝니다.**
- C++, C# 같은 언어에서 지원합니다.

```cpp
#include <iostream>
using namespace std;

void change(int &num) { // 참조 전달
    num = 100;
}

int main() {
    int x = 5;
    change(x);
    cout << x; // 출력: 100 (원본 변경)
}
```

`&num`은 x의 별명(alias)이라서, 수정하면 원본이 그대로 바뀌는 거죠.

## 3. 그럼 Java는 왜 Pass by Value를 채택했을까 ??

### C/C++의 경우

C나 C++은 자유도가 엄청 높아요.

- 값을 그대로 전달할 수도 있고,
- 변수의 주소를 전달할 수도 있으며,
- 포인터를 직접 넘겨서 조작할 수도 있죠.

즉, 원하는 거의 모든 방식으로 인자를 넘길 수 있어요.

하지만 자유도가 높다는 건 **단점도 많다**는 의미를 가집니다.

- 호출 방식이 다양해서 코드 읽기가 어렵고,
- 메모리 관리 실수로 메모리 침범 같은 문제가 발생하기도 합니다.
    
    그래서 C/C++로 작성된 프로그램은 성능은 좋지만, 디버깅 난이도가 높고 안전하지 않은 경우가 많아요.
    

### Java의 선택

Java는 그런 C언어의 단점을 커버하여 등장했고, 단순하고 안전하게라는 목표를 가지고 설계됐습니다.

- 포인터 연산을 없애서 메모리 문제와 보안 문제를 최소화했고,
- 호출 방식도 무조건 **Pass by Value**로 통일했죠.
- 게다가 Garbage Collector와 자동 메모리 관리와도 잘 맞습니다.

결과적으로 Java에서는 메서드에 변수를 넘기더라도, **동작을 예측하기 쉽고 안전한 코드**를 작성할 수 있어요.

즉, 복잡하게 포인터를 직접 다룰 필요가 없어서 초보자도 비교적 안전하게 코드를 작성할 수 있죠.

### C#과 비교

C#은 Java와 비슷하지만, 필요할 때는 `ref`나 `out` 키워드를 사용해서 **Pass by Reference**도 지원해요.

즉, 성능 최적화나 원본 수정이 필요하면 선택적으로 참조를 넘길 수 있죠.

Java는 이런 선택권을 제공하지 않고, 

**한 가지 방식(Pass by Value)만 지원**해 단순성과 안전성을 더 중시합니다.

## 6. 결론

| 구분 | 전달 방식 | 원본 변경 |
| --- | --- | --- |
| Pass by Value | 값 복사 | 불변 |
| Pass by Reference | 참조 전달 | 변경 가능 |

Java는 **항상 Pass by Value**

- Primitive 타입: 값 자체를 복사
- 객체 타입: 참조값을 복사 → 객체 내부는 변경 가능, 참조 자체는 변경 불가
- 장점: 단순, 안전, 예측 가능

**정리하면**

> Java는 단순성과 안전성을 위해 모든 인자를 Pass by Value로 처리합니다.
> 
> 
> 객체는 참조값을 값으로 복사하기 때문에 reference처럼 보여도 실제로는 value입니다.
>