---
description: 자바의 신을 읽고 중요한 점을 정리합니다.
---

# 📑 자바의 신 (도서)&#x20;

## 클래스는 상태와 행위가 있어야 한다.

클래스를 이용해서 우리는 객체(인스턴스)를 생성자를 통해  생성한다. &#x20;



자바에서는 4가지의 변수가 존재한다.

1. 지역 변수 (local variables) : 중괄호 내에서 선언된 변수로 중괄호 내에서만 유효
2. 매개 변수 (parameters) : 메소드에 넘겨주는 변수 메소드 호출 및 종료시 생성 소멸
3. 인스턴스 변수 (instance variables) : 메소드 밖에, 클래스 안에 선언된 변수, static 없어야 함 객체가 생성될때 생성되고 객체 참조하는 다른 객체가 없을때 소멸
4. 클래스 변수 (class variables) : 인스턴스 변수 중 static 예약어 변수 클래스가 처음 호출될때 생성되고 자바 프로그램이 종료될때 소멸

GC (Garbage Collector)에 변수마다 메모리를 자동 해제&#x20;



System.out.println 은 어떻게 객체 생성 없이 사용이 가능할까?

System 클래스에 out이 정적으로 정의되어 있기 때문에 JVM 시작 시 System.out 자체가 미리 만들어진 싱글톤 인스턴스를 참조하게 됩니다. 따라서 객체 생성 없이 println을 사용 가능 하게 됩니다.

```
public class ReferenceStaticVariable {
    static String name;
    public ReferenceStaticVariable() {}
    public ReferenceStaticVariable(String name) {this.name = name;}

    public static void main(String[] args) {
        ReferenceStaticVariable ref = new ReferenceStaticVariable();
        ref.checkName();
    }
    public void checkName() {
        ReferenceStaticVariable ref = new ReferenceStaticVariable("dongjae");
        System.out.println(ref.name);
        ReferenceStaticVariable ref2 = new ReferenceStaticVariable("dongjae2");
        System.out.println(ref.name);
    }
}

```

위에서는 static 사용 시 ref.name이 dongjae2로 변경되는 문제가 발생한 코드이다.&#x20;

static은 JVM 시 싱글톤 인스턴스를 참조하게 되므로 클래스 변수 사용에 주의가 필요하다.



Pass&#x20;

