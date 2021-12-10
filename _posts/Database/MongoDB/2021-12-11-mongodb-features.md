---
title: "[MongoDB] MongoDB의 특징"

categories: Database

date: 2021-12-11
last_modified_at: 2021-12-11
---

MongoDB의 특징에 대한 정리
{:.notice--primary}

## 주요 특징

### Document 데이터베이스

[![Document 데이터베이스](https://docs.mongodb.com/manual/images/crud-annotated-document.bakedsvg.svg)](https://docs.mongodb.com/manual/introduction/)

- JSON, XML 등과 같이 구조화된 Document를 레코드로 갖는 데이터베이스
- MongoDB는 레코드를 JSON 형식과 유사하게 Field-Value 쌍으로 지정
- Field의 값으로는 일반적인 값뿐만 아니라, Document, 배열, Document 배열 또한 지정 가능
- MongoDB에서는 이러한 Document를 관계형 데이터베이스의 테이블과 유사한 Collections에 저장

### 고성능

- 임베디드된 데이터 모델을 지원하여 데이터베이스 시스템에 대한 I/O 연산 감소
- 인덱스를 통해 고속의 쿼리 연산 제공

### 고가용성

- [Replica Set](https://docs.mongodb.com/manual/replication/)이라고 불리는 복제 시스템을 통한 고가용성 제공
- 해당 시스템을 통해 자동 장애 극복 및 데이터 복제 기능 제공

### 수평적 확장성

- [Sharding](https://docs.mongodb.com/manual/sharding/)을 이용하여 데이터 분산 기능 제공