---
layout: post
title: Lambda (정리중)
update:  2017-06-05T21:00:00Z
published: true
complete: false 
tags: lambda, java1.8, FunctionalInterface, 함수적프로그래밍
excerpt: 자바 8부터 함수적 프로그래밍을 위하여 람다식 (Lambda Expressions) 지원하기 시작하면서, 기본의 코드 형태가 많이 달라졌다. 람다식은 ...
---
# 람다식 (Lambda Expression) 

## Intro
자바 8부터 함수적 프로그래밍을 위하여 람다식 (Lambda Expressions) 지원하기 시작하면서, 기본의 코드 형태가 많이 달라졌다. 람다식은 익명함수 (anonymous function)를 생성하기 위한 식으로 객체지향 언어보다는 **함수 지향 언어**에 가깝다. 객체지향 프로그래밍에 익숙한 개발자들은 혼란스러울 수 있지만, 다음과 같은 장점이 있다.

> - 자바 코드가 매우 간결해짐 
> - 컬렉션의 요소를 탐색해서 결과를 쉽게 얻어낼 수 있음

람다식은 아래와 같은 형태로 작성된다.

> (매개변수) -> {실행코드}

매개 변수를 가진 코드 블록이지만, 런타임 시에는 익명 구현 객체를 생성한다.
예를들어 **Runnable 인터페이스의 익명 구현 객체를 생성하는 코드**는 아래와 같이 차이가 난다.

**기존 자바코드**
```java
Runnable runnable = new Runnable() {
	    @Override
	    public void run() {
			...
		}
	}
```

**람다식으로 작성한 코드**
```java
Runnable runnable = () -> { ... } 
```

-----------------------------
## 람다식 기본 문법

람다식은 아래와 같은 형태로 작성된다.

> (타입 매개변수, ...) -> {실행문; ...}

**타입 매개변수** 부분은 실제 인터페이스에 선언된 매개 변수 타입과 해당 타입에 맞는 객체를 의미하고, 코드 작성 시에 매개 변수 이름은 자유롭게 지정이 가능하다. 매개변수의 경우 이미 인터페이스에 선언이 되어있기 때문에 *(= 런타임 시에 대입되는 값에 따라 자동으로 인식됨)* 별도 타입을 명시하지 않아도 된다. 또, 하나의 매개 변수만 있다면 괄호 ( )를 생략할 수 있다.
예를들어 int 매개 변수 a 의 값을 콘솔에 출력하는 람다식은 아래 모두 동일한 표현이다.

``` java
(int a) -> { System.out.println(a); }
(a) -> { System.out.println(a); }		// a type is int 
a -> { System.out.println(a); }			// a param is only one
```

**->** 기호는 매개 변수를 이용하여 우측 { } 블록을 실행하라고 지시하는 것으로 생각하면 된다.

**실행문**의 경우 실제 수행해야 할 코드를 입력하는 영역이다. 만약 실행문이 하나일 경우 중괄호 {}를 생략할 수 있다. 예를들어 위의 int 매개 변수 a의 값을 콘솔에 출력하는 람다식은 실행문이 하나이므로 아래 모두 동일한 표현이다.

```java
a -> { System.out.println(a); }
a -> System.out.println(a);
```
실행 결과를 리턴해야 할 경우 마찬가지로 하나의 실행문으로 표현이 가능하다면 { } 생략이 가능하고, **return 구문을 생략**이 가능하다. 예를들ㅇ int형 매개 변수 a, b 로부터 두 값의 합을 리턴해야 할 경우, 아래 모두 동일한 표현이다.
```java
(a, b) -> { return a+b; }
(a, b) -> a+b;
```

--------------------------
## 타겟 타입과 함수적 인터페이스

람다식의 형태는 매개 변수를 가진 코드 블록이기 때문에 마치 **자바의 메소드를 선언**하는 것처럼 보인다.
자바는 메소드를 단독으로 선언할 수 없고, 항상 클래스의 구성 멤버로 선언하기 때문에 람다식은 메소드를 선언하는것이 아니라 이 메소드를 구현하고 있는 객체를 생성해 낸다.

> 인터페이스 변수 = 람다식;

