# [Refactoring] Extract Method(메소드 추출)

어떤 코드를 그룹으로 묶어도 되겠다고 판단될 땐 _**그 코드를 뺀내어 목적을 잘 나타내는 직관적 이름의 메서드로 만들자.**_



```java
void printOwing(double amount){
  printBanner();
  
  // 세부 정보 출력
  System.out.println("name: " + _name);
  System.out.println("amount" + amount);
}
```

<center>▼</center>

```java
void printOwing(double amount){
  printBanner();
  printDetails(amount);
}

void printDetails(double amount){
  System.out.println("name: " + _name);
  System.out.println("amount" + amount);
}
```



**방법**

- 목적에 부합하는 새 메서드를 생성하자

  > 더 이해하기 쉬운 이름으로 추출하지 않을 바에는 차라리 코드를 추출하지 말자

- 기존 메서드에서 빼낸 코드를 새로 생성한 메서드로 복사하자

- 빼낸 코드에서 기존 메서드의 모든 지역변수 참조를 찾자. 그것들을 새로 생성한 메서드의 지역변수나 매개변수로 사용할 것이다.

- 추출한 코드에 의해 변경되는 지역변수가 있는지 파악하자. 만약 하나의 지역변수만 변경된다면 추출 코드를 메서드 호출처럼 취급ㅎ

- 빼낸 코드에서 읽어들인 지역변수를 대상 메서드에 매개변수로 전달한다.

- 모든 지역변수 처리를 완료했으면 컴파일을 실시하자

- 원본 메서드 안에 있는 빼낸 코드 부분을 새로 생성한 메서드 호출로 수정

- 컴파일과 테스트 실시



**예제**

```java
void printOwing(){
  	Enumeration e = _orders.elements();
  	double outstanding = 0.0;
  
  	// 배너 출력
  	System.out.println("*****************");
  	System.out.println("*****고객 외상*****");
    System.out.println("*****************");
  
  	// 외상액 계산
  	while (e.hasMoreElements()){
      	Order each = (Order) e.nextElement();
      	outstanding += each.getAmount();
    }
  
  	// 세부 내역 출력
  	System.out.println("고객명: " + _name);
  	System.out.println("외상액: " + outstanding);
}
```



**결과**

```java
void printOwing(double previousAmount){
  	printBanner();
  	double outstanding = getOutstanding(previousAmount * 1.2);
  	printDetails(outstanding);
}

double getOutstanding(double initialValue){
  	double result = initialValue;
  	Enumeration e = _orders.elements();
  	while (e.hasMoreElements()){
      	Order each = (Order) e.nextElement();
      	result += each.getAmount();
    }
  	return result;
}

void printBanner(){
  	// 배너 출력
  	System.out.println("*****************");
  	System.out.println("*****고객 외상*****");
    System.out.println("*****************");
}

void printDetails(double outstanding){
  	// 세부 내역 출력
  	System.out.println("고객명: " + _name);
  	System.out.println("외상액: " + outstanding);
}
```


