---
title: "빌더(Builder) 패턴"

categories: [Computer-Science, Design-Pattern]
tags: [Design Pattern, Gang of Four, GoF, Builder]

date: 2022-04-03T02:55:00+09:00
last_modified_at: 2022-04-04T22:42:00+09:00
---

빌더(Builder) 패턴에 대한 정리
{:.notice--primary}

## 의도

동일한 구성 절차를 이용해 다양한 표현을 생성할 수 있도록 하기 위해서, 복잡한 객체의 표현으로부터 생성을 분리하는 패턴이다.

## 활용성

- 복잡한 객체를 생성하는 알고리즘이 해당 객체의 부속품(Part)을 조립하는 방법과 독립적이어야 할 때 활용할 수 있다.
- 생성하려는 객체가 다양한 표현을 갖고 있고, 이를 구성 절차에서 지원할 수 있어야 할 때 활용할 수 있다.

## 구조

![구조](https://user-images.githubusercontent.com/22683489/161396023-6c9f7bc1-2c70-4fcc-86e0-a2494972d4c0.png){:.align-center}

### Builder

- `Product` 객체의 부속품을 생성하기 위한 인터페이스를 명세한다.

### ConcreteBuilder

- `Builder`에서 명세한 인터페이스를 구현함으로써, 시스템의 구성요소(Product)에 필요한 부속품을 생성하고 조립한다.
- 구성 과정을 기록하고 저장한다.
- 만들어진 구성요소를 획득할 수 있는 인터페이스를 제공한다.

### Director

- `Builder`의 인터페이스를 사용하여 객체를 구성한다.

### Product

- 생성하려는 복잡한 객체에 해당하며, `ConcreteBuilder`는 `Product` 객체의 내부 표현을 구성하고 조립할 수 있는 절차를 정의한다.
- 구성에 사용되는 부속품을 정의하는 클래스뿐만 아니라, 부속품을 조립하여 최종 결과물을 만들어내는 인터페이스도 `Product`에 해당한다.

## 협력 방법

![협력 방법](https://user-images.githubusercontent.com/22683489/161396507-891759a0-efeb-4085-a64e-f6e4468fa9c5.png){:.align-center}

- 클라이언트는 `Director` 객체를 생성하고, 해당 객체에 자신이 원하는 `Builder` 객체를 덧붙인다.
- 디렉터는 구성요소의 부속품을 생성해야 할 때마다 빌더에게 통보(Notification)한다.
- 빌더는 디렉터의 요청을 처리하고 구성요소에 부속품을 추가한다.
- 클라이언트는 만들어진 구성요소를 빌더로부터 획득한다.

## 구현 방법

일반적으로 빌더 패턴은 각각의 컴포넌트를 생성할 수 있는 오퍼레이션이 정의된 추상 클래스(Abstract Class)인 `Builder`를 이용해 구현된다. `Builder` 클래스의 각 오퍼레이션은 기본적으로 아무런 동작도 하지 않도록 구현되어, 이를 상속받은 `ConcreteBuilder` 클래스가 필요로 하는 오퍼레이션만 오버라이딩 할 수 있도록 만든다.

## 예제

![예제](https://user-images.githubusercontent.com/22683489/161397358-6733a25b-9dbb-4525-852d-b55bdef3b9b1.png){:.align-center}

텍스트 포맷 또는 JSON 포맷으로 To-Do를 생성할 수 있도록 하기 위해, 위와 같이 빌더 패턴을 이용한 시스템을 구성하였다.

### To-Do 빌더 - Builder

``` java
interface ToDoBuilder {
    void buildToDo(LocalDate date, String... list);
    Buffer getToDo();
}

class TextToDoBuilder implements ToDoBuilder {
    private TextBuffer buffer = new TextBuffer();

    @Override
    public void buildToDo(LocalDate date, String... list) {
        buffer.addLine("- " + date.format(DateTimeFormatter.ISO_DATE));

        for (String item : list) {
            buffer.addLine("  - " + item);
        }
    }

    @Override
    public Buffer getToDo() {
        return buffer;
    }
}

class JsonToDoBuilder implements ToDoBuilder {
    private JsonBuffer buffer = new JsonBuffer();

    @Override
    public void buildToDo(LocalDate date, String... list) {
        buffer.add(date.format(DateTimeFormatter.ISO_DATE), list);
    }

    @Override
    public Buffer getToDo() {
        return buffer;
    }
}
```

To-Do 빌더는 `Builder`에 해당하는 `ToDoBuilder`와 `ConcreteBuilder`에 해당하는 `TextToDoBuilder`, `JsonToDoBuilder`로 구성된다.

`TextToDoBuilder`와 `JsonToDoBuilder`는 To-Do를 생성하기 위한 정보를 받아, 텍스트 포맷과 JSON 포맷으로 구성하는 작업을 수행한다.

### To-Do 디렉터 - Director

``` java
class ToDoDirector {
    private ToDoBuilder builder;

    public ToDoDirector(ToDoBuilder builder) {
        this.builder = builder;
    }

    public Buffer construct() {
        builder.buildToDo(LocalDate.of(2022, 1, 1), "시골 내려가기", "세배하기");
        builder.buildToDo(LocalDate.of(2022, 1, 2), "떡국 먹기");

        return builder.getToDo();
    }
}
```

To-Do 디렉터는 앞서 정의한 `ToDoBuilder`를 이용하여 To-Do 목록을 작성한 후, 포맷에 맞게 변환된 결과를 반환하도록 구현하였다.

### 버퍼 - Product

``` java
interface Buffer {
    String convertToString();
}

class TextBuffer implements Buffer {
    private StringBuffer buffer = new StringBuffer();

    public void addLine(String string) {
        buffer.append(string + '\n');
    }

    @Override
    public String convertToString() {
        return buffer.toString();
    }
}

class JsonBuffer implements Buffer {
    private List<Json> buffer = new ArrayList<>();

    public void add(String key, String... values) {
        buffer.add(new Json(key, values));
    }

    @Override
    public String convertToString() {
        StringBuffer output = new StringBuffer();

        output.append("{\n");

        for (int index = 0; index < buffer.size(); index++) {
            output.append(buffer.get(index).toString());

            if (index < buffer.size() - 1) {
                output.append(",\n");
            }
        }

        output.append("\n}");

        return output.toString();
    }

    private class Json {
        public String       key;
        public List<String> values;

        public Json(String key, String... values) {
            this.key    = key;
            this.values = Arrays.asList(values);
        }

        @Override
        public String toString() {
            StringBuffer valueString = new StringBuffer();

            if (values.size() == 1) {
                valueString.append('"' + values.get(0) + '"');
            }
            else {
                valueString.append('[');

                for (int index = 0; index < values.size(); index++) {
                    valueString.append('"' + values.get(index) + '"');

                    if (index < values.size() - 1) {
                        valueString.append(", ");
                    }
                }

                valueString.append(']');
            }

            return "  \"" + key + "\": " + valueString;
        }
    }
}
```

일반적으로 구체 빌더가 생성하는 구성요소마다 표현되는 방식이 매우 다르기 때문에, 구성요소를 위한 공통 부모 클래스를 정의하는 것이 어렵다. 또한 클라이언트는 사용하려는 구체 빌더가 어떤 구성요소를 다룰 수 있는지 잘 알고 있는 상태이기 때문에, 보통 필요한 구체 클래스(Concrete Class)를 명시적으로 이용한다.

하지만 해당 예제에서는 동일한 To-Do 시나리오를 이용하여 텍스트 포맷과 JSON 포맷 결과를 한꺼번에 생성하기 위해 `Buffer`라는 공통 인터페이스를 정의하였다. `Buffer` 인터페이스는 단순히 최종 결과물을 문자열 형태로 변환할 수 있는 메서드만을 정의하고 있으며, `TextBuffer`와 `JsonBuffer`는 이를 상속받아 자신의 포맷에 맞게 데이터를 관리하는 역할을 수행한다.

### 메인 함수

``` java
class Main {
    public static void main(String[] args) {
        ToDoDirector textToDoDirector = new ToDoDirector(new TextToDoBuilder());
        System.out.println(textToDoDirector.construct().convertToString());

        ToDoDirector jsonToDoDirector = new ToDoDirector(new JsonToDoBuilder());
        System.out.println(jsonToDoDirector.construct().convertToString());
    }
}
```

``` bash
[출력]
- 2022-01-01
  - 시골 내려가기
  - 세배하기
- 2022-01-02
  - 떡국 먹기

{
  "2022-01-01": ["시골 내려가기", "세배하기"],
  "2022-01-02": "떡국 먹기"
}
```

구체 빌더에 따라 서로 다른 포맷의 결과물이 생성되는지 테스트하기 위해, 위와 같은 메인 함수를 작성하였다.

## 결론

- 구성요소의 내부 표현을 다양화 가능
    - 구성요소는 `Builder` 객체가 제공하는 추상 인터페이스를 통해서만 생성되기 때문에, 내부 표현을 변경하려면 새로운 종류의 빌더를 정의하기만 하면 된다.

- 생성과 표현을 위한 코드를 분리
    - 빌더 패턴은 복잡한 객체를 구성하고 표현하는 방법을 캡슐화하기 때문에 모듈성을 향상시킬 수 있다.
    - 클라이언트는 구성요소의 내부 구조를 정의하기 위해 어떤 클래스가 이용되는지 알 필요가 없어진다.
    - 동일한 구체 빌더를 이용하여 서로 다른 디렉터에서 다양한 구성요소를 생성할 수 있다.

- 생성 절차를 세밀하게 통제 가능
    - 한 번에 구성요소를 만들어내는 다른 생성 패턴들과 달리, 빌더 패턴은 디렉터의 통제에 따라 단계별로 구성요소를 생성하기 때문에 생성 절차를 세밀하게 통제할 수 있다.

## 참고 자료

- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides, 「Design Patterns: Elements of Reusable Object-Oriented Software」, Addison-Wesley Professional, 1994