람다식은 인터페이스 변수에 대입이 된다. 즉, **람다식은 인터페이스의 구현 객체를 생성한다는 의미**다.
인터페이스는 직접 객체화 될 수 없기 때문에 구현체가 필요한데, 람다식은 익명으로 구현체를 생성하고 객체화 시킨다. 여기서 대입될 인터페이의 종류에 따라 작성 방법이 달라지므로, 람다식이 대입될 인터페이스를 람다식의 타겟 타입이라고 한다.

예를들어 아래와 같은 인터페이스가 있다면, 이를 구현한 람다식 구현체는 아래와 같다.
```java
public interface Calculator {
	public void calc(int a, int b);
}

...
Calculator add = (a, b) -> return a+b;
Calculator diffAbs = (a, b) -> return Math.abs(a-b);
```
<br>
모든 인터페이스를 람다식의 타겟 타입으로 사용할 수 없다. **람다식은 하나의 메소드를 정의**하기 때문에, 두 개 이상의 추상메소드가 선언된 인터페이스는 람다식을 이용하여 구현체를 생성할 수 없다. 즉, ***하나의 메소드가 선언된 인터페이스만이 람다식의 타겟 타입이 될 수 있는데, 이러한 인터페이스를 함수적 인터페이스라(Functional Interface)고 한다***.
함수적 인터페이스를 작성할 때 두 개 이상의 메소드가 선언되지 않도록 컴파일러에서 체크해주는 기능이 있으며, 인터페이스 선언 시 **@FunctionalInterface** 어노테이션을 사용하면 된다. 아래와 같은 경우 컴파일에러가 발생한다.

```java
@FunctionalInterface
public interface Calculator {
	public int calc(int a, int b);
	public int calc2(int a);
}

- Error : Invalid '@FunctionalInterface' annotation; Calculator is not a functional interface
```

**@FunctionalInterface** 어노테이션이 없더라도 하나의 추상 메소드만 있다면 모두 함수적 인터페이스다. 하지만 명시적으로 함수적 인터페이스라고 표시하거나, 실수로  두 개 이상 메소드를 선언하는 것을 방지하고 싶을 경우 붙여주는 것이 좋다.

------------------------------
## 클래스 멤버와 로컬 변수 사용

람다식 실행 블록에는 클래스의 멤버 필드와 메소드를 사용할 수 있다. 다만, **this 키워드를 사용할 때는 주의가 필요하다. 일반적으로 익명 객체 내부에서 this 는 익명 객체를 참조하지만, 람다식에서 this는 람다식을 실행한 객체의 참조**다.

Calculator 인터페이스를 이용하여 아래와 같이 확인이 가능하다.

```java
public class Outer {
	public int outerField = 100;
	
	class Inner {
		int innerField = 200;
		
		void func() {
			Calculator calc = (a, b) -> {
				System.out.println("OuterField : " + outerField);
				System.out.println("OuterField : " + Outer.this.outerField);
				
				System.out.println("InnerField : " + innerField);
				System.out.println("InnerField : " + this.innerField);
				return a+b;
			};
			
			calc.calc(10, 20);
		}
	}
}
```

```java
public static void main(String[] ar) {
	Outer outer = new Outer();
	Outer.Inner inner = outer.new Inner();
	inner.func();
}
```

로컬 변수의 경우 바깥 클래스의 필드나 메소드는 제한없이 사용 가능하지만, **메소드의 매개변수 또는 로컬 변수를 사용하면 이 변수들은 final 특성을 가지게 된다.** 즉, 매개 변수 또는 로컬 변수를 람다식에서 읽는 것은 허용되지만, 이럴 경우 람다식 내부 및 외부에서 값을 변경할 수 없다.

```java
int local = 30;
Calculator calc = (a, b) -> {
	return local+a+b;
};
	
System.out.println("main calc - " + calc.calc(0,0));	// 30
local = 40;

- Error : Local variable local defined in an enclosing scope must be final or effectively final
```

--------------------------------------------
## 표준 API의 함수적 인터페이스
자바에서 제공되는 표준 API에서 한 개의 추상 메소드를 가지는 인터페이스들은 모두 람다식을 이용해서 익명 구현 객체로 표현이 가능하다. 그리고 객체 생성자로 함수적 인터페이스를 람다식 형태로 전달이 가능하다.

```java
Thread thread = new Thread( () -> System.out.println("Hello, Lambda") );
thread.start();
```

