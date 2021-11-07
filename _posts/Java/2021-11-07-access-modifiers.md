---
title: "[Java] 접근 지정자"

categories: Java

date: 2021-11-07
last_modified_at: 2021-11-07
---

자바의 접근 지정자에 대한 정리
{:.notice--primary}

## 클래스 접근 지정자

| 관계 | public | default |
| --- | :---: | :---: |
| 같은 패키지의 클래스 | O | O |
| 다른 패키지의 클래스 | O | X |

public 클래스의 경우 반드시 클래스명이 자바 파일명과 동일해야 하며, 하나의 자바 파일에는 하나의 public 클래스만 선언할 수 있다.

## 멤버 접근 지정자

| 관계 | public | protected | default | private |
| --- | :---: | :---: | :---: | :---: |
| 같은 패키지의 클래스 | O | O | O | X |
| 다른 패키지의 클래스 | O | X | X | X |
| 같은 패키지의 서브 클래스 | O | O | O | X |
| 다른 패키지의 서브 클래스 | O | O | X | X |