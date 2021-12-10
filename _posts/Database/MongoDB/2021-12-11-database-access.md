---
title: "[MongoDB] 데이터베이스 접근 방법"

categories: Database

date: 2021-12-11
last_modified_at: 2021-12-11
---

MongoDB의 데이터베이스 접근 방법에 대한 정리
{:.notice--primary}

## MongoDB 실행 및 종료

- MongoDB가 환경 변수에 등록되어 있고 서비스가 실행 중인 상태라면, mongo 명령어를 통해 MongoDB 실행 가능
- MongoDB 내장 함수인 quit()를 이용하여 MongoDB 종료 가능

## 데이터베이스 전환

- use 쿼리를 통해 사용 중인 데이터베이스 전환 가능
- MongoDB에서는 전환 전에 새로운 데이터베이스를 만들 필요가 없으며, 만들려는 데이터베이스에 처음으로 데이터를 삽입할 때 자동으로 생성

### 사용법

``` bash
> use practice
switched to db practice
```

## 사용 중인 데이터베이스 이름 출력

- db 쿼리를 통해 사용 중인 데이터베이스 이름 출력 가능
- 아무런 설정을 하지 않았을 경우, 기본 데이터베이스에 해당하는 test 출력

### 사용법

``` bash
> db
test
```