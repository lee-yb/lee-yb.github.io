# Object Class

Object 클래스는 자바에서 모든 클래스의 조상이다. 이 클래스에는 모든 클래스가 공통으로 포함하고 있어야 하는 기능을 제공하고 있다.

Object 클래스에 정의되어 있는 메소드를 살펴보자.



## getClass()

```java

    /**
     * Returns the runtime class of this {@code Object}. The returned
     * {@code Class} object is the object that is locked by {@code
     * static synchronized} methods of the represented class.
     *
     * <p><b>The actual result type is {@code Class<? extends |X|>}
     * where {@code |X|} is the erasure of the static type of the
     * expression on which {@code getClass} is called.</b> For
     * example, no cast is required in this code fragment:</p>
     *
     * <p>
     * {@code Number n = 0;                             }<br>
     * {@code Class<? extends Number> c = n.getClass(); }
     * </p>
     *
     * @return The {@code Class} object that represents the runtime
     *         class of this object.
     * @jls 15.8.2 Class Literals
     */
    @HotSpotIntrinsicCandidate
    public final native Class<?> getClass();
```

자신이 속한 클래스의 **[Class객체](./2020-06-17-Class-class)**를 반환하는 메서드인데, 쉽게말해 **현재 참조하고 있는 클래스를 확인할 수 있는 메소드**이다. ```System.out.println(a.getClass());``` 의 결과로 ```class java.lang.Object``` 이라면 a 는 java.lang 패키지의 Object 클래스의 인스턴스이다.



## equals(Object obj)

```java
/**
     * Indicates whether some other object is "equal to" this one.
     * <p>
     * The {@code equals} method implements an equivalence relation
     * on non-null object references:
     * <ul>
     * <li>It is <i>reflexive</i>: for any non-null reference value
     *     {@code x}, {@code x.equals(x)} should return
     *     {@code true}.
     * <li>It is <i>symmetric</i>: for any non-null reference values
     *     {@code x} and {@code y}, {@code x.equals(y)}
     *     should return {@code true} if and only if
     *     {@code y.equals(x)} returns {@code true}.
     * <li>It is <i>transitive</i>: for any non-null reference values
     *     {@code x}, {@code y}, and {@code z}, if
     *     {@code x.equals(y)} returns {@code true} and
     *     {@code y.equals(z)} returns {@code true}, then
     *     {@code x.equals(z)} should return {@code true}.
     * <li>It is <i>consistent</i>: for any non-null reference values
     *     {@code x} and {@code y}, multiple invocations of
     *     {@code x.equals(y)} consistently return {@code true}
     *     or consistently return {@code false}, provided no
     *     information used in {@code equals} comparisons on the
     *     objects is modified.
     * <li>For any non-null reference value {@code x},
     *     {@code x.equals(null)} should return {@code false}.
     * </ul>
     * <p>
     * The {@code equals} method for class {@code Object} implements
     * the most discriminating possible equivalence relation on objects;
     * that is, for any non-null reference values {@code x} and
     * {@code y}, this method returns {@code true} if and only
     * if {@code x} and {@code y} refer to the same object
     * ({@code x == y} has the value {@code true}).
     * <p>
     * Note that it is generally necessary to override the {@code hashCode}
     * method whenever this method is overridden, so as to maintain the
     * general contract for the {@code hashCode} method, which states
     * that equal objects must have equal hash codes.
     *
     * @param   obj   the reference object with which to compare.
     * @return  {@code true} if this object is the same as the obj
     *          argument; {@code false} otherwise.
     * @see     #hashCode()
     * @see     java.util.HashMap
     */
    public boolean equals(Object obj) {
        return (this == obj);
    }
```

**매개변수로 객체의 참조변수를 받아서 비교하여 결과를 boolean값으로 알려 주는 메서드**이다.

코드에서 알수 있듯이 두 객체의 같고 다름을 **```참조변수의 값```**으로 판단한다. 그렇기 때문에 서로 다른 객체를 비교하면 항상 false이다.

알고 있듯이 비교연산자 "==" 는 주소값을 비교하고 equals() 메소드는 데이터 값을 비교한다고 알고 있는데, 이는 비교하는 값클래스들이 값을 비교할수 있도록 equals 메서드를 **```오버라이딩```**하였기 때문이다

