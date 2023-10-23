# Dependency Injection

### 목차

* Spring AOP(Aspect Oriented Programming)
* Denpendency Injection
* 싱글턴 패턴
* IoC(Inversion of Control)
* Spring Bean
* BeanFactory

### 강의 정리

### Factory 패턴

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

객체 지향 프로그래밍에서 팩토리는 다른 객체를 생성하기 위한 객체이다.&#x20;

왜 사용하는걸까?&#x20;

객체 생성 코드를 분리하여 결합도를 낮추어 유지보수하기 편리하도록 코드를 건드리는 횟수를 최소화 하기 위한 패턴이다.&#x20;

결합도가 왜 낮아질까?

인터페이스를 통해 구현한 객체들은 같은 인터페이스에 의존되기 때문에 코드에 유연함이 생긴다.&#x20;

팩토리 패턴을 통해 변경이 일어나는 부분을 캡슐화 한다면 공통된 부분은 코드 중복을 줄이고 다형성의 이점을 가져갈 수 있다.



### Spring AOP(Asepect Oriented Programming)

관점 지향 프로그래밍 AOP는 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어 각각 모듈화 하겠다는 것이다.

객체지향 프로그래밍 방식에서 실제 핵심 로직 수행 메서드를 아무리 설계를 잘해도 분리된 모듈로 작성하기 힘든 부분이 발생하는데 예를 들면 로깅,인증, 트랜잭션과 같은 공통적이고 중복되는 내용이 많다. 이는 핵심 로직을 파악하기 어렵게하고 코드 가독성을 떨어뜨리기 때문에 AOP라는 개념이 탄생하게 되었다.



\| AOP 주요 개념

* Aspect : 위에서 설명한 흩어진 관심사를 모듈화 한 것. 주로 부가기능을 모듈화함.
* Target : Aspect를 적용하는 곳 (클래스, 메서드 .. )
* Advice : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
* JointPoint : Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능
* PointCut : JointPoint의 상세한 스펙을 정의한 것. 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음

&#x20;

\| 스프링 AOP 특징

* 프록시 패턴 기반의 AOP 구현체, 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위해서임
* 스프링 빈에만 AOP를 적용 가능
* 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적

### 싱글턴 패턴

## Singleton 정의 <a href="#singleton" id="singleton"></a>

* 특정 클래스에서 만들 수 있는 인스턴스 수를 하나로 제한
*   조건

    * 프로그램 실행 중에 최대 하나만 있어야 함
      * Setting, file system
    * 전역적 접근 지점을 제공해야 함


* 특징
  * 객체이기 때문에 힙에 싱글톤 객체가 저장된다. 따라서 쓰레드간 공유가 가능하다

## Singleton의 비공식적 정의 <a href="#singleton" id="singleton"></a>

* static 사용에 대한 비판을 해결하는 패턴
* 즉, OO에서 전역 변수 및 전역 함수를 만드는 법

### Dependency Injection과 IoC

* IoC는 프레임워크의 공통적인 특징으로 프로그래머가 작성한 코드가 라이브러리의 흐름 제어를 받게된 소프트웨어 디자인 패턴이다.&#x20;

제어 역전은 프레임워크를 라이브러리와 다르게 만드는 핵심 부분입니다. 라이브러리는 본질적으로 호출할 수 있는 함수 세트이며 요즘에는 일반적으로 클래스로 구성됩니다. 각 호출은 일부 작업을 수행하고 클라이언트에 제어권을 반환합니다.

원래 코드

```
public class ServerFacade
{
  public Object respondToRequest(Object pRequest)
  {
    if(businessLayer.validateRequest(pRequest))
    {
      DAO.getData(pRequest);
      return Aspect.convertData(pRequest);
    }

    return null;
  }
}
```

제어 반전 코드

```
public class ServerFacade
{
  public Object respondToRequest(Object pRequest, DAO dao)
  {
    return dao.getData(pRequest);
  }
}
```

### DI란?

DI는 Dependency Injection으로 어플리케이션 클래스에서 의존성을 제거하는 방법의 일종이다.

내용은 인터페이스에만 의존하고 실제 구현을 위한 정보는   외부에서 주입받아 만든다.

DI에서 dependency를 의존객체라고 보는 것은 적절치 않습니다.대신 코드 레벨에서 제거했지만, 런타임시에 가지게 해주는 의존관계 또는 의존성으로 보는 것이 타당하다는 것입니다.\


우선 `의존성`이란 무엇인지부터 간단하게 짚고 넘어가자.

> _의존대상 B가 변하면, 그것이 A에 영향을 미친다._
>
> **- 이일민, 토비의 스프링 3.1, 에이콘(2012), p113**

```java
public class A {
    private B b = new B();
}
```

➡️ 생성자, Setter, 인터페이스를 통해 주입할 수 있으며 DI를 통해 객체 간 의존성을 줄여 변경이 덜 취약한 코드를 만들고 모의 객체를 주입하여 단위 테스트를 용이하게 만들 수 있다.

### 궁금한 점?

1. 싱글턴 패턴과 정적클래스의 차이가 무엇인가?

\=> **싱글턴 패턴은 그래도 객체를 생성한다. (확장과 인터페이스 구현이 가능)**



**싱글턴 패턴은 코드 상에서 의존 관계를 명확하게 보여준다.**

싱글턴 패턴은 상태를 가지고 생성과 파괴시기를 선택할 수 있어 테스트 하는데 문제가 없지만 정적 static class는 mocking이 불가능하거나 구현에 제한이 있다 또한정적 스택에 저장이 되기 때문에 쓰레드 관리가 어렵다.



```
   void myMethod(Singleton singleton) {
   	singleton.methodA();
  }

  // 또는

  void myMethod() {
  	Singleton singleton = Singleton.valueOf();
  	singleton.methodA();
  }
```
