# SOLID 원칙

SOLID원칙의 개념은 Robert C. Martin이 2000년도에 발표한
Design Principles and Design Patterns라는 논문에서 처음 소개되었다.
이후에 이 컨셉들을 Michael Feathers가 발전시켜 SOLID라는 약자로 소개했다.
그 이후 20년동안 이 다섯가지 원칙은 객체지향 프로그래밍 세계를 혁신했고
우리가 소프트웨어를 작성하는 방식을 변화시켜왔다.

---

그러면 SOLID 원칙이란 무엇이며 어떻게 이 원칙이 더 좋은 소프트웨어를 작성할수 있게 도와주는가?
간단히 말하면 Martin과 Feather의 디자인 원칙은 **우리가 좀 더 유지보수가 쉽고, 이해가 쉽고, 유연한 소프트웨어를 작성하도록 장려한다.** 그렇게 됨으로써 **우리의 프로그램이 크기가 커져도 복잡성을 줄일수 있고** 생겨날 수많은 머리아픈 문제로부터 벗어날 수 있다.

SOLID 원칙은 다음 5개의 컨셉으로 이뤄져있다.

1. **Single Responsibility - 단일역할**
2. **Open/Closed - 개방성/폐쇄성**
3. **Liscov Substitution - 리스코프 치환**
4. **Interface Segregation - 인터페이스 분리**
5. **Dependency Inversion - 의존성 역전**

---

### 1.Single Responsibility - 단일역할

이 원칙은 **한개의 클래스는 하나의 역할만 가져야 한다**는 것을 말한다. 더 나아가서는 **수정을 하기 위해서는 반드시 하나의 이유만 있어야한다**는 뜻이다.

어떻게 이 원칙이 나은 프로그램을 만들수 있게 하는가?
이 원칙의 장점을 몇가지 살펴보자:

1. **테스트 - 한개의 역할만 하는 클래스는 테스트 케이스 갯수가 적어진다**
2. **낮은 결합도 - 클래스에 기능이 적을수록 더 적은 의존성을 가진다.**
3. **정돈 - 크기가 작고, 잘 정돈된 클래스일수록 모노리딕한 것들보다 찾기가 쉽다.**

```java
public class Book {
	private String name;
	private String author;
    private String text;
 
    //constructor, getters and setters
}
```

이 코드에서는 이름과 저자,내용을 Book의 인스턴스에 저장한다.

이제 책의 내용을 가져오는 몇개의 메소드를 추가해보자.

```java
public class Book {
 
    private String name;
    private String author;
    private String text;
 
    //constructor, getters and setters
 
    // methods that directly relate to the book properties
    public String replaceWordInText(String word){
        return text.replaceAll(word, text);
    }
 
    public boolean isWordInText(String word){
        return text.contains(word);
    }
}
```

Book 클래스는 잘 동작하고 우리는 프로그램에서 원하는 만큼의 책을 저장할 수 있다. 그런데, 콘솔에 출력을 못해서 읽지도 못하는 내용을 저장해서 뭐하나?

고민하지 말고 다음의 print 메소드를 추가해보자.

```java
public class Book {
    //...
 
    void printTextToConsole(){
        // our code for formatting and printing the text
    }
}
```

이 코드는 동작하지만 앞서 말했던 단일 역할 원칙을 위배한다. 이걸 고치려면 오직 책의 text만 print하는 클래스를 따로 분리해서 구현해야 한다.

```java
public class BookPrinter {
 
    // methods for outputting text
    void printTextToConsole(String text){
        //our code for formatting and printing the text
    }
 
    void printTextToAnotherMedium(String text){
        // code for writing to any other location..
    }
}
```

Book 클래스에서 print하는 기능을 분리해서 가볍게 만들었을뿐만 아니라 BookPrinter가 책 내용을 다른 출력매체에 출력할 수 있도록 활용할수도 있게 구현했다.

이메일에 출력하든 logging이든 다른 어떤것이든 간에 이 문제에 대해서만 관심을 가지는 분리된 클래스를 가지게 되었다.

---

### 2.Open/Closed - 개방성/폐쇄성

