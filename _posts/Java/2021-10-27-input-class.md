---
title: "[Java] 입력 클래스"
categories: Java
last_modified_at: 2021-10-27
---

자바의 입력 클래스에 대한 정리
{:.notice--primary}

## System.in

- 입력된 값을 바이트 데이터로 바꾸어 반환하는 표준 입력 스트림 객체
- 바이트 데이터를 반환하기 때문에, 입력된 값을 손쉽게 사용하기 위해선 고수준의 스트림 클래스 사용 필요

## Scanner

- 입력된 값을 공백 문자로 구분되는 토큰 단위로 읽는 클래스
- 공백 문자에는 ' ', '\t', '\n', '\f'가 존재
- 값을 원하는 자료형으로 손쉽게 읽기 가능

### 사용법

``` java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int    x = scanner.nextInt();
        double y = scanner.nextDouble();
        String z = scanner.next();

        System.out.println(x);
        System.out.println(y);
        System.out.println(z);

        scanner.close();
    }
}
```

``` bash
[입력]
10
3.14
Hello, world!

[출력]
10
3.14
Hello,
```

### 핵심 메서드

| 메서드 | 설명 |
| --- | --- |
| boolean nextBoolean() | 다음 토큰을 boolean 자료형으로 반환 |
| byte nextByte() | 다음 토큰을 byte 자료형으로 반환 |
| short nextShort() | 다음 토큰을 short 자료형으로 반환 |
| int nextInt() | 다음 토큰을 int 자료형으로 반환 |
| long nextLong() | 다음 토큰을 long 자료형으로 반환 |
| float nextFloat() | 다음 토큰을 float 자료형으로 반환 |
| double nextDouble() | 다음 토큰을 double 자료형으로 반환 |
| String next() | 다음 토큰을 문자열로 반환 |
| String nextLine() | 한 줄을 읽어 문자열로 반환 |
| void close() | Scanner 닫기 |

## BufferedReader

- Scanner의 경우 편리하지만 속도가 느리기 때문에, 일반적으로 입력을 많이 받아야 하는 경우엔 BufferedReader를 사용
- 많이 사용되는 readLine() 메서드를 이용할 경우 문자열을 반환하기 때문에, 별도로 추가적인 형변환이 필요

### 사용법

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        String   x = reader.readLine();
        String[] y = x.split(" ");

        System.out.println(x);

        for (int index = 0; index < y.length; index++) {
            System.out.println(y[index]);
        }

        reader.close();
    }
}
```

``` bash
[입력]
Hello, world!

[출력]
Hello, world!
Hello,
world!
```

### 주의 사항

``` java
public static void main(String[] args) throws IOException
```

위와 같이 BufferedReader를 사용하는 메서드에 IO에 대한 예외처리를 반드시 해주어야 에러가 발생하지 않는다.

### 핵심 메서드

| 메서드 | 설명 |
| --- | --- |
| int read() | 단일 문자를 반환 |
| String readLine() | 한 줄을 읽어 문자열로 반환 |
| void close() | 스트림을 닫고 사용된 시스템 자원을 해제 |