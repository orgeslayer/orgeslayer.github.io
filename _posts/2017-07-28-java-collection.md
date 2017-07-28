---
layout: post
author: orgeslayer
title: Collection Framework
update:  2017-07-028T21:00:00Z
published: true
complete: true
tags: ArrayList, LinkedList, TreeSet, HashMap, TreeMap
excerpt: Java에서는 자료구조를 쉽게 사용할 수 있도록 Collection Framework 를 제공한다. 대표적인 내용들을 대략적으로 정리해본다.
---
# Collection Framework

<br>
![Collection Framework](/images/collection_framework_abstract_structure.gif)

- 유기적인 관계가있는 객체들을 그룹화시키고 정형화시켜 구체화된 다수의 데이터를 쉽게 처리할 수 있는 방법을 제공하는 클래스들과 재사용할 수 있는 컬렉션 데이터 구조들을 구현하는 인터페이스들의 집합
- 데이터의 집합을 표현하기 위한 다양한 클래스를 제공하는 프레임워크 
- List, Set, Map 3가지 타입이 존재한다고 인식하고 각 타입을 인터페이스로 정의
- List, Set 의 경우 공통된 부분을 다시 뽑아서 Collection 이라는 인터페이스를 구현

-----------------
## List
**순서가 있는 데이터의 집합이며, 중복을 허용한다.**

### Vector
JDK 1.0 버전에서 추가되었으며, **Array(배열) 구조를 통해 자료를 저장, 배열의 복사를 이용하여 내부적으로 데이터를 저장 처리**한다. **Synchronized 가 보장**된다.

### ArrayList
JDK 1.2 버전에서 추가되었으며, 기본적인 동작 원리는 Vector 와 같으나 **Syncrhonized 가 보장되지 않는다.**

장점
> 많은 데이터를 한번에 가져와서 여러번 참조 (ex. loop) 할 경우 용이함 <br>
> 특정 index 에 해당되는 데이터에 접근할 경우 직접 접근 가능

단점
> 데이터를 추가/삭제 시 많은 비용이 발생함

### LinkedList
JDK 1.2 버전에서 추가되었으며, **자신의 자료의 다음 위치에 해당되는 자료 위치 정보만 가지고 있다.** 주로 사용자가 Queue, Stack 등 추상자료형(ADT)을 만들어서 사용하는데 많이 사용된다.

장점
> 데이터의 추가/삭제 시 적은 비용이 발생함

단점
> 데이터 전체를 검색할 경우 처음 자료에서부터 순차적인 검색을 하기 때문에 많은 비용 발생

----------
## Set
**순서가 없는 데이터의 집합, 중복을 허용하지 않는다.** 이를 위하여 Set 에 의하여 관리되는 객체는 equals() 메소드가 구현되어야 한다. 

### HashSet
Set 인터페이스의 가장 대표적인 클래스이며, JDK 1.2 버전에서 추가되었다. Synchronized 가 보장되지 않으며 Object 의 hashcode() 를 이용하여 자료를 관리한다.

### TreeSet
JDK 1.2 버전에서 추가되었으며, 이진탐색트리 자료구조 형태로 데이터를 저장, 즉 저장되는 시점에 정렬된 위치에 저장된다. 따라서, **HashSet 보다는 삽입/삭제 등에서 성능은 떨어지나, 정렬 및 검색에서는 용이하다.**

----------------------------

## Map
키(key) 와 값(value)의 쌍으로 이루어진 데이터의 집합, **키의 중복은 허용하지 않고, 값의 중복은 허용**한다.

### HashTable 
JDK 1.0 버전에서 추가되었으며, 기본적인 동작은 HashMap 과 동일하지만 **HashTable 은 synchronized 가 보장된다.** 

### HashMap
JDK 1.2 버전에서 추가되었으며, 키에 대한 해시값을 사용하여 값을 저장하고 조회한다. 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 Map (associate array). HashTable 에 비해 보조 해시함수를 사용하기 때문에 해시 충돌이 덜 발생하며, 성능상 이점이 있다. KeySet 을 이용하여 전체 데이터를 조회한다.

### TreeMap
JDK 1.2 버전에서 추가되었으며, HashMap 에서 **키값을 TreeSet 으로 정렬된 상태로 관리, 따라서 KeySet 을 요청하면 정렬된 KeySet 을 얻어올 수 있다.** 초기 생성 시 Comparator 을 전달하여 데이터 저장 시 키 리스트를 TreeSet 으로 저장하며, HashMap 보다 성능은 떨어지지만 범위 검색 및 정렬이 필요할 경우 TreeMap 을 쓰는것이 더 효율적이다.

----------

### 동기화 지원
동기화를 처리해야 할 경우 JDK 1.0 에서 제공되었던 클래스들 (Vector, HashTable 등)을 사용하지 말고, JDK 1.2 에서 추가된 클래스(ArrayList, HashMap 등)를 사용하는 것이 낫다. JDK 업데이트마다 하위 호환성을 유지한 채로 개선된 방향으로 해당 클래스들의 업데이트가 지원되기 때문이다. 
만약 해당 클래스들이 **동기화가 지원되어야 할 경우에는 Collections 의 synchronizedList, synchronizedSet, synchronizedMap 등의 함수를 이용**하는 것이 낫다.



-----------
참조
- [Creating data structures with the Java Collections Framework](http://www.techrepublic.com/article/creating-data-structures-with-the-java-collections-framework/)
- [Java HashMap 은 어떻게 동작하는가?](http://helloworld.naver.com/helloworld/textyle/831311)