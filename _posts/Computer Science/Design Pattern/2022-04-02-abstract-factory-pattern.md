---
title: "추상 팩토리(Abstract Factory) 패턴"

categories: [Computer-Science, Design-Pattern]
tags: [Design Pattern, Gang of Four, GoF, Abstract Factory]

date: 2022-04-02T03:35:00+09:00
last_modified_at: 2022-04-12T23:55:00+09:00
---

추상 팩토리(Abstract Factory) 패턴에 대한 정리
{:.notice--primary}

## 의도

서로 관련성이 있거나 의존 관계가 있는 객체들의 집합을 구체 클래스(Concrete Class)가 무엇인지 명시하지 않아도 생성할 수 있는 인터페이스를 제공하는 패턴이다.

## 활용성

- 시스템이 시스템의 구성요소(Product)가 생성·구성·표현되는 방식과 독립적이어야 할 때 활용할 수 있다.
- 구성요소에 대한 여러 집합 중 하나를 선택해서 시스템을 구성해야 할 때 활용할 수 있다.
- 서로 관련성이 있는 구성요소의 객체들이 함께 사용되도록 설계되었고, 이러한 제약을 강제하고 싶을 때 활용할 수 있다.
- 구성요소에 대한 클래스 라이브러리를 제공하되, 인터페이스만 드러내고 구현부는 숨기고 싶을 때 활용할 수 있다.

## 구조