자바 8부터는 **빈번하게 사용되는 함수적 인터페이스를 java.util.function 표준 API 패키지로 제공한다. 이 패키지에서 제공**하는 함수적 인터페이스의 목적은 메소드 또는 생성자의 매개 타입으로 사용되어 람다식을 대입할 수 있게 하기 위함이다. 
java.util.function 패키지의 함수적 인터페이스는 크게 ***Consumer, Supplier, Function, Operator, Predicate*** 로 구분된다.

### Consumer 함수적 인터페이스
**Consumer 함수적 인터페이스의 특징은 리턴값이 없는 accept() 메소드를 제공**한다. accept() 메소드는 단지 매개값을 소비하는 역할만 한다. 매개 변수의 타입, 수에 따라 아래와 같은 Consumer 들이 있다.

|인터페이스 명|추상 메소드|설명|
|-----------|---------|---|
|Consumer&lt;T&gt;|void accept(T t)|객체 T를 받아 소비|
|BiConsumer&lt;T, U&gt;|void accept(T t, U u)|객체 T와 U를 받아 소비|
|IntConsumer|void accept(int value)|int 값을 받아 소비|
|DoubleConsumer|void accept(double value)|double 값을 받아 소비|
|LongConsumer|void accept(long value)|long 값을 받아 소비|
|ObjIntConsumer&lt;T&gt;|void accept(T t, int value)|객체 T와 int 값을 받아 소비|
|ObjDoubleConsumer&lt;T&gt;|void accept(T t, double value)|객체 T와 double 값을 받아 소비|
|ObjLongConsumer&lt;T&gt;|void accept(T t, long value)|객체 T와 long 값을 받아 소비|

Consumer 사용 예제는 아래와 같다.
```java
Consumer<String> consumer = (t) -> System.out.println(t + "를 소모합니다.");
consumer.accept("에너지");
consumer.accept("가스");
```

### Supplier 함수적 인터페이스
**Supplier 함수적 인터페이스의 특징은 매개 변수가 없고 리턴값이 있는 getXXX() 메소드를 제공**한다. 이 메소드들은 실행 후 호출한 곳으로 데이터를 공급(return)하는 역할을 한다. 리턴타입에 따라 아래와 같은 Supplier 함수적 인터페이스가 있다.

|인터페이스 명|추상 메소드|설명|
|-----------|---------|---|
|Supplier&lt;T&gt;|T get()|T 객체를 리턴|
|BooleanSupplier|boolean getAsBoolean()|boolean 값을 리턴|
|IntSupplier|int getAsInt()|int 값을 리턴|
|LongSupplier|long getAsLong()|long 값을 리턴|
|DoubleSupplier|double getAsDouble|double 값을 리턴|

다음 예제는 주사위 숫자를 랜덤하게 공급하는 IntSupplier 인터페이스를 타겟 타입으로 하는 람다식입니다.

```java
IntSupplier intSupplier = () -> {
	int num = (int) (Math.random() * 6) + 1;
	return num;
};

int num = intSupplier.getAsInt();
System.out.println("주사위의 수 : " + num);
```

### Function 함수적 인터페이스 
**Function 함수적 인터페이스의 특징은 매개변수를 가지고 리턴값을 매핑(형변환) 해주는 applyXXX() 메소드를 제공**한다. 매가 변수 타입과 리턴 타입에 따라서 아래와 같은 Function 함수적 인터페이스가 있다.

|인터페이스 명|추상 메소드|설명|
|-----------|---------|---|
|Function&lt;T, R&gt;|R apply(T t)|객체 T를 받아 객체 R로 매핑|
|BiFunction&lt;T, U, R&gt;|R apply(T t, U u)|객체 T와 U를 받아 객체 R로 매핑|
|IntFunction&lt;R&gt;|R apply(int value)|int 값을 받아 R로 매핑|
|DoubleFunction&lt;R&gt;|R apply(double value)|double 값을 받아 R로 매핑|
|IntToDoubleFunction|double applyAsDouble(int value)|int 값을 받아 double로 매핑|
|IntToLongFunction|long applyAsLong(int value)|int 값을 받아 long으로 매핑|
|LongToDoubleFunction|double applyAsDouble(long value)|long 값을 받아 double로 매핑|
|LongToIntFunction|int applyAsInt(long value)|long 값을 받아 int로 매핑|
|ToIntBiFunction&lt;T, U&gt;|int applyAsInt(T t, U u)|객체 T와 U를 받아 int로 매핑|
|ToIntFunction&lt;T&gt;|int applyAsInt(T t)|객체 T를 받아 int로 매핑|
|ToDoubleBiFunction&lt;T, U&gt;|double applyAsDouble(T t, U u)|객체 T와 U를 받아 double로 매핑|
|ToDoubleFunction&lt;T&gt;|double applyAsDouble(T t)|객체 T를 받아 double로 매핑|
|ToLongBiFunction&lt;T, U&gt;|long applyAsLong(T t, U u)|객체 T와 U를 받아 long으로 매핑|
|ToLongFunction&lt;T&gt;|long applyAsLong(T t)|객체 T를 받아 long으로 매핑|


