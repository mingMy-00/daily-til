# Java에 존재하는 4가지 변수

종류 : `클래스 변수`, `인스턴스 변수`, `지역변수`, `매개변수`

⇒ 이 4가지 변수를 구분짓는 방법은 `선언위치` 와 `예약어`

# 1. 클래스 변수

선언 위치 : 클래스 블록 내부, 메서드 외부

키워드 : `static` 

생명주기 : 클래스가 처음 로딩될 때 Method Area에 올라가고 JVM 종료 시까지 유지

```jsx
public class Main {
    static String name = "김버디몽총이"; // 클래스 변수

    public static void main(String[] args) {
        System.out.println(Main.name);
    }
}
```

다음과 같은 코드가 있으면 

1. JVM이 Main.class를 로딩하면서 name 변수를 Method Area에 하나만 생성함. 
2. 모든 객체가 이 저장소를 공유함 (Method Area는 모든 스레드가 공유하는 공간이기 때문) 
3. 따라서 Main.class를 바꾸면 모든 인스턴스에서 같은 값이 보임. 

# 2. 인스턴스 변수

- **선언 위치**: 클래스 블록 내부, 메서드 외부 (클래스 변수와 같음)
- **키워드**: 없음
- **생명 주기**: 객체가 `new`로 생성될 때 **Heap 영역**에 만들어지고, 참조가 사라지면 GC 대상이 됨

```jsx
public class VariableTest {
	static int staticValue = 10; //클래스 변수 선언
	int instanceValue = 20; //인스턴스 변수 선언
	
	public static void main(String[] args) {
		System.out.println(staticValue);
		//System.out.println(instanceValue);
		//클래스 메소드에서 인스턴스 변수를 호출 불가능
	}
	
	public void display() {
		System.out.println(staticValue);
		System.out.println(instanceValue);
	}
}

class Ex {
	VariableTest test = new VariableTest();
	
	public void print() {
		System.out.println(VariableTest.staticValue);
		System.out.println(test.instanceValue);
	}

}
```

⇒ 위에서 보듯이 인스턴스 변수는 `static`이 없이 선언되어 있음. 주목해야 하는곳은 `main`메서드에서 `인스턴스 변수`를 사용할 수 없다는 점.

<aside>
💡

이유는 `main`메서드가 `static`이 붙은 `클래스 메서드`이기 때문임.

</aside>

클래서 메서드와 인스턴스 메서드는 변수사용에서 차이를 가짐.

- `클래스 메서드`에서는 인스턴스 메서드, 인스턴스 변수를 `사용할 수 없다.`
- `인스턴스 메서드`는 클래스 메서드, 클래스 변수를 `사용할 수 있다.`

<aside>
💡

이러한 규칙이 있는 이유는 `인스턴스 멤버`가 존재하는 시점에서 `클래스 멤버`는 항상 존재하지만, `클래스 멤버`가 존재하는 시점에서 `인스턴스 멤버`가 항상 존재한다는 보장이 없기 때문.

인스턴스 멤버는 반드시 객체를 생성한 후에 참조 또는 호출이 가능하나,  클래스 멤버는 객체를 생성하지 않고 참조 연산자를 통해 접근이 가능하기 때문 

</aside>

# 3. 지역변수

- **선언 위치**: 메서드 안, `{ }` 블록 안
- **생명 주기**: 메서드가 호출되면 **Stack Frame** 안에 생성되고, 메서드가 끝나면 Stack Frame과 함께 사라짐

```jsx
public class Main {
    public static void main(String[] args) {
       if("김버디".equals("멍총총총이") {
          int num = 23;
       }
       
       if("김민제".equals("똑똑이") {
          int num = 26;
       }
    }
}
```

`지역변수`들은 메서드가 호출될 때 `호출 스택(call stack)`영역 생성되었다가 메서드의 작업이 끝나면 반환됨.

따라서 코드 상에 지금 num이라고 하는 같은 이름의 변수명이 생성됐지만, if 문 안의 지역변수 이고 해당 if문이 종료될 시 소멸되기 때문에 저렇게 선언이 가능한 것이다.

# 매개변수

- **선언 위치**: 메서드 괄호 `()` 안
- **생명 주기**: 메서드가 호출되면 **Stack Frame 안에서 지역 변수처럼 생성**되고, 호출 종료 시 사라짐

```jsx
public class Calculator {
    public int add(int a, int b) { // a, b는 매개변수
        int sum = a + b;          // sum은 지역 변수
        return sum;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        int result = calc.add(3, 5); // 인자로 전달
        System.out.println(result);  // 8
    }
}
```

메소드에 넘겨주는 변수

파라미터 라고 부르기도함. 

# 변수 사용 기준을 생각해보자.

그럼 클래스 변수랑 인스턴스 변수는 언제 사용할까  ??

1️⃣ 클래스 변수는 모든 객체들이 동일한 값을 가져야 하는 속성일때 

⇒ `클래스 변수는 하나의 저장공간을 공유하기 때문에` 누군가 클래스 변수의 값을 바꾸면 모든 객체들이 가진 클래스 변수값이 변하게됨. 

2️⃣ 인스턴스 변수는 모든 객체들이 고유의 값을 가져야 하는 속성일때 

⇒ new 라는 키워드를 통해 생성되어 , `각각의 독립된 저장공간을 가지게 되기 때문`이다.