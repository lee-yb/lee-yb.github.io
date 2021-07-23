# [Refactoring] Split Temporary Variable (임시 변수 분리)

루프 변수나 값 누적용 임시변수가 아닌 임시변수에 여러 번 값이 대입될 땐 _**각 대입마다 다른 임시변수를 사용하자.**_



```java
double temp = 2 * (_height + _width);
System.out.println(temp);
temp = _height * _width;
System.out.println(temp);
```

<center>▼</center>

```java
final double perimeter = 2 * (_height + _width);
System.out.println(perimeter);
final double area = _height * _width;
System.out.println(area);
```



**방법**

- 선언문과 첫 번째 대입문에 있는 임시변수 이름을 변경하자.
- 이름을 바꾼 새 임시변수를 final로 선언하자
- 그 임시변수의 모든 참조 부분을 두 번째 대입문으로 수정하자
- 두번째 대입문에 있는 임시변수를 선언하자
- 컴파일과 테스트를 실시하자
- 각 대입문마다 차례로 선언문에서 임시변수 이름을 변경하고, 그 다음 대입문까지 참조를 수정하며 위의 과정을 반복하자