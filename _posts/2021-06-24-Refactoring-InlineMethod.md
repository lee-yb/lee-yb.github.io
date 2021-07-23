# [Refactoring] Inline Method (메서드 내용 직접 삽입)

메서드 기능이 너무 단순해서 메서드명만 봐도 너무 뻔할 땐 _**그 메서드의 기능을 호출하는 메서드에 넣어버리고 그 메서드는 삭제하자**_



```java
int getRating(){
  	return (moreThanFiveLateDeliveries()) ? 2: 1;
}
boolean moreThanFiveLateDeliveries(){
  	return _numberOfLateDeliveries > 5;
}
```

<center>▼</center>

```java
int getRating(){
  	return (_numverOfLateDeliveries > 5) ? 2 : 1;
}
```



**방법**

- 메서드가 재정의되어 있지 않은지 확인하자
- 각 메서드를 호출하는 부분을 모두 찾자
- 각 호출 부분을 메서드 내용으로 교체하자
- 테스트를 실시하자
- 메서드 정의를 삭제하자