> #### ex. String 클래스의 equals()
>
> ```java
> public boolean equals(Object anObject) {
>         if (this == anObject) {
>             return true;
>         }
>         if (anObject instanceof String) {
>             String aString = (String)anObject;
>             if (coder() == aString.coder()) {
>                 return isLatin1() ? StringLatin1.equals(value, aString.value)
>                                   : StringUTF16.equals(value, aString.value);
>             }
>         }
>         return false;
>     }
> ```
>
> #### ex. Integer 클래스의 equals()
>
> ```java
> public boolean equals(Object obj) {
>         if (obj instanceof Integer) {
>             return value == ((Integer)obj).intValue();
>         }
>         return false;
>     }
> ```

이렇게 함으로써 서로 다른 인스턴스일지라도 같은 값을 가지고 있다면 equals로 비교했을때 true를 결과로 얻을 수 있다.

equals 메서드를 재정의할 때는 반드시 Object명세에 따른 [규약](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object))을 지켜야한다. 규약을 확인하면서 equals메서드를 구현한 잘못된 사례를 확인해보자.

- **반사성**(_reflexive_) - x.equals(x) 는 true다.

- **대칭성**(_symmetric_) - x.equals(y) 가 true면, y.equals(x) 도 true 이다.

  ```java
  /*잘못된 코드 - 대칭성 위배*/
  public final class CaseInsensitiveString {
    	private final String s;
    
    	public CaseInsensitiveString(String s){
        	this.s = Objects.requireNonNull(s);
      }
    
    	// 대칭성 위배!
    	@Override public boolean equals(Object o){
        	if (o instanceof CaseInsensitiveString)
            	return s.equalsIgnoreCase(((CaseInsensitiveString) o).s);
        	if (o instanceof String)	// 한 방향으로만 작동한다.
            	return s.equalsIgnoreCase((String) o);
        	return false;
      }
    	...
  }
  ```

  위 코드는 대칭성을 위배한 케이스이다. 두 객체 ```CaseInsensitiveString cis = new CaseInsensitiveString("polish");``` 와 ```String s = "polish";``` 가 있을때 cis.equals(s) 는 true를 반환한다. 문제는 CaseInsensitiveString의 equals는 일반 String을 알고 있지만 String의 equals는 CaseInsensitiveString의 존재를 모른다는 데 있다. 따라서 s.equals(cis)는 false 를 반환하여, 대칭성을 위반한다.

  이 문제를 해결하려면 CaseInsensitiveString의 equals를 String과도 연동하겠다는 꿈을 버려야 한다.

  ```java
  /*수정된 코드*/
  @Override public boolean equals(Object o){
    	return o instanceof CaseInsensitiveString &&
        	((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
  }
  ```

- **추이성**(_transitive_) - x.equals(y)가 true이고 y.equals(z)가 true 이면, x.equals(z)도 true이다. 간단하지만 자칫하면 어기기 쉽다. 상위 클래스에 없는 새로운 필드를 하위 클래스에 추가하는 상황을 보자.

  ```java
  public class Point {
    	private final int x;
    	private final int y;
    
    	public Point(int x, int y){
        	this.x = x;
        	this.y = y;
      }
    
    	@Override public boolean equals(Object o){
        	if (!(o instanceof Point))
            	return false;
        	Point p = (Point)o;
        	return p.x == x && p.y == y;
      }
    
    	...
  }
  ```

  이 클래스를 확장하는 클래스를 보자.

  ```java
  public class ColorPoint extends Point {
    	private final Color color;
    
    	public ColorPoint(int x, int y, Color color){
        	super(x, y);
        	this.color = color;
      }
    
    	...
  }
  ```

  equals 메서드를 오버라이딩 하지 않는다면 색상 정보는 무시한 채 비교를 수행한다. 비교대상이 또 다른 ColorPoint이고 위치와 색상이 같을 때만 true를 반환하는 equals를 생각해보자.

  ```java
  /*잘못된 코드 - 대칭성 위배*/
  @Override public boolean equals(Object o){
    	if (!(o instanceof ColorPoint))
        	return false;
    	return super.equals(o) && ((ColorPoint) o).color == color;
  }
  ```

  ```Point p = new Point(1, 2);``` ```ColorPoint cp = new ColorPoint(1, 2, Color.RED);``` 를 비교해보면 p.equals(cp)는 true를, cp.equals(p)는 false를 반환한다. ColorPoint.equals가 Point와 비교할 때는 색상을 무시하도록 하면 해결이 될까?

  ```java
  /*잘못된 코드 - 추이성 위배*/
  @Override public boolean equals(Object o){
    	if (!(o instanceof Point))
        	return false;
    
    	// o가 Point면 색상을 무시하고 비교한다.
    	if (!(o instanceof ColorPoint))
        	return o.equals(this);
    
    	// o가 ColorPoint면 색상까지 비교한다.
    	return super.equals(o) && ((ColorPoint) o).color == color;
  }
  ```

  위 코드는 대칭성은 지켜주지만 추이성을 깨버린다. ```ColorPoint p1 = new ColorPoint(1, 2, Color.RED);``` ```Point p2 = new Point(1, 2);``` ```ColorPoint p3 = new ColorPoint(1, 2, Color.BLUE);``` 일때 p1.equals(p2)와 p2.equals(cp3)는 true를 반환하는데, p1.equals(p3)가 false로 추이성에 위배된다. 색상까지 고려했기 때문이다.

  **결과적으로 구체 클래스를 확장해 새로운 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않는다.**

