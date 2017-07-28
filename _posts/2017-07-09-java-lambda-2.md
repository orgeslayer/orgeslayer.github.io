---
layout: post
title: 람다식 - (2)
author: orgeslayer
update:  2017-07-009T11:15:00Z
published: true
complete: true
tags: java1.8, lambda, FunctionalInterface, 함수적프로그래밍
excerpt: 자바에서 제공되는 표준 API에서 한 개의 추상 메소드를 가지는 인터페이스들은 모두 람다식을 이용해서 익명 구현 객체로 표현이 가능하다.  ...
---
# 람다식 (Lambda Expression) 

#### 목차
1. [람다식 기본](https://orgeslayer.github.io/2017/07/09/java-lambda-1/)
2. [Java 표준API](https://orgeslayer.github.io/2017/07/09/java-lambda-2/)
3. [메소드참조](https://orgeslayer.github.io/2017/07/09/java-lambda-3/)

------------------------
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
|BinaryOperator&lt;T&gt;|BiFunction&lt;T, U, R&gt;의 하위 인터페이스|두 개의 Obj를 연산하여 T 리턴|
|UnaryOperator&lt;T&gt;|Function&lt;T, R&gt;의 하위 인터페이스|하나의 Obj를 연산하여 T 리턴|
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
### andThen() 과 compose() 디폴트 메소드

java.util.function 패키지의 함수적 인터페이스는 하나 이상의 디폴트 메소드를 가지고 있다.
Consumer, Function, Operator 종류의 함수적 인터페이스는 andThen()과 compose() 디폴트 메소드를 가지고 있다. andThen() 과 compose() 메소드는 두 개의 함수적 인터페이스를 순차적으로 연결하고, 첫 번째 처리 결과를 두 번째 매개값으로 제공해서 최종 결과값을 얻을 때 사용합니다. 두 디폴트 메소드의 차이는 어떤 함수적 인터페이스부터 먼저 처리되느냐에 따라 다르다.

```java
Interface interfaceAB = interfaceA.andThen(interfaceB);
Result result = interfaceAB.method();
```

interfaceAB 의 method()를 호출하면 우선 **interfaceA부터 처리하고 결과를 interfaceB의 매개값으로 사용된다.** interfaceB는 제공받은 매개값을 가지고 처리한 후 최종 결과를 리턴한다.

```java
Interface interfaceAB = interfaceA.compose(interfaceB);
Result result = interfaceAB.method();
```

interfaceAB 의 method()를 호출하면 우선 **interfaceB부터 처리하고 결과를 interfaceA에 매개값으로 사용된다.** interfaceA는 제공받은 매개값을 가지고 처리한 후 최종 결과를 리턴한다.

아래는 andThen(), compose() 디폴트 메소드를 제공하는 java.util.function 패키지의 함수적 인터페이스 목록이다.

**Consumer** 

|함수적 인터페이스|andThen()|compose()|
|--------------|:-------:|:-------:|
|Consumer&lt;T&gt; | O |  |
|BiConsumer&lt;T, U&gt; | O |  |
|DoubleConsumer | O |  |
|IntConsumer | O |  |
|LongConsumer | O |  |

**Function** 

|함수적 인터페이스|andThen()|compose()|
|--------------|:-------:|:-------:|
|Function&lt;T, R&gt; | O |  |
|BiFunction&lt;T, U, R&gt; | O | O |

**Operator** 

|함수적 인터페이스|andThen()|compose()|
|--------------|:-------:|:-------:|
|BinaryOperator&lt;T&gt; | O |  |
|DoubleUnaryOperator | O | O |
|IntUnaryOperator | O | O |
|LongUnaryOperator | O | O |


------------------------------
### and(), or(), negate() 디폴트 메소드와 isEqual() 정적 메소드

**Predicate** 종류의 함수적 인터페이스는 and(), or(), negate() 디폴트 메소드를 가지고 있다. 
이 메소드들은 각각 논리연산자인 &&, ||, ! 과 대응된다. 
**&&, || 연산의 경우 조건을 모두 체크하지 않기도 하는데 이는 and(), or() 디폴트 메소드에서도 동일하게 적용**된다.  
아래 예제는 2의 배수, 3의 배수를 조사하는 Predicate와 두 Predicate를 논리연산한 새로운 Predicate 를 생성한다.

``` java
IntPredicate predicateA = (a) -> {
	System.out.println("[predicateA 호출됨]");
	return a % 2 == 0;
};
IntPredicate predicateB = (b) -> {
	System.out.println("[predicateB 호출됨]");
	return b % 3 == 0;
};

boolean result;
IntPredicate predicateAB = predicateA.and(predicateB);
result = predicateAB.test(9);
System.out.println("9는 2와 3의 배수입니까? " + result);

predicateAB = predicateA.or(predicateB);
result = predicateAB.test(9);
System.out.println("9는 2또는 3의 배수입니까? " + result);

predicateAB = predicateA.negate();
result = predicateAB.test(9);
System.out.println("9는 홀수입니까? " + result);
```

Predicate&lt;T&gt; 함수적 인터페이스는 isEqual() 정적 메소드를 별도로 제공한다. isEqual() 메소드는 test() 매개값인 sourceObject 와 isEqual() 의 매개값인 targetObject 를 java.util.Objects 클래스의 equals() 의 매개값으로 제공하고, Objects, equals(source, targetObject) 의 리턴값을 얻어 새로운 Predicate&lt;T&gt; 를 생성한다. 

```java
Predicate<Object> predicate = Predicate.isEqual(targetObject);
boolean result = predicate.test(sourceObject);
```

---------------------------------
### minBy(), maxBy() 정적 메소드
BinaryOperator&lt;T&gt; 함수적 인터페이스는 minBy(), maxBy() 정적 메소드를 제공한다. 두 메소드는 **매개값으로 제공되는 Comparator 를 이용해서 최대 T와 최소 T를 얻는 BinaryOperator&lt;T&gt;를 리턴**한다.

Comparator&lt;T&gt;는 o1과 o2를 비교하여 o1작으면 음수를, o1과 o2가 동일하면 0, o1이 크면 양수를 리턴하는 compare() 메소드가 선언된 함수적 인터페이스다.
```java
@FunctionalInterface
public interface Comparator<T> {
    public int compare(T o1, T o2);
}
```

-------------------------------
참조
[palpit's log-b](http://palpit.tistory.com/670)