**확장에 대해서는 열려있고 수정에 대해서는 폐쇄적이어야 한다.**
간단히 말해서 클래스는 확장에 대해서는 개방적이어야하고
수정에 대해서는 폐쇄적이야 한다는 말이다. 그렇게 함으로써 기존의 코드를 수정하느라 새로운 버그를 만들어내는 것을 방지하고 행복한 응용프로그램을 만들어낼 수 있다.

물론 기존 코드에 있는 버그를 고치는 것은 이 원칙에서 예외이다.

간단한 코드 예제로 이 컨셉을 살펴보자. 새 프로젝트의 일환으로 Guitar 클래스를 구현했다고 가정해보자.

```java
public class Guitar {
 
    private String make;
    private String model;
    private int volume;
 
    //Constructors, getters & setters
}
```

응용프로그램을 런치했고 모두들 좋아했다. 그런데 몇달이 지나자 Guitar가 조금 지루해졌고 더 '락앤롤'스럽게 보이기 위해 멋진 불꽃 패턴을 집어넣기로 결정했다.

이 시점에서 Guitar클래스파일을 열고서는 불꽃무늬 패턴을 집어넣고 싶은 유혹이 들것이다 - 하지만 그렇게 하면 우리 응용프로그램에서 에러가 발생할수 있을지도 모른다.

그렇게 하는 대신에 개방성/폐쇄성 원칙에 따라 간단하게 Guitar 클래스를 확장하도록 하자.

```java
public class SuperCoolGuitarWithFlames extends Guitar {
 
    private String flameColor;
 
    //constructor, getters + setters
}
```

Guitar 클래스를 확장함으로써 우리는 기존의 응용프로그램이 영향받지 않는다는 것을 보장할 수 있다.

---

### 3.Liscov Substitution - 리스코프 치환
이는 논란의 여지가 있겠지만 5가지 원칙중에서 가장 복잡한 원칙이다. 간단하게 말하면, 클래스A가 클래스B의 서브타입이라면 프로그램 동작을 방해하지 않고, 프로그램에서 B를 A로 대체할 수 있어야 한다는 것이다.

이 난해한 컨셉을 이해해볼 수 있도록 도와주는 코드를 바로 살펴보자.

```java
public interface Car {
 
    void turnOnEngine();
    void accelerate();
}
```

위의 코드에서는 간단한 Car 인터페이스를 정의했다. 인터페이스에는 모든 차량이 수행해야하는 몇가지의 메소드들을 정의했다 - 엔진켜기와 전진 같은.

이 인터페이스를 구현하고 메소드들에 코드를 넣어보자.

```java
public class MotorCar implements Car {
 
    private Engine engine;
 
    //Constructors, getters + setters
 
    public void turnOnEngine() {
        //turn on the engine!
        engine.on();
    }
 
    public void accelerate() {
        //move forward!
        engine.powerOn(1000);
    }
}
```

코드에서 보이듯이, 시동을 걸 수 있는 엔진이 생겼고 또한 출력을 높일수도 있다. 아 그런데, 지금은 2019년이 되었고, 이제 우리는 전기차의 시대에 살고 있다.

```java
public class ElectricCar implements Car {
 
    public void turnOnEngine() {
        throw new AssertionError("I don't have an engine!");
    }
 
    public void accelerate() {
        //this acceleration is crazy!
    }
}
```

엔진없는 차가 섞이면서 프로그램의 동작을 본질적으로 바꾸어버리게 됐다. 이것은 명백한 리스코프 치환 원칙을 위배한 것이고 이 경우에는 앞의 두 원칙의 경우보다 조금 더 고치기가 어렵다.

한가지 가능한 해법은 **모델을 재구성해서 우리의 Car 인터페이스가 엔진이 없는 상태도 고려할 수 있게 작업**하는 것이다.

---

### 4.Interface Segregation - 인터페이스 분리

SOLID에서의 I는 인터페이스 분리를 의미한다. 간단히 말하면 큰 인터페이스는 더 작은 인터페이스들로 쪼개져야 한다는 것이다. 그렇게 함으로써, 그 클래스에서 관심을 가져야하는 메소드에만 신경쓰면서 클래스를 구현하는것을 보장할 수 있다.

예를 들기 위해 한번 사육사가 되어보기로 하자.

곰 사육사로써의 역할을 간추린 인터페이스로 시작해보자.