- **일관성**(_consistency_) - 두객체가 같다면 영원히 같아야 한다는 뜻이다.

- **null-아님** - o.equals(null)은 당연하게도 false 여야 한다. 실수로 NullPointerException을 던지는 상황도 있어서는 안된다. instanceof 는 첫번째 피연산자가 null이면 false를 반환한다.

위의 5가지 규약을 제외하고 추가적으로 주의할 사항으로

- equals를 재정의할 땐 hashCode 도 반드시 재정의하자.
- 너무 복잡하게 하려 들지말자.
- Object 외의 타입을 매개변수로 받는 equals 메서드는 선언하지 말자.



> **꼭 필요한 경우가 아니면 equals를 재정의하지 말자.** 많은 경우에 Object의 equals가 원하는 비교를 정확히 수행해준다. 재정의해야 할때는 그 클래스의 핵심 필드 모두를 빠짐없이, 다섯가지 규약을 확실히 지켜가며 비교해야 한다.



## hashCode()

해싱기법에 사용되는 ```해시함수``` 를 구현한 것이다. 일반적으로 **객체의 주소값을 변환하여 생성한 객체의 고유한 정수값**이다.

hashCode는 HashTable과 같은 자료구조를 사용할 때 데이터가 저장되는 위치를 결정하기 위해 사용된다.

Object 클래스에 정의된 hashCode 메서드는 객체의 주소값을 이용해서 해시코드를 만들어 반환하기 때문에 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.

```java
/**
     * Returns a hash code value for the object. This method is
     * supported for the benefit of hash tables such as those provided by
     * {@link java.util.HashMap}.
     * <p>
     * The general contract of {@code hashCode} is:
     * <ul>
     * <li>Whenever it is invoked on the same object more than once during
     *     an execution of a Java application, the {@code hashCode} method
     *     must consistently return the same integer, provided no information
     *     used in {@code equals} comparisons on the object is modified.
     *     This integer need not remain consistent from one execution of an
     *     application to another execution of the same application.
     * <li>If two objects are equal according to the {@code equals(Object)}
     *     method, then calling the {@code hashCode} method on each of
     *     the two objects must produce the same integer result.
     * <li>It is <em>not</em> required that if two objects are unequal
     *     according to the {@link java.lang.Object#equals(java.lang.Object)}
     *     method, then calling the {@code hashCode} method on each of the
     *     two objects must produce distinct integer results.  However, the
     *     programmer should be aware that producing distinct integer results
     *     for unequal objects may improve the performance of hash tables.
     * </ul>
     * <p>
     * As much as is reasonably practical, the hashCode method defined
     * by class {@code Object} does return distinct integers for
     * distinct objects. (The hashCode may or may not be implemented
     * as some function of an object's memory address at some point
     * in time.)
     *
     * @return  a hash code value for this object.
     * @see     java.lang.Object#equals(java.lang.Object)
     * @see     java.lang.System#identityHashCode
     */
    @HotSpotIntrinsicCandidate
    public native int hashCode();
```

클래스의 인스턴스변수 값으로 객체의 같고 다름을 판단해야하는 경우라면 equals메서드 뿐 아니라 hashCode메서드도 적절히 오버라이딩해야 한다.