다음 예제는 숫자를 입력하여 문자열 포멧으로 출력하는 Function&lt;Integer, String&gt; 인터페이스를 타겟 타입으로 하는 람다식입니다.

```java
Function<Integer, String> formatter = (value) -> String.format("%,d", value);
System.out.println("1234567 > " + formatter.apply(1234567));
```

### Operator 함수적 인터페이스
**Operator 함수적 인터페이스는 Function과 동일하게 매개 변수와 리턴값이 있는 applyXXX() 메소드를 제공한다. 하지만, 이 메소드들은 매개값을 리턴값으로 매핑(형변환)하는 역할보다 매개값을 이용해서 연산을 수행한 후 동일한 타입으로 리턴값을 제공하는 역할**을 한다. 매개 변수의 타입과 수에 따라서 아래와 같은 Operator 함수적 인터페이스가 있다.

|인터페이스 명|추상 메소드|설명|
|-----------|---------|---|
|UnaryOperator&lt;T&gt;|Function&lt;T, R&gt;의 하위 인터페이스|T를 연산 후 R 리턴|
|BinaryOperator&lt;T&gt;|BiFunction&lt;T, U, R&gt;의 하위 인터페이스|T와 U를 연산 후 R 리턴|
|IntBinaryOperator|int applyAsInt(int, int)|두개의 int 를 연산|
|IntUnaryOperator|int applyAsInt(int)|하나의 int 를 연산|
|LongBinaryOperator|int applyAsLong(long, long)|두개의 long을 연산|
|LongUnaryOperator|int applyAsLong(long)|하나의 long을 연산|
|DoubleBinaryOperator|double applyAsDouble(double, double)|두개의 double를 연산|
|DoubleUnaryOperator|double applyAsDouble(double)|하나의 double를 연산|

두개의 값을 가지고 더 큰 값, 더 작은 값을 리턴하는 IntBinaryOperator 인터페이스를 타겟으로 하는 람다식 예제입니다.

```java
int[] scores = {99, 82, 94, 77, 63, 58, 97, 86, 66, 70};
IntBinaryOperator maxOperator = (a, b) -> a >= b ? a : b;
IntBinaryOperator minOperator = (a, b) -> a <= b ? a : b;

int min = scores[0];
int max = scores[0];
for(int score : scores) {
	min = maxOperator.applyAsInt(min, score);
	max = minOperator.applyAsInt(max, score);
}
```

### Predicate 함수적 인터페이스
**Predicate함수적 인터페이스는 매개 변수와 boolean 을 리턴하는 testXXX() 메소드를 제공**한다. 이 메소드들은 매개값을 조사해서 true 또는 false 를 리턴하는 역할을 한다. 매개 변수 타입과 수에 따라 아래와 같은 Predicate 함수적 인터페이스들이 있다.

|인터페이스 명|추상 메소드|설명|
|-----------|---------|---|
|Predicate&lt;T&gt;|boolean test(T t)|객체 T를 조사|
|BiPredicate&lt;T, U&gt;|boolean test(T t, U u)|객체 T와 U를 조사|
|IntPredicate|boolean test(int)|int 값을 조사|
|LongPredicate|boolean test(long)|long 값을 조사|
|DoublePredicate|boolean test(double)|double 값을 조사|

입력된 값 중 90 이상인 수의 개수를 구하는 IntPredicate 인터페이스를 타겟으로 하는 람다식 예제입니다.

```java
int[] scores = {99, 82, 94, 77, 63, 58, 97, 86, 66, 70};
IntPredicate a = (value) -> value >= 90;
		
int count = 0;
for(int score : scores) {
	if( a.test(score) ) count++;
}
System.out.println("90점 이상 : " + count);
```

------------------------------------
