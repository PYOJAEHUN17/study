# 01. 자바의 구동 원리

### 프로그래밍의 3대 요소: 변수, 자료형, 할당
> 1. 변수 : 데이터를 저장하는 기억공간의 이름
> 2. 자료형 : 변수의 크기와 저장될 데이터의 종류를 결정
> 3. 할당 : 변수에 데이터를 저장(대입, 할당) 하는 것

#### 기본 자료형

| 자료형   | 크기             | 기본값   | 설명                     |
|----------|------------------|----------|--------------------------|
| `byte`   | 1 byte (8bit)    | `0`      | -128 ~ 127 정수          |
| `short`  | 2 bytes          | `0`      | -32,768 ~ 32,767 정수    |
| `int`    | 4 bytes          | `0`      | 기본 정수형, -21억 ~ 21억 |
| `long`   | 8 bytes          | `0L`     | 더 큰 정수형             |
| `float`  | 4 bytes          | `0.0f`   | 소수점 가능 (단정도)     |
| `double` | 8 bytes          | `0.0d`   | 더 정밀한 실수 (배정도)  |
| `char`   | 2 bytes          | `'\u0000`' | 유니코드 문자 하나       |
| `boolean`| 1 bit (논리적)   | `false`  | `true` 또는 `false`      |

**

## 시작 클래스
> "시작 클래스(Start Class)"는 프로그램 실행 시 가장 먼저 실행되는 진입점(entry point)이 정의된 클래스를 의미합니다.

Java는 public static void main(String[] args) 메서드를 **시작점(main entry point)**으로 사용합니다.

---
# 02. 자바 컴파일과 실행

### 해석(컴파일, 컴파일러): 오류, 구문체크
> 컴퓨터는 0, 1로 된 기계어 코드만 이해해요?

소스코드(원시코드, OnePlusTwo.java)   
-> 인간중심의 언어

컴파일 (컴파일러, javac.exe)   
-> 오류, 구문 체크를 통한 이상유무 확인, 기계어, 어셈블리언어

실행가능한코드(bytecode)  
-> .class, 중간어 코드

**JVM**에서 실행(java.exe)   
-> JVM(Java Virtual Machine을 통해서 실행)

| 역할               | 설명                                               |
|--------------------|----------------------------------------------------|
| 바이트코드 실행     | `.class` 파일로 컴파일된 바이트코드를 읽고 실행        |
| 메모리 관리         | 객체 생성, 가비지 컬렉션(GC) 등                     |
| 플랫폼 독립성 제공 | OS에 상관없이 Java가 동작하도록 함 (Write Once, Run Anywhere) |
| 보안 및 에러 처리   | 런타임 오류 감지 및 예외 처리 지원                  |
| 멀티스레드 지원     | 병렬 실행을 위한 스레드 관리                        |

| 구성 요소 | 설명                                                       |
|-----------|------------------------------------------------------------|
| JVM       | `.class` 파일 실행 담당 (Java Virtual Machine)             |
| JRE       | JVM + Java 실행에 필요한 라이브러리 및 클래스 파일 포함     |
| JDK       | JRE + 컴파일러(javac) 등 개발 도구 포함                    |


#### **JVM**
> JVM은 Java Virtual Machine의 약자로, 쉽게 말하면 자바 프로그램을 실행해주는 가상 컴퓨터입니다.   

JVM의 위치
|위치|설명|
|------------|----------------------------------------------|
|Java Program|개발자가 Java 프로그래밍 언어로 작성한 소스코드|
|Bytecode|JVM에서 실행할 수 있는 바이트 코드|
|**JVM**|바이트코드 실행을 담당.(클래스로더, 바이트코드 검증기, JIT (운영체제에 맞추어 실행) 컴파일러 및 가비지 수집기와  같은 구성요소 포함)
|Java Standard Library (Java API)|가전 구축 클래스 및 메서드 세트 제공|
|Java Native Interface (JNI)|JNI를 사용하면 Java프로그램이 C 및 C++와 같은 언어로 작성된 기본 코드와 상호 작용가능|
|Java Native Libraries|JNI를 사용하여 호출 할 수 있는 네이티브 라이브러리|
|**OS and Hardware**|-|

# 03. Bytecode란 무엇인가
**OnePlusTwo.java**
```java
public class OnePlusTwo {
    public static void main(String[] args) {
        int a, b, sum; // 변수 선언 -> int(크기:4 Byte, 종류: 정수)
        a = 1;
        b = 1;
        sum = a+b;
        System.out.println(sum);
    }
}
```
**OnePlusTwo bytecode**   
$ javap -c build//classes/java/main/org/example/OnePlusTwo.class 

```java
Compiled from "OnePlusTwo.java"
public class OnePlusTwo {
  public OnePlusTwo();  // 생성자 메서드
    Code:
       0: aload_0 // 로컬 변수 스택에 현재 객체(this)를 로드한다.
       1: invokespecial #1 // Method java/lang/Object. 를 호출한다.
       4: return  // 생성자가 완료되고 반환된다.

  public static void main(java.lang.String[]);
    Code:
       0: iconst_1           // push int 1 onto stack
       1: istore_1           // store into local variable 1 (a)
       2: iconst_1           // push int 1 onto stack
       3: istore_2           // store into local variable 2 (b)
       4: iload_1            // load local variable 1 (a)
       5: iload_2            // load local variable 2 (b)
       6: iadd               // add integers
       7: istore_3           // store result in local variable 3 (sum)
       8: getstatic     #2   // Field java/lang/System.out:Ljava/io/PrintStream;
      11: iload_3            // load sum
      12: invokevirtual #3   // Method java/io/PrintStream.println:(I)V
      15: return
}

```
> 바이트 코드는 어셈블리 언어로 구성되어 있습니다.  
> 이처럼 자바는 소스코드를 .class 파일로 컴파일할 때 JVM 명령어 집합으로 번역되며, 이 바이트코드는 JVM에서 실행됩니다.
   