> #### ex. Object 클래스의 hashCode()
>
> 참고) [How does the default hashCode() work?](https://srvaroa.github.io/jvm/java/openjdk/biased-locking/2017/01/30/hashCode.html)
>
> #### ex. String 클래스의 hashCode()
>
> ```java
> public int hashCode() {
>         int h = hash;
>         if (h == 0 && value.length > 0) {
>             hash = h = isLatin1() ? StringLatin1.hashCode(value)
>                                   : StringUTF16.hashCode(value);
>         }
>         return h;
>     }
> 
> private boolean isLatin1() {
>         return COMPACT_STRINGS && coder == LATIN1;
>     }
> 
> private final byte coder;
> static final boolean COMPACT_STRINGS;
> 
>     static {
>         COMPACT_STRINGS = true;
>     }
> @Native static final byte LATIN1 = 0;
> @Native static final byte UTF16  = 1;
> 
> final class StringLatin1 {
>   	public static int hashCode(byte[] value) {
>         int h = 0;
>         for (byte v : value) {
>             h = 31 * h + (v & 0xff);
>         }
>         return h;
>     }
> }
> 
> final class StringUTF16 {
>   	public static int hashCode(byte[] value) {
>         int h = 0;
>         int length = value.length >> 1;
>         for (int i = 0; i < length; i++) {
>             h = 31 * h + getChar(value, i);
>         }
>         return h;
>     }
> }
> ```
>
> #### ex. Integer 클래스의 hashCode()
>
> ```java
> @Override
>     public int hashCode() {
>         return Integer.hashCode(value);
>     }
> 
>     public static int hashCode(int value) {
>         return value;
>     }
> ```
>
> 

```java
public class Main {

    public static void main(String[] args) {
        Object obj = new Object();
        Object obj2 = new Object();
        String str = new String("123");
        String str2 = new String("123");
        Integer i = new Integer(123);
        Integer i2 = new Integer(123);

        System.out.println(obj.hashCode());                 //1057941451
        System.out.println(System.identityHashCode(obj));   //1057941451
        System.out.println(obj2.hashCode());                //1975358023
        System.out.println();

        System.out.println(str.hashCode());                 //48690
        System.out.println(System.identityHashCode(str));   //2101440631
        System.out.println(str2.hashCode());                //48690
        System.out.println();

        System.out.println(i.hashCode());                   //123
        System.out.println(System.identityHashCode(i));     //2109957412
        System.out.println(i2.hashCode());                  //123
    }
}
```

String 클래스는 문자열의 내용이 같으면, 동일한 해시코드를 반환하도록 오버라이딩 되어 있기 때문에, 문자열 내용이 같은 인스턴스에 대해 hashCode()를 호출하면 항상 동일한 해시코드값을 얻는다.



### equals와 hashCode의 관계

_동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다._ 그렇기 때문에 _**equals() 메소드를 오버라이드 한다면, hashCode() 메소드도 오버라이드 되어야 한다.**_

[**hashCode 명세**](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#hashCode()) 를 살펴보면 obj1.equals(obj2) 이 true 이면 hashCode(obj1) == hashCode(obj2) 이여야 하지만 obj1.equals(obj2)가 false 라도 hashCode(obj1) == hashCode(obj2) 일 수도 있다. 단 다른 객체에 대해서는 다른 값을 반환하는편이 성능면에서 좋다는 것을 인지해야 한다.

hashCode 재정의를 잘못했을때 문제가 되는 부분은 equals를 재정의 하여 논리적으로 같은 객체에 대해서 true를 반환할수 있다. 하지만 Object의 기본 hashCode 메서드는 둘이 다르다고 판단하여 다른값을 반환한다.

equals를 재정의한 클래스를 예제로 살펴보자

```java
public class Employee {
  	private Integer id;
  	private String firstName;
  	private String lastName;
  	private String department;
  
  	// Setter & Gettter
  
  	public boolean equals(Object o){
      	if (o == null)
          	return false;
      	if (o == this)
          	return true;
      	if (getClass() != o.getClass())
          	return false;
      
      	Employee e = (Employee) o;
      	return (this.getId() == e.getId());
    }
}
```

equals 비교는 해결된 것처럼 보인다. 하지만 Employee를 HashSet 과 같은 자료구조에 저장하려고 하면 문제가 생기게 된다.

HashTable이나 HashSet, HashMap과 같은 자료구조는 자료를 저장하기 위한 위치를 선택하기 위해 hashCode를 이용한다. 고유값인 Id를 일치시킨 두 객체를 자료구조에 넣을때 각 개체는 다른 해시값을 반환할 것이고, 서로 다른 위치에 저장될 것이다.

#### hashCode 작성요령





## toString()

**인스턴스에 대한 정보를 문자열(String)로 제공할 목적**으로 정의한 것.

```java
/**
     * Returns a string representation of the object. In general, the
     * {@code toString} method returns a string that
     * "textually represents" this object. The result should
     * be a concise but informative representation that is easy for a
     * person to read.
     * It is recommended that all subclasses override this method.
     * <p>
     * The {@code toString} method for class {@code Object}
     * returns a string consisting of the name of the class of which the
     * object is an instance, the at-sign character `{@code @}', and
     * the unsigned hexadecimal representation of the hash code of the
     * object. In other words, this method returns a string equal to the
     * value of:
     * <blockquote>
     * <pre>
     * getClass().getName() + '@' + Integer.toHexString(hashCode())
     * </pre></blockquote>
     *
     * @return  a string representation of the object.
     */
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }	
```

toString()은 일반적으로 인스턴스나 클래스에 대한 정보 또는 인스턴스 변수들의 값을 문자열로 변환하여 반환하도록 오버라이딩 되는 것이 보통이다.



## clone()

**자신을 복제하여 새로운 인스턴스를 생성**하는 일을 한다. 원래의 인스턴스는 보존하여 작업에 실패해서 원래의 상태로 되돌리거나 변경되기 전의 값을 참고하는 도움이 되게 하기 위하여 만든 메서드이다.

```java
		/**
     * Creates and returns a copy of this object.  The precise meaning
     * of "copy" may depend on the class of the object. The general
     * intent is that, for any object {@code x}, the expression:
     * <blockquote>
     * <pre>
     * x.clone() != x</pre></blockquote>
     * will be true, and that the expression:
     * <blockquote>
     * <pre>
     * x.clone().getClass() == x.getClass()</pre></blockquote>
     * will be {@code true}, but these are not absolute requirements.
     * While it is typically the case that:
     * <blockquote>
     * <pre>
     * x.clone().equals(x)</pre></blockquote>
     * will be {@code true}, this is not an absolute requirement.
     * <p>
     * By convention, the returned object should be obtained by calling
     * {@code super.clone}.  If a class and all of its superclasses (except
     * {@code Object}) obey this convention, it will be the case that
     * {@code x.clone().getClass() == x.getClass()}.
     * <p>
     * By convention, the object returned by this method should be independent
     * of this object (which is being cloned).  To achieve this independence,
     * it may be necessary to modify one or more fields of the object returned
     * by {@code super.clone} before returning it.  Typically, this means
     * copying any mutable objects that comprise the internal "deep structure"
     * of the object being cloned and replacing the references to these
     * objects with references to the copies.  If a class contains only
     * primitive fields or references to immutable objects, then it is usually
     * the case that no fields in the object returned by {@code super.clone}
     * need to be modified.
     * <p>
     * The method {@code clone} for class {@code Object} performs a
     * specific cloning operation. First, if the class of this object does
     * not implement the interface {@code Cloneable}, then a
     * {@code CloneNotSupportedException} is thrown. Note that all arrays
     * are considered to implement the interface {@code Cloneable} and that
     * the return type of the {@code clone} method of an array type {@code T[]}
     * is {@code T[]} where T is any reference or primitive type.
     * Otherwise, this method creates a new instance of the class of this
     * object and initializes all its fields with exactly the contents of
     * the corresponding fields of this object, as if by assignment; the
     * contents of the fields are not themselves cloned. Thus, this method
     * performs a "shallow copy" of this object, not a "deep copy" operation.
     * <p>
     * The class {@code Object} does not itself implement the interface
     * {@code Cloneable}, so calling the {@code clone} method on an object
     * whose class is {@code Object} will result in throwing an
     * exception at run time.
     *
     * @return     a clone of this instance.
     * @throws  CloneNotSupportedException  if the object's class does not
     *               support the {@code Cloneable} interface. Subclasses
     *               that override the {@code clone} method can also
     *               throw this exception to indicate that an instance cannot
     *               be cloned.
     * @see java.lang.Cloneable
     */
    @HotSpotIntrinsicCandidate
    protected native Object clone() throws CloneNotSupportedException;
```

Object 클래스에 정의된 clone()은 _인스턴스변수의 값만을 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다._

예를들어 배열의 경우 복제된 인스턴스도 같은 배열의 주소를 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다. 그렇기 때문에 오버라이딩해서 clone 메서드를 구현하여야 한다.

clone() 을 사용하기 위해서는 복제할 클래스가 **```Cloneable 인터페이스```**를 구현해야하고, 오버라이드 된 clone() 의 **```접근 제어자를 public```** 으로 변경한다. 그래야 상속관계가 없는 다른 클래스에서 clone()을 호출할 수 있다.

```java
package java.lang;

/**
 * A class implements the <code>Cloneable</code> interface to
 * indicate to the {@link java.lang.Object#clone()} method that it
 * is legal for that method to make a
 * field-for-field copy of instances of that class.
 * <p>
 * Invoking Object's clone method on an instance that does not implement the
 * <code>Cloneable</code> interface results in the exception
 * <code>CloneNotSupportedException</code> being thrown.
 * <p>
 * By convention, classes that implement this interface should override
 * {@code Object.clone} (which is protected) with a public method.
 * See {@link java.lang.Object#clone()} for details on overriding this
 * method.
 * <p>
 * Note that this interface does <i>not</i> contain the {@code clone} method.
 * Therefore, it is not possible to clone an object merely by virtue of the
 * fact that it implements this interface.  Even if the clone method is invoked
 * reflectively, there is no guarantee that it will succeed.
 *
 * @author  unascribed
 * @see     java.lang.CloneNotSupportedException
 * @see     java.lang.Object#clone()
 * @since   1.0
 */
public interface Cloneable {
}
```

Cloneable 인터페이스를 구현하게 하는 이유는 인스턴스의 데이터를 보호하기 위해서이다. _**Cloneable 인터페이스가 구현되어 있다는 것은 클래스 작성자가 복제를 허용한다는 의미**_이다.



### 얕은복사 와 깊은복사

clone()은 단순히 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지 복제하지는 않는다.

배열인 경우를 예로들면 기본형 배열인 경우에는 아무런 문제가 없지만, 객체배열을 clone()으로 복제하는 경우에는 **원본과 복제본이 같은 객체를 공유**하므로 완전한 복제라고 보기 어렵다. 이러한 복제를 **```얕은복사(shallow copy)```**라고 한다.

반면에 **원본이 참조하고 있는 객체까지 복제**하는 것을 **```깊은복사(deep copy)```**라고 하며, 원본의 변경이 복사본에 영향을 미치지 않는다.

> #### 예제 클래스
>
> ```java
> public class Stack{
> 		private Object[] elements;
>   	private int size = 0;
>   	private static final int DEFAULT_INITIAL_CAPACITY = 16;
>   
>   	...
> }
> ```
>
> #### ex. 가변상태를 참조하는 클래스용 재귀적 clone
>
> ```java
> @Override public Stack clone() {
>   	try{
>       	Stack result = (Stack) super.clone();
>       	result.elements = elements.clone();
>       	return result;
>     } catch (CloneNotSupportedException e) {
>       	throw new AssertionError();
>     }
> }
> ```
>
> #### ex. 복잡한 가변상태를 갖는 클래스용 재귀적 clone
>
> ```java
> public class HashTable implements Cloneable {
>   	private Entry[] buckets = ...;
>   	
>   	private static class Entry{
>       	final Object key;
>       	Object value;
>       	Entry next;
>       	
>       	Entry(Object key, Object value, Entry next){
>           	this.key = key;
>           	this.value = value;
>           	this. next = next;
>         }
>       
>       	//이 엔트리가 가리키는 연결 리스트를 재귀적으로 복사
>       	Entry deepCopy() {
>           	return new Entry(key, value, next == null? null : next.deepCopy());
>         }
>     }
>   	
>   	@Override public HashTable clone() {
>       	try {
>           	HashTable result = (HashTable) super.clone();
>           	result.buckets = new Entry[buckets.length];
>           	for (int i = 0; i < buckets.length; i++)
>               	if (buckets[i] != null)
>                   	result.buckets[i] = buckets[i].deepCopy();
>           	return result;
>         } catch (CloneNotSupportedException e) {
>           	throw new AssertionError();
>         }
>     }
>   	...
> }
> ```
>
> 이 방법은 간단하며, 버킷이 너무 길지 않다면 잘 작동한다. 하지만 리스트의 원소 수만큼 스택 프레임을 소비하며, 리스트가 길면 스택 오버플로를 일으킬 위험이 있어 그다지 좋지 않다.
>
> #### ex. 엔트리 자신이 가리키는 연결 리스트를 반복적으로 복사
>
> ```java
> Entry deepCopy() {
>   	Entry result = new Entry(key, value, next);
>   	for (Entry p = result; p.next != null; p = p.next)
>       	p.next = new Entry(p.next.key, p.next.value, p.next.next);
>   	return result;
> }
> ```
>
> 