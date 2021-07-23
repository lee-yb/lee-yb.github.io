# [Refactoring] Replace Temp with Query (임시변수를 메서드 호출로 전환)

수식의 결과를 저장하는 임시변수가 있을 땐 _**그 수식을 빼내어 메서드로 만든 후, 임시변수 참조 부분을 전부 수식으로 교체하자. 새로 만든 메서드는 다른 메서드에서도 호출 가능하다.**_



```java
double basePrice = _quantity * _itemPrice;
if (basePrice > 1000)
  return basePrice * 0.95;
else
  return basePrice * 0.98;
```

<center>▼</center>

```java
if (basePrice() > 1000)
  return basePrice() * 0.95;
else
  return basePrice() * 0.98;
...
  
double basePrice(){
  return _quantity * _ itemPrice;
}
```



**방법**

- 값이 한 번만 대입되는 임시변수를 찾자
- 그 임시변수를 final로 선언하자
- 컴파일을 실시하자
- 대입문 우변을 빼내어 메서드로 만들자
- 컴파일과 테스트를 실시하자
- 임시변수를 대상으로 임시변수 내용 직접 삽입 기법을 실시하자



**예제**

```java
double getPrice(){
  int basePrice = _quantity * _itemPrice;
  double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFacotr = 0.98;
  return basePrice * discountFactor;
}
```

임시변수를 final로 선언하여 그 임시변수들이 값을 한 번만 대입을 받는지 시험해보는 것도 나쁘지 않다.

```java
double getPrice(){
  final int basePrice = _quantity * _itemPrice;
  finaldouble discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFacotr = 0.98;
  return basePrice * discountFactor;
}
```

임시변수를 한번에 하나씩 메서도 호출로 바꾼다. 대입문의 우변을 메서드로 빼낸다.

```java
double getPrice(){
  final int basePrice = basePrice();
  final double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFacotr = 0.98;
  return basePrice * discountFactor;
}

private int basePrice(){
  return _quantity * _itemPrice;
}
```

컴파일 및 테스트 실시 후 임시변수 내용 직접 삽입을 실시

```java
double getPrice(){
  final int basePrice = basePrice();
  final double discountFactor;
  if (basePrice() > 1000) discountFactor = 0.95;
  else discountFacotr = 0.98;
  return basePrice * discountFactor;
}

private int basePrice(){
  return _quantity * _itemPrice;
}
```

두번째 임시변수 참조부분을 바꾼 후, 더이상 임시변수 참조부분이 없으니 임시변수 선언도 삭제

```java
double getPrice(){
  final double discountFactor;
  if (basePrice() > 1000) discountFactor = 0.95;
  else discountFacotr = 0.98;
  return basePrice() * discountFactor;
}

private int basePrice(){
  return _quantity * _itemPrice;
}
```

나머지 메서드도 추출

```java
double getPrice(){
  final double discountFactor = discountFactor();
  return basePrice() * discountFactor;
}

private int basePrice(){
  return _quantity * _itemPrice;
}

private double discountFactor(){
  if (basePrice() > 1000) return 0.95;
  else return 0.98;
}
```



**결과**

```java
double getPrice(){
  return basePrice() * discountFactor();
}

private int basePrice(){
  return _quantity * _itemPrice;
}

private double discountFactor(){
  if (basePrice() > 1000) return 0.95;
  else return 0.98;
}
```