<a href="https://user-images.githubusercontent.com/22683489/161324861-2c7d2cdd-ccd9-4a35-b940-e41391d02b39.png">
    ![구조](https://user-images.githubusercontent.com/22683489/161324861-2c7d2cdd-ccd9-4a35-b940-e41391d02b39.png){:.align-center}
</a>

### AbstractFactory

- 구성요소의 추상 객체(Abstract Object)를 생성하는 인터페이스를 선언한다.

### ConcreteFactory

- 구성요소의 구체 객체(Concrete Object)를 생성하는 오퍼레이션을 구현한다.

### AbstractProduct

- 구성요소의 유형(Type)별 인터페이스를 선언한다.

### ConcreteProduct

- 각각의 구체 팩토리(Concrete Factory)에 대응되는 구성요소의 객체를 정의한다.
- `AbstractProduct`에서 선언한 인터페이스를 구현한다.

### Client

- `AbstractFactory`와 `AbstractProduct`에 선언된 인터페이스만을 사용하여 시스템을 구성한다.

## 협력 방법

- 일반적으로 런타임 단계에서 `ConcreteFactory`의 인스턴스가 한 개 생성되며, 해당 구체 팩토리를 이용해 특정한 구성요소 집합에 포함된 객체를 생성한다.
- 클라이언트가 다른 구성요소 집합에 포함된 객체를 생성하고자 한다면, 해당 구성요소 집합을 생성할 수 있는 새로운 구체 팩토리 인스턴스를 생성해야 한다.
- `AbstractFactory`는 구성요소의 객체를 생성하는 책임을 서브클래스인 `ConcreteFactory`에 위임한다.

## 구현 방법

다음과 같이 추상 팩토리 패턴을 구현하기 위한 여러 테크닉 기법이 존재한다.

### 싱글톤(Singleton) 패턴을 적용한 팩토리

일반적으로 애플리케이션은 구성요소 집합당 하나의 `ConcreteFactory` 인스턴스를 필요로 한다. 이를 강제하기 위해서 팩토리를 구현할 때 싱글톤 패턴을 적용하기도 한다.

### 팩토리 메서드(Factory Method) 패턴을 이용한 구성요소 생성

추상 팩토리 패턴을 구현하기 위해 일반적으로 팩토리 메서드 패턴을 적용하며, 이 때 구체 팩토리는 각각의 팩토리 메서드를 오버라이딩하여 생성할 구성요소를 명시하게 된다. 이러한 구현 방식은 간단하긴 하지만, 구성요소 집합이 조금씩만 다르다 할지라도 항상 새로운 구체 팩토리 서브클래스를 생성해야만 한다는 단점이 있다.

### 프로토타입(Prototype) 패턴을 적용한 팩토리

만약 사용할 수 있는 구성요소 집합이 많다면 프로토타입 패턴을 적용하여 구체 팩토리를 구현할 수도 있다. 해당 방법에서 구체 팩토리는 집합에 포함된 각각의 구성요소에 대한 원형 인스턴스로 초기화되며, 저장한 원형 인스턴스를 복사하여 새로운 구성요소 인스턴스를 생성하게 된다. 이러한 구현 방식을 적용하면 각각의 구성요소 집합에 대한 구체 팩토리 클래스를 만들 필요가 없어진다는 장점이 있다.

``` java
class Factory {
    private Map<String, Product> productCatalog = new HashMap<>();

    public Product make(String productName) {
        Product product = null;

        try {
            product = (Product)productCatalog.get(productName).clone();
        }
        catch (Exception e) {
            e.printStackTrace();
        }

        return product;
    }

    public void addProduct(String productName, Product product) {
        productCatalog.put(productName, product);
    }
}

class Product implements Cloneable {
    protected String type;

    @Override
    public String toString() {
        return type;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class Keyboard extends Product {
    Keyboard() {
        this.type = "Keyboard";
    }
}

class Mouse extends Product {
    Mouse() {
        this.type = "Mouse";
    }
}

class Main {
    public static void main(String[] args) {
        Factory factory = new Factory();
        factory.addProduct("CustomKeyboard", new Keyboard());
        factory.addProduct("CustomMouse", new Mouse());

        System.out.println(factory.make("CustomKeyboard"));
        System.out.println(factory.make("CustomMouse"));
    }
}
```

``` bash
[출력]
Keyboard
Mouse
```

### 확장 가능한 팩토리

추상 팩토리 패턴은 보통 구성요소마다 생성하는 오퍼레이션을 따로 정의하며, 구성요소의 유형을 오퍼레이션의 시그니처에 인코딩하게 된다. 따라서 새로운 유형의 구성요소가 추가될 경우, `AbstractFactory`의 인터페이스뿐만 아니라 의존 관계가 있는 모든 클래스를 변경해야만 한다.

이를 더 유연하게 바꾸기 위해 객체를 생성하는 오퍼레이션에 파라미터를 추가하는 방법을 이용할 수 있다. 이 때 파라미터는 생성할 객체의 유형을 명세하게 되며, 정수와 문자열을 포함해 객체의 유형을 구분할 수 있는 어떠한 자료형이 와도 상관없다. 또한 이러한 접근법을 이용하면 `AbstractFactory`는 생성할 객체의 유형에 대한 파라미터를 갖는 `make()` 오퍼레이션 하나만 필요로 하게 된다.

## 예제

<a href="https://user-images.githubusercontent.com/22683489/161340052-7df4b9c4-95f1-431e-adc8-7967d3a55d77.png">
    ![예제](https://user-images.githubusercontent.com/22683489/161340052-7df4b9c4-95f1-431e-adc8-7967d3a55d77.png){:.align-center}
</a>

OS마다 서로 다른 룩앤필을 갖는 위젯을 생성하기 위해, 위와 같이 추상 팩토리 패턴을 이용한 시스템을 구성하였다.

### 위젯 팩토리 - Factory

``` java
interface WidgetFactory {
    Button createButton();
    Edit createEdit();
}

class WindowsWidgetFactory implements WidgetFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Edit createEdit() {
        return new WindowsEdit();
    }
}

class MacWidgetFactory implements WidgetFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Edit createEdit() {
        return new MacEdit();
    }
}
```

위젯 팩토리는 `AbstractFactory`에 해당하는 `WidgetFactory`와 `ConcreteFactory`에 해당하는 `WindowsWidgetFactory`, `MacWidgetFactory`로 구성된다.

`WidgetFactory`는 각각의 위젯을 생성하기 위한 인터페이스를 선언하고 있으며, `WindowsWidgetFactory`와 `MacWidgetFactory`는 이를 상속받아 OS에 알맞은 위젯을 생성하도록 구현하였다.

### 위젯 - Product

``` java
interface Button {
    void setGeometry(int x, int y, int width, int height);
}

class WindowsButton implements Button {
    @Override
    public void setGeometry(int x, int y, int width, int height) {
        System.out.println("WindowsButton: (" + x + ", " + y + ", " + width + ", " + height + ")");
    }
}

class MacButton implements Button {
    @Override
    public void setGeometry(int x, int y, int width, int height) {
        System.out.println("MacButton: (" + x + ", " + y + ", " + width + ", " + height + ")");
    }
}

interface Edit {
    void setGeometry(int x, int y, int width, int height);
}

class WindowsEdit implements Edit {
    @Override
    public void setGeometry(int x, int y, int width, int height) {
        System.out.println("WindowsEdit: (" + x + ", " + y + ", " + width + ", " + height + ")");
    }
}

class MacEdit implements Edit {
    @Override
    public void setGeometry(int x, int y, int width, int height) {
        System.out.println("MacEdit: (" + x + ", " + y + ", " + width + ", " + height + ")");
    }
}
```

위젯은 `AbstractProduct`에 해당하는 `Button`, `Edit`과 `ConcreteProduct`에 해당하는 `WindowsButton`, `MacButton`, `WindowsEdit`, `MacEdit`으로 구성된다.

`Button`과 `Edit`은 각각의 위젯에 필요한 인터페이스를 선언하며, 해당 예제에서는 구현을 간단하게 하기 위해 위젯의 좌표와 크기를 설정할 수 있는 `setGeometry()`만을 선언하였다.

### 애플리케이션 - Client

``` java
class Application {
    public void initialize(WidgetFactory factory) {
        Edit edit = factory.createEdit();
        edit.setGeometry(0, 0, 100, 50);

        Button button = factory.createButton();
        button.setGeometry(0, 50, 100, 25);
    }
}
```

앞서 정의한 위젯을 이용하는 간단한 애플리케이션을 작성하였다. 해당 애플리케이션은 파라미터로 넘어오는 `WidgetFactory`의 인스턴스에 따라 자동으로 Windows OS 또는 Mac OS에 알맞은 위젯으로 구성된다.

### 메인 함수

``` java
class Main {
    public static void main(String[] args) {
        Application application = new Application();
        application.initialize(new WindowsWidgetFactory());
        application.initialize(new MacWidgetFactory());
    }
}
```

``` bash
[출력]
WindowsEdit: (0, 0, 100, 50)
WindowsButton: (0, 50, 100, 25)
MacEdit: (0, 0, 100, 50)
MacButton: (0, 50, 100, 25)
```

애플리케이션이 파라미터로 넘어오는 `WidgetFactory`의 인스턴스에 따라 알맞은 위젯으로 구성되는지 테스트하기 위해, 위와 같은 메인 함수를 작성하였다.

## 결론

- 클라이언트와 구성요소의 구체 클래스를 분리
    - 팩토리가 구성요소의 객체를 생성하는 책임과 과정을 캡슐화한 것이기 때문에, 클라이언트로부터 구성요소의 구체 클래스가 분리된다.
    - 클라이언트는 구성요소의 인스턴스를 조작하기 위해 추상 인터페이스만을 사용하게 된다.

- 구성요소 집합을 다른 구성요소 집합으로 손쉽게 변경 가능
    - 구체 팩토리의 인스턴스를 생성하는 부분은 애플리케이션에서 한 번만 나타나기 때문에, 애플리케이션에서 사용하는 구체 팩토리를 손쉽게 변경할 수 있다.
    - 애플리케이션이 사용하는 구성요소 집합을 변경하기 위해 간단하게 구체 팩토리만 다른 것으로 바꾸면 된다.

- 구성요소 사이의 일관성 향상
    - 앞선 예제의 `Button`과 `Edit`처럼 구성요소의 객체들이 연관성을 갖도록 설계되었을 때, 추상 팩토리 패턴은 이를 손쉽게 강제할 수 있다.

- 새로운 유형의 구성요소를 지원하기 어려움
    - `AbstractFactory`의 인터페이스가 기존에 정의된 구성요소들만 생성할 수 있도록 만들어졌기 때문에, 새로운 유형의 구성요소를 생성하기 위해 기존의 추상 팩토리를 확장하는 것이 어렵다.
    - 새로운 유형의 구성요소를 지원하려면 추상 팩토리의 인터페이스를 확장해야 할 뿐만 아니라, 이를 상속받는 모든 구체 팩토리도 변경해야만 한다.

## 참고 자료

- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides, 「Design Patterns: Elements of Reusable Object-Oriented Software」, Addison-Wesley Professional, 1994