# 04. JVM을 통한 메모리 이해하기
> 중간어 코드 = Bytecode = 실행가능한 코드   
> ___.java -> ___.class  
> 각 운영체제용 JVM이 ___.java 코드를 __.class 파일로 컴파일해준다.  
> JIT 는 ___.exe 파일로 바꿔줌

### JVM의 메모리 모델
|Method area|Stack area|Heap area|Literal Pool area|
|-----------|-----------|-----------|-----------|
|Method의 기계어 코드가 로딩|메서드의 호출 상태가 저장 (지역변수, 매개변수가 할당)|객체가 생성되는 메모리 영역|문자열 상수가 저장되는 공간|
|class meta data가 기록되는 곳|Call stack Frame Area|gc(Garbage Collector) 대상|재활용 메모리 공간|
|static Zone  none static zone|LIFO구조|new연산자, 생성자 메소드를 사용   동적할당(malloc, free) C언어|-|

# 05. 코드로 알아보는 JVM의 메모리
```java
package org.example;

public class Model1 {
    public static void main(String[] args) {
        int a, b, sum;
        a = 1;
        b = 1;
        sum = Model1.hap(a, b); // static 이므로 즉시 호출
        System.out.println(sum);
    }
    public static int hap(int a, int b){
        int sum = a + b;
        return sum;
    }
}
```
![alt text](image-1.png)

# 06. 코드로 알아보는 JVM의 메모리

클래스 메서드
> static이 붙어있는 클래스로써, 클래스 이름으로 접근 가능   

인스턴스 메서드
> none static 클래스
```java
package org.example;

public class Model2 {
    // 멤버변수는 new를 통해 객체가 생성될 때 메모리에 올라감.
    
    // 디폴트 생성자 메서드 (객체의 초기화를 담당하는 역할)
    public Model2(){
    }
    public static void main(String[] args) {
        int a, b, sum;
        a = 1;
        b = 1;
        // 객체를 생성 Method Area의 metadata를 Heap Area에 로드드
        Model2 m2 = new Model2();
        sum = m2.hap(a, b);
        System.out.println(sum);
    }

    public int hap(int a, int b){
        int sum = a + b;
        return sum;
    }
}
```
![alt text](image-2.png)

# 07. 코드로 알아보는 JVM의 메모리3

```java
package org.example;

public class Model3 {
    public static void main(String[] args) {
        int a = 1;
        int b = 1;
        CalHap ch = new CalHap(a, b); // 객체생성 (클래스를 사용하기 위해 메모리에 올리는 작업)
        ch.hap();
        int sum = ch.getSum();
        System.out.println(sum);
    }
}
```
```java
package org.example;

public class CalHap {
    private int a;
    private int b;
    private int sum;
    public CalHap(int a, int b){
        this.a = a;
        this.b = b;
    }
    public void hap(){
        sum = a + b;
    }
    public int getSum(){
        return sum;
    }
}
```

![alt text](image-3.png)

# 08. 프로세스와 스레드
#### 프로세스
> 프로세스 = 실행 중인 프로그램 하나
- 우리가 .exe, .java, .py 등을 실행하면 OS는 이를 프로세스로 만들어 메모리를 할당합니다.  
- 각각의 프로세스는 **독립된 메모리 공간(코드, 데이터, 스택 등)**을 가지고 있음  
- 기본적으로 서로 다른 프로세스는 메모리를 공유하지 않음 (통신하려면 IPC 필요)
  
#### 스레드
> 스레드 = 프로세스 안에서 실행 흐름을 나눈 작업 단위
- 하나의 프로세스 안에는 여러 개의 스레드가 있을 수 있음 (멀티스레딩)
- 스레드들은 코드, 힙 메모리 등을 공유하고, Stack만 독립적으로 가짐
- 하나의 스레드가 예외로 죽어도 다른 스레드는 영향을 받을 수 있음

| 구분      | 프로세스(Process)                              | 스레드(Thread)                                |
|-----------|------------------------------------------------|------------------------------------------------|
| 정의      | 실행 중인 프로그램 하나                        | 프로세스 내 실행 흐름의 단위                   |
| 메모리    | 각각 독립된 메모리 공간을 가짐                  | 프로세스의 메모리 공간을 공유                  |
| Stack     | 각각 독립적                                     | 스레드마다 별도 Stack                         |
| 속도      | 생성/전환 느림                                 | 생성/전환 빠름                                |
| 오류 영향 | 다른 프로세스에 영향 없음                      | 프로세스 전체에 영향 가능                     |
| 예시      | 크롬, Eclipse, 게임 등                          | 워드의 입력/렌더링/저장 스레드 등             |


#### 자바에서의 프로세스와 스레드
> 메인 스레드 -> java 애플리케이션 실행을 담당하는 초기스레드  

| | |
|--------------|-------------------------------------|
|JVM 프로세스 생성|JVM이 별도의 프로세스로 시작된다. 이 JVM 프로세스는 JAVA프로그램 실행을 담당합니다.|
|메인 스레드생성|JVM 프로세스 내부에서 메인 스레드가 생성됩니다. 이 메인 스레드는 JAVA 프로그램의 진입점이며 프로그램 메인 클래스의 main메소드 실행을 담당합니다.|
|메인스레드 실행|메인 스레드는 프로그램 main 메서드 내에서 코드 실행을 시작합니다. 여기가 프로그램 실행이 시작되는 곳입니다.|


![alt text](image-4.png)
