---
title: "제어문"

categories: [Programming-Language, Java]
tags: [Java, Control Statement]

date: 2022-03-16T21:00:00+09:00
last_modified_at: 2022-03-16T21:00:00+09:00
---

자바 제어문에 대한 정리
{:.notice--primary}

## 조건문

조건문은 조건의 결과에 따라 프로그램 실행 흐름을 분기하기 위해 사용된다.

### if-else 문

- `if` 조건식이 참이라면 `if` 뒤의 명령문을 실행하고, 거짓이라면 `else` 뒤의 명령문을 실행한다.
- `if` 뒤에 오는 명령문이 여러 개라면 중괄호로 감싸 블록으로 만들어야 하지만, 하나의 명령문만 오는 경우 생략해도 무관하다.
- `else if`의 경우 `else` 다음에 오는 문장이 `if` 하나라서 중괄호가 생략된 구조이다.

``` java
int age = 20;

if (age < 10)
    System.out.println("10살 미만");
else if (age < 20) {
    System.out.println("20살 미만");
}
else {
    if (age == 20) {
        System.out.println("20살");
    }
    else if (age == 21) {
        System.out.println("21살");
    }
    else {
        System.out.println("22살 이상");
    }
}
```

``` bash
[출력]
20살
```

### switch 문

- `switch` 뒤에 오는 변수의 값에 따라 실행을 분기한다.
- 변수의 값과 일치하는 `case` 뒤에 오는 모든 명령문을 실행하며, 이로 인해 `break`를 사용하지 않으면 다음 `case`도 수행한다.
- 일치하는 `case`가 없을 경우 `default` 뒤에 오는 명령문을 실행한다.
- 자바 7부터 정수 자료형 변수뿐만 아니라, `String` 자료형 변수도 사용할 수 있다.

``` java
int dayOfWeek = 0;

switch (dayOfWeek) {
    case 0:
        System.out.println("월요일");

    case 1:
        System.out.println("화요일");

    case 2:
        System.out.println("수요일");
        break;

    case 3:
        System.out.println("목요일");
        break;

    case 4:
        System.out.println("금요일");
        break;

    case 5:
        System.out.println("토요일");
        break;

    case 6:
        System.out.println("일요일");
        break;

    default:
        System.out.println("오류");
}
```

``` bash
[출력]
월요일
화요일
수요일
```

## 반복문

반복문은 특정 명령문을 여러 차례 반복하여 실행하기 위해 사용된다.

### for 문

- 초기식, 조건식, 증감식 구조로 구성되며, 조건식을 만족하는 동안 뒤에 오는 명령문을 반복하여 실행한다.
- 초기식, 조건식, 증감식은 `;` 기호를 통해 구분되며, 불필요한 식은 생략하는 것이 가능하다.
- 조건식을 생략하게 될 경우 항상 참으로 인식하여 무한 루프를 돌게 된다.
- `for` 뒤에 오는 명령문이 여러 개라면 중괄호로 감싸 블록으로 만들어야 하지만, 하나의 명령문만 오는 경우 생략해도 무관하다.

``` java
for (int index = 0; index < 3; index++) {
    System.out.println(index);
}
```

``` bash
[출력]
0
1
2
```

### while 문

- 조건식이 참인 동안 뒤에 오는 명령문을 반복하여 실행한다.
- for 문과 달리 조건식을 생략할 수 없다.
- `while` 뒤에 오는 명령문이 여러 개라면 중괄호로 감싸 블록으로 만들어야 하지만, 하나의 명령문만 오는 경우 생략해도 무관하다.

``` java
int index = 0;

while (index < 3) {
    System.out.println(index);
    index++;
}
```

``` bash
[출력]
0
1
2
```

### do-while 문

- while 문처럼 조건식이 참인 동안 `do` 뒤에 오는 명령문을 반복하여 실행한다.
- 조건의 만족 여부과 상관없이 무조건 `do` 뒤에 오는 명령문을 1회 수행한다.
- while 문과 마찬가지로 조건식을 생략할 수 없다.
- `do` 뒤에 오는 명령문이 여러 개라면 중괄호로 감싸 블록으로 만들어야 하지만, 하나의 명령문만 오는 경우 생략해도 무관하다.

``` java
int index = 3;

do {
    System.out.println(index);
    index++;
} while(index < 3);
```

``` bash
[출력]
3
```

### for-each 문

- 배열 또는 `java.lang.Iterable`을 상속받은 클래스의 요소를 순차적으로 하나씩 순회한다.
- for 문처럼 생겼지만 요소가 담길 변수와 순회할 변수로 구성되며, 두 변수는 `:` 기호를 통해 구분된다.
- `for` 뒤에 오는 명령문이 여러 개라면 중괄호로 감싸 블록으로 만들어야 하지만, 하나의 명령문만 오는 경우 생략해도 무관하다.

``` java
int[] array = { 1, 2, 3 };

for (int x : array) {
    System.out.println(x);
}
```

``` bash
[출력]
1
2
3
```

### 반복문 제어

반복문의 뒤에 오는 명령문을 수행하는 도중 반복을 제어하기 위해 사용된다.

#### continue

명령문 수행 도중 `continue`를 만나면, 뒤에 오는 명령문을 건너뛰고 다음 반복을 수행한다.

``` java
for (int index = 0; index < 5; index++) {
    if (index == 2) {
        continue;
    }

    System.out.println(index);
}
```

``` bash
[출력]
0
1
3
4
```

#### break

명령문 수행 도중 `break`를 만나면, 뒤에 오는 명령문을 건너뛰고 반복을 즉시 종료한다.

``` java
for (int index = 0; index < 5; index++) {
    if (index == 2) {
        break;
    }

    System.out.println(index);
}
```

``` bash
[출력]
0
1
```