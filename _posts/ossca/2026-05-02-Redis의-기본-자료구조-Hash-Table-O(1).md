---
title: "[OSSCA] Redis의 기본 자료구조 Hash Table - O(1)"
date: 2026-05-02 16:27:51 +0900
categories: [CS, OSSCA]
tags: []
toc: true
math: false
mermaid: false
---

skiplist

시간 복잡도: log n

level은 Random하게 결정됨

단점: 배열을 쓰고 있어서 메모리를 많이 사용한다.

hash 자료구조

해시 테이블 내에 또 서브 해시 테이블을 가짐

### Redis is Single Threaded

프로세스 커멘드에서 패킷들을 확인하고 명령이 완성되면 바로 처리

싱글스레드라서 원자성을 유지할 수 있다. 이것이 redis의 중심가치이다.

valkey는 하나의 메인 스레드에서 분배하는 중앙집중형 작업 분배 방식을 사용하고

Redis는 분산형 이벤트 루프 네트워크 방식을 사용한다.

\*java netfy 보스 워크(?)랑 구조가 비슷하다.

### Redis 메모리를 아끼기 위한 노력

메모리에 있을 때는 좋은 자료구조가 아니라도 N이 작으면 속도가 빠르다.

예전엔 ziplist를 사용했지만 최근 listpack 자료구조를 사용한다.

ziplist는 이전, 이후의 데이터도 가지고 있었지만 listpack은 가지고 있지 않다.

10만개가 있을때는 hashtable 일때는 8.99M를 사용하지만 ziplist, listpack은 각각 약 2M를 사용한다.

### RESP (Redis Serialization Protocol

RESP v2, v3가 있는데 기본은 v2 사용, hello 3 사용시 개별로 v3 적용