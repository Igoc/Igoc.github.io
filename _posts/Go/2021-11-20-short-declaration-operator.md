---
title: "[Go] 짧은 변수 선언 연산자 (Short Declaration Operator)"

categories: Go

date: 2021-11-20
last_modified_at: 2021-11-20
---

Go 언어의 짧은 변수 선언 연산자에 대한 정리
{:.notice--primary}

## := 연산자

- 변수 선언 키워드인 var과 자료형을 생략하고 변수를 선언하는 연산자
- 함수 내에서만 사용할 수 있으며, 함수 밖에서는 var 키워드를 필수적으로 사용

### 사용법

``` go
x := 1
y := 3.14
z := "String"
```