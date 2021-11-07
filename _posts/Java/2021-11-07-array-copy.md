---
title: "[Java] 배열을 복사하는 방법"

categories: Java

date: 2021-11-07
last_modified_at: 2021-11-07
---

자바의 배열을 복사하는 방법에 대한 정리
{:.notice--primary}

## 얕은 복사 (Shallow Copy)

- 배열의 레퍼런스만 복사
- 배열의 실제 데이터 공간을 공유하기 때문에 낮은 복사 비용
- 원본 배열이나 복사된 배열의 값이 변경될 때, 변경을 하지 않은 배열의 값도 동시에 변경

### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3 };
        int[] b = a;

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));

        a[0] = 5;
        b[2] = 10;

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3]
b: [1, 2, 3]
a: [5, 2, 10]
b: [5, 2, 10]
```

## 깊은 복사 (Deep Copy)

- 배열의 요소 각각의 값을 복사
- 모든 요소를 복사하기 때문에 높은 복사 비용
- 원본 배열이나 복사된 배열의 값이 변경될 때, 변경을 하지 않은 배열의 값은 유지

### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3 };
        int[] b = new int[a.length];

        for (int index = 0; index < a.length; index++) {
            b[index] = a[index];
        }

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));

        a[0] = 5;
        b[2] = 10;

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3]
b: [1, 2, 3]
a: [5, 2, 3]
b: [1, 2, 10]
```

## 깊은 복사를 위한 여러가지 메서드

- 자바에서는 깊은 복사를 위한 여러가지 메서드를 제공
- clone(), Arrays.copyOf(), Arrays.copyOfRange(), System.arraycopy() 등이 존재

### clone()

- 깊은 복사를 위한 가장 보편적인 방법
- 배열의 모든 요소를 복사

#### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3 };
        int[] b = a.clone();

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));

        a[0] = 5;
        b[2] = 10;

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3]
b: [1, 2, 3]
a: [5, 2, 3]
b: [1, 2, 10]
```

### Arrays.copyOf()

- 배열의 시작 요소부터 원하는 길이만큼 배열을 복사

#### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3, 4, 5 };
        int[] b = Arrays.copyOf(a, 3);

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3, 4, 5]
b: [1, 2, 3]
```

### Arrays.copyOfRange()

- 시작 인덱스와 마지막 인덱스를 주어 해당 구간만큼 배열을 복사
- 마지막 인덱스는 복사에서 제외

#### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3, 4, 5 };
        int[] b = Arrays.copyOfRange(a, 1, 3);

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3, 4, 5]
b: [2, 3]
```

### System.arraycopy()

- 소스 배열의 특정 구간을 대상 배열의 원하는 구간에 복사
- 소스 배열과 대상 배열의 시작 인덱스는 각각 지정
- 소스 배열과 대상 배열의 구간 길이는 공유

#### 함수 원형

``` java
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
```

| 파라미터 | 설명 |
| --- | --- |
| src | 소스 배열 |
| srcPos | 소스 배열의 시작 인덱스 |
| dest | 대상 배열 |
| destPos | 대상 배열의 시작 인덱스 |
| length | 복사할 배열의 길이 |

#### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = { 1, 2, 3, 4, 5 };
        int[] b = { 6, 7, 8, 9, 10 };

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));

        System.arraycopy(a, 1, b, 2, 3);

        System.out.println("a: " + Arrays.toString(a));
        System.out.println("b: " + Arrays.toString(b));
    }
}
```

``` bash
[출력]
a: [1, 2, 3, 4, 5]
b: [6, 7, 8, 9, 10]
a: [1, 2, 3, 4, 5]
b: [6, 7, 2, 3, 4]
```