```java
public interface BearKeeper {
    void washTheBear();
    void feedTheBear();
    void petTheBear();
}
```
열정있는 사육사로써 우리는 사랑스러운 곰을 씻기고 먹이는것을 매우 좋아한다. 그러나 우리 모두 곰을 쓰다듬는 것의 위험성은 잘 알고 있다. 안좋게도, 우리 인터페이스는 좀 커서 곰을 쓰다듬는 메소드를 구현하는 것 말고는 다른 선택지가 없다.

이 큰 인터페이스를 세개로 쪼개서 문제를 해결해보자.

```java
public interface BearCleaner {
    void washTheBear();
}
 
public interface BearFeeder {
    void feedTheBear();
}
 
public interface BearPetter {
    void petTheBear();
}
```

인터페이스 분리덕분에 이제 우리에게 중요한 메소드들만 구현할수 있게 되었다.

```java
public class BearCarer implements BearCleaner, BearFeeder {
 
    public void washTheBear() {
        //I think we missed a spot...
    }
 
    public void feedTheBear() {
        //Tuna Tuesdays...
    }
}
```

그리고 마침내 위험한 일들은 미친사람들한테 떠넘길수도 있게 됐다.
```java
public class CrazyPerson implements BearPetter {
 
    public void petTheBear() {
        //Good luck with that!
    }
}
```

더 나아가서, 앞서 예제에서 보았던 BookPrinter 클래스 역시 인터페이스 분리를 통해 쪼개볼 수 있을 것이다.

print 메소드만 있는 Printer 인터페이스를 구현함으로써 ConsoleBookPrinter 와 OtherMediaBookPrinter 클래스들의 인스턴스를 따로 만들어낼 수도 있다.

---

### 5.Dependency Inversion - 의존성 역전
이 의존성 역전의 원칙은 소프트웨어 모듈들을 분리시키는 것을 의미한다. 고수준 모듈이 저수준 모듈에 의존하는 대신 둘 다 추상화에 의존하게 하는 것이다.

예를 들기 위해, 옛날 시절로 돌아가서 코딩으로 Windows 98 컴퓨터를 살려내보자.

```java
public class Windows98Machine {}
```
모니터도 없고 키보드도 없는 컴퓨터가 무슨 소용인가? 이것들을 생성자에 한개씩 추가하여 우리가 생성한 모든 Windows98Computer들이 Monitor와 StandardKeyboard를 기본으로 장착되게 해보자.

```java
public class Windows98Machine {
 
    private final StandardKeyboard keyboard;
    private final Monitor monitor;
 
    public Windows98Machine() {
        monitor = new Monitor();
        keyboard = new StandardKeyboard();
    }
 
}
```

이 코드는 돌아갈 것이고 Windows98Computer 클래스 안에서 StandardKeyboard와 Monitor를 자유롭게 사용할 수 있게 됐다. 문제해결?? 살짝 부족하다. new 키워드로 StandardKeyboard와 Monitor를 선언해버려서 이 3개의 클래스들이 강하게 연결되었다.

이렇게 되면 Windows98Computer를 테스트하기 힘들뿐만아니라 StandardKeyboard를 다른 것으로 대체할 필요가 있을때 교체하기도 어렵다. 역시나 Monitor클래스에도 고정돼버렸다.

좀 더 일반적인 Keyboard 인터페이스를 추가하고 클래스에서 이용해서 우리의 기계를 StandardKeyboard로부터 분리해보자.

```java
public interface Keyboard { }
```

```java
public class Windows98Machine{
 
    private final Keyboard keyboard;
    private final Monitor monitor;
 
    public Windows98Machine(Keyboard keyboard, Monitor monitor) {
        this.keyboard = keyboard;
        this.monitor = monitor;
    }
}
```

이제 이 클래스들은 분리되었고 Keyboard 인터페이스 추상화를 통해 통신하게 되었다. 원한다면 이 기계의 키보드를 인터페이스를 구현하는 다른 종류의 키보드로 쉽게 바꿀 수 있다. Monitor 클래스에 대해서도 같은 원칙을 적용해볼 수 있을 것이다.

훌륭하다. 이제 의존성들을 다 분리해냈고 Windows98Machine은 우리가 원하는 어떤 테스팅 프레임워크를 통해서도 테스트가 가능하다.
