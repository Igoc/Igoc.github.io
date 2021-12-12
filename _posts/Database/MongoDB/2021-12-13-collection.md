---
title: "[MongoDB] Collection"

categories: Database

date: 2021-12-13
last_modified_at: 2021-12-13
---

MongoDB의 Collection에 대한 정리
{:.notice--primary}

## Collection

[![Collection](https://docs.mongodb.com/manual/images/crud-annotated-collection.bakedsvg.svg)](https://docs.mongodb.com/manual/core/databases-and-collections/)

- MongoDB의 Document가 저장되는 공간
- 관계형 데이터베이스의 테이블과 유사

## Collection 생성

- 데이터베이스와 마찬가지로 Collection에 최초로 데이터를 저장하는 시점에 자동으로 Collection 생성
- Collection 이름에는 몇 가지 [제약 사항](https://docs.mongodb.com/manual/reference/limits/#std-label-restrictions-on-db-names)이 존재

### 사용법

``` bash
> db.newCollection1.insertOne({ x: 1 })
{
	"acknowledged" : true,
	"insertedId" : ObjectId("61b643d875822efbc1d2bdb5")
}

> db.newCollection2.createIndex({ y: 1 })
{
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"createdCollectionAutomatically" : true,
	"ok" : 1
}
```

위의 예제에서는 [insertOne()](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/)과 [createIndex()](https://docs.mongodb.com/manual/reference/method/db.collection.createIndex/) 연산자를 사용하여, Collection에 해당하는 newCollection1과 newCollection2를 생성한다.

## 명시적 Collection 생성

- [createCollection()](https://docs.mongodb.com/manual/reference/method/db.createCollection/) 메서드를 통해 명시적으로 Collection 생성 가능
- Collection의 최대 크기나 Document 유효 규칙과 같은 다양한 옵션 설정 가능

### 사용법

``` bash
> db.createCollection("newCollection3", {
	capped: true,
	size: 4096,
	max: 10000
})
{ "ok" : 1 }
```

위의 예제에서는 최대 크기가 4096바이트이고, 최대 Document 개수가 10000개인 newCollection3를 명시적으로 생성하고 있다.

## Collection 삭제

- [drop()](https://docs.mongodb.com/manual/reference/method/db.collection.drop/) 메서드를 통해 Collection 삭제 가능

### 사용법

``` bash
> db.newCollection3.drop()
true
```

## Collection 목록 출력

- show collections 명령어를 통해 Collection 목록 출력 가능

### 사용법

``` bash
> show collections
newCollection1
newCollection2
```