# List Interface



[**List**](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html)

> An ordered collection (also known as a *sequence*). The user of this interface has precise control over where in the list each element is inserted. The user can access elements by their integer index (position in the list), and search for elements in the list.

sequence 로 불리는 이 정렬된 컬렉션은 index라는 위치 정수값으로 요소들을 조회하고 삽입할수 있다. 즉 순서가 있는 데이터들을 목록으로 이용할수 있도록 만들어진 인터페이스이며, **중복을 허용**하면서 **저장순서가 유지**되는 컬렉션을 구현하는데 사용된다.

데이터 목록으로 이용할수 있는 Array 와 비교해보자면 배열의 경우 초기에 정해진 크기 외에는 사용할수가 없다. 이러한 단점을 보완하여 List 인터페이스를 통해 구현된 클래스들은 _**동적크기**_ 를 갖으며 배열처럼 사용할수가 있다. 

Collection 인터페이스를 상속받았고, List 인터페이스를 구현하는 클래스들로는 ArrayList, LinkedList, Vector, Stack 등이 있다.



**List 인터페이스에 정의된 메서드 [ JDK 11 ]** (상속받은것 제외)

| Type                   | Method and Desc                                              |
| ---------------------- | ------------------------------------------------------------ |
| boolean                | **addAll(int index, Collection<? extends E> c)**<br>지정된 컬렉션(c)의 모든 요소를 지정된 위치(index)에 추가한다. |
| default void           | **replaceAll(UnaryOperator<E> operator)**<br>컬렉션의 각요소를 연산자(operator)를 적용한 결과로 바꾼다. |
| default void           | **sort(Comparator<? super E> c)**<br>지정된 Comparator 에 따라 컬렉션을 정렬한다. |
| E                      | **get(int index)**<br>지정된 위치(index)에 있는 요소를 반환한다. |
| E                      | **set(int index, E element)**<br>지정된 위치(index)에 있는 요소를 지정된 요소(element)로 바꾼다. |
| void                   | **add(int index, E element)**<br>지정된 위치(index)에 요소(element)를 삽입한다. |
| E                      | **remove(int index)**<br>지정된 위치(index)에 있는 요소를 제거한다. |
| int                    | **indexOf(Object o)**<br>지정된 요소(o)가 처음 발생한 위치를 반환한다. 요소가 포함되어 있지 않으면 -1을 반환한다. |
| int                    | **lastIndexOf(Object o)**<br>지정된 요소(o)가 마지막으로 발생한 위치를 반환한다. 요소가 포함되어 있지 않으면 -1을 반환한다. |
| ListIterator<E>        | **listIterator()**<br>컬렉션 요소들에 대한 iterator를 반환한다. |
| ListIterator<E>        | **listIterator(int index)**<br>컬렉션의 지정된 위치(index)에서 시작하여 컬렉션 요소들에 대한 iterator를 반환한다. |
| List<E>                | **subList(int fromIndex, int toIndex)**<br>지정된 구간 (fromIndex이상, toIndex 미만) 사이의 list를 반환한다. |
| default Spliterator<E> | **spliterator()**<br>요소들에 대한 Spliterator를 생성한다.   |
| static <E> List<E>     | **of()** <br>요소가 없는 수정 불가능한 list를 반환한다.      |
| static <E> List<E>     | **of(E e1)**<br>하나의 요소를 포함하는 수정 불가능한 list를 반환한다. |
| •••                    | •••                                                          |
| static <E> List<E>     | **of(E e1, E e2, E e3, E e4, E e5, E e6, E e7, E e8, E e9, E e10)**<br>열개의 요소를 포함하는 수정 불가능한 list를 반환한다. |
| static <E> List<E>     | **of(E... elements)**<br>임의의 개수 요소를 포함하는 수정 불가능한 list를 반환한다. |
| static <E> List<E>     | **copyOf(Collection<? extends E> coll)**<br>요소(coll)를 포함하는 수정 불가능한 list를 반환한다. |



**Array(배열) 와의 차이점**

위에서 얘기하였듯이 배열은 초기에 정해진 크기를 변경할수가 없고 (정적할당 _static allocation_), 메모리에 연속적으로 나열되어서 할당된다. 또 크기가 변경되지 않기 때문에 데이터를 삭제하더라도 해당 index에는 빈공간으로 남아있게된다.

리스트는 반대로 리스트의 길이가 가변적 (동적할당 _dynamic allocation_) 이고, 메모리에 연속적으로 나열되어있지 않고 주소로 연결되어있다. 그리고 데이터 사이에 빈 공간을 허용하지 않고 **연속적**이다.



**리스트의 장단점**

장점으로는

1. 데이터의 개수에 따라 메모리를 할당해주기 때문에 메모리 관리가 편하다.
2. 데이터 사이의 빈공간을 허용하지 않기 때문에 데이터 관리에도 편하다.
3. 주소(포인터)로 데이터들이 연결되어 있기 때문에 연결된 주소만 바꿔주면 되기 때문에 삽입,삭제에 용이하다.

단점으로는

1. 객체로 데이터를 다루기 때문에 적은양의 데이터만 쓸 경우 배열에 비해 차지하는 메모리가 커진다.

   간단히 예로들어 primitive type인 Int는 4Byte를 차지한다. 반면에 Wraaper class인 Integer는 32bit JVM에선 객체의 헤더(8Byte), 원시 필드(4Byte), 패딩(4Byte)으로 '최소 16Byte를 차지한다. 거기에다가 이러한 객체데이터들을 다시 주소로 연결하기 때문에 16 + α 가 된다.

2. 기본적으로 주소를 기반으로 구성되어있고 메모리에 순차적으로 할당하는 것이 아니기 때문에(물리적 주소가 순차적이지 않다) 색인(검색)능력은 떨어진다.
