# Collection Interface



[**Collection**](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collection.html)

> The root interface in the *collection hierarchy*. A collection represents a group of objects, known as its *elements*. Some collections allow duplicate elements and others do not. Some are ordered and others unordered. The JDK does not provide any *direct* implementations of this interface: it provides implementations of more specific subinterfaces like `Set` and `List`. This interface is typically used to pass collections around and manipulate them where maximum generality is desired.

공식문서에서 Collection 인터페이스의 설명이다. 컬렉션은 객체들의 그룹이며, 컬렉션들은 객체들의 중복을 허용할수도, 허용하지 않을수도 있다. **세부적인 구현은 SubInterface에서 다룬다.**



**Collection 인터페이스에 정의된 메서드 [ JDK 11 ]**

| Type                   | Method and Desc                                              |
| ---------------------- | ------------------------------------------------------------ |
| int                    | **size()**<br>모든 요소의 개수를 반환한다.                   |
| boolean                | **isEmpty()**<br>이컬렉션에 요소가 없는 경우 true를 반환한다. |
| boolean                | **contains(Object o)**<br>이 컬렉션에 지정된 요소(o)가 포함된 경우 true를 반환한다. |
| Iterator<E>            | **iterator()**<br>이 컬렉션에 대한 Iterator를 반환한다.      |
| Object[]               | **toArray()**<br>이 컬렉션의 모든 요소들을 포함하는 배열을 반환한다. |
| <T> T[]                | **toArray(T[] a)**<br>이 컬렉션에 모든 요소들을 포함하는 배열(a)을 반환한다. 반환된 배열의 런타임 타입은 지정된 타입(T) 이다. |
| boolean                | **add(E e)** <br>컬렉션에 요소(e)가 포함되어있는지 확인한다. |
| boolean                | **remove(Object o)**<br>지정된 단일인스턴스(o)가 있다면 제거한다. |
| boolean                | **containsAll(Collection<?> c)**<br>지정된컬렉션(c)의 모든 요소가 포함된경우 true를 반환한다. |
| boolean                | **addAll(Collection<? extends E> c)** <br>지정된 컬렉션(c)의 모든 요소를 이 컬렉션에 추가한다. |
| boolean                | **removeAll(Collection<?> c)**<br>지정된 컬렉션(c)에 포함된 모든 요소들을 제거한다. |
| default boolean        | **removeIf(Predicate<? super E> filter)**<br>주어진 Predicate를 만족하는 요소들을 제거한다. |
| boolean                | **retainAll(Collection<?> c)**<br>지정된 컬렉션(c)에 포함된 요소들만 유지한다. |
| void                   | **clear()**<br>컬렉션의 모든요소를 지운다.                   |
| boolean                | **equals(Object o)**<br>지정된 인스턴스(o)와 컬렉션의 동등성을 비교한다. |
| int                    | **hashCode()**<br>컬렉션의 해시코드 값을 반환한다.           |
| default Spliterator<E> | **spliterator()**<br>컬렉션의 요소들에 대해 Spliterator를 만든다. |
| default Stream<E>      | **stream()**<br>이 컬렉션을 소스로하는 순차 Stream을 반환한다. |
| default Stream<E>      | **parallelStream()**<br>이 컬렉션을 소스로하는 병렬 가능한 Stream을 반환한다. |



[**Iterable**](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Iterable.html)

> Implementing this interface allows an object to be the target of the enhanced `for` statement (sometimes called the "for-each loop" statement).

이 인터페이스를 구현하면 향상된 for문을 사용할수 있다.

Collection<E> 인터페이스는 Iterable<E> 인터페이스를 상속받고 있다. 즉 Collection을 구현하는 클래스들은 forEach 메서드를 사용할수 있다.



_**상속관계도**_

![상속관계도](/assets/images/collection.png)
