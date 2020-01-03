---
layout: post
title:  "Redis Introduce"
date:   2020-01-03
desc: "django & celery & redis"
keywords: "Database,NoSql,Redis"
categories: [Database]
tags: [Database,NoSql,Redis]
icon: icon-redis
---

## Redis(Remote Directory System)

- 키밸류(KeyValue) 데이터베이스로 분류되는 NoSQL이며, 2009년 Salvatore Sanfilippo가 처음 개발

  2005년 부터 RedisLabs co.에 의해 상용 SW가 개발되어 지원되고 있다

- 키밸류 DB이면서 In Memory 기반의 데이터 처리 및 저장 기술을 제공하기 때문에 상대적으로 빠른 Read/Write가 가능

- String, Set, Sorted Set, Hash, List, HyperLog유형의 데이터를 저장가능

- Master/Slave Replication 기능을 통해 데이터 분산, 복제 기능을 제공하며 Query Off Loading기능을 통해 Master는 Read/Write를 수행하고 Slave는 Read만 수행할 수 있다

- 파티셔닝을 통해 동적인 스케일 아웃(Scale Out)인 수평 확장이 가능하다

- Expriation기능은 일정 시간이 지났을 때 메모리 상의 데이터를 자동 삭제 할 수 있다



## Redis 설치(Linux)

Download, extract and compile Redis with:

```
$ wget http://download.redis.io/releases/redis-5.0.7.tar.gz
$ tar xzf redis-5.0.7.tar.gz
$ cd redis-5.0.7
$ make
```

The binaries that are now compiled are available in the `src` directory. Run Redis with:

```
$ src/redis-server
```

You can interact with Redis using the built-in client:

```
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

## Redis 설치(Window)

- https://github.com/microsoftarchive/redis/releases 사이트에서 
- Redis-x64-3.2.100.msi Window용 다운로드 및 설치

![1575206453304](/static/assets/img/blog/database/1575206453304.png)

- Next 

![1575206533066](/static/assets/img/blog/database/1575206533066.png)

- 시스템 Path에 설정 체크박스 체크

![1575206560216](/static/assets/img/blog/database/1575206560216.png)

- 기본포트 6379, 윈도우즈 방화벽에 예외처리체크박스 체크

![1575206579880](/static/assets/img/blog/database/1575206579880.png)

- Redis 서버가 사용할 수 있는 최대 메모리 크기 설정

![1575206621038](/static/assets/img/blog/database/1575206621038.png)

- 설치가 완료되면 아래와 같은 파일을 확인 할 수 있다
  - redis.windows.conf 파라메터 파일
  - redis-server.exe 인스턴스 시작 실행 파일

![1575207046803](/static/assets/img/blog/database/1575207046803.png)

## Redis 시작과 종료(Linux)

- Redis Server 시작

```bash
$ cd /home/redis/src
$ ./redis-server redis_5000.conf
```

- Redis Client 실행 및 종료

```bash
$ cd /home/redis/src
$ ./redis-cli -p 5000 #port 5000으로 redis 서버 접속

#종료
120.0.0.1:5000 > shutdown
```



## Redis 시작과 종료(Window)

#### redis 서버 서비스로 등록

- 관리자 권한으로 Redis 폴더내에서 명령프롬프트(cmd.exe) 실행

```bash
#파일명변경
\path\to\Redis> move redis.windows-service.conf redis.conf
#Redis 서버를 시작하는 작업이 윈도우 서버가 시작될때 마다 자동으로 실행되도록 등록
\path\to\Redis> redis-server --service-install redis.conf --service-name redis6379
```

- 작업관리자 - 서비스텝에서 확인가능

![1575285205883](/static/assets/img/blog/database/1575285205883.png)

- 제어판 - 서비스 에서 확인가능

![1575285249180](/static/assets/img/blog/database/1575285249180.png)

#### redis-cli를 통해 Redis 서버 접속

```
\path\to\Redis> redis-cli -p 6379
```

![1575285920669](/static/assets/img/blog/database/1575285920669.png)

- 명령어 목록 https://redis.io/commands 참고

## Redis 데이터 처리

### 데이터 입력/수정/삭제/조회

- set : 데이터를 저장할 때(key, value)
- get : 저장된 데이터를 검색할 때
- rename : 저장된 데이터 값을 변경할 때
- rendomkey : 저장된 key 중에 하나의 key를 랜덤하게 검색할 때
- keys : 저장된 모든 key를 검색할 때
- exits : 검색 대상 key가 존재하는지 여부를 확인 할 때
- mset/ mget : 여러개의 key와 value를 한 번 저장하고 검색할 때
- del :  저장된 데이터 삭제

#### 실습

```cmd
#key:1111, value:KH PARK 데이터 저장
127.0.0.1:6379> set 1111 "KH PARK"
OK
127.0.0.1:6379> set 1112 "YH JOO"
OK
127.0.0.1:6379> set 1113 "KO HONG"
OK

#현재 저장되어 있는 모든 키 출력
127.0.0.1:6379> keys *
1) "name"
2) "1113"
3) "1112"
4) "1111"

#key:name 삭제
127.0.0.1:6379> del name
(integer) 1

127.0.0.1:6379> keys *
1) "1113"
2) "1112"
3) "1111"
#key중이 2로 끝나는 key만 검색
127.0.0.1:6379> keys *2
1) "1112"

#key:1113에서 key:1116으로 변경
127.0.0.1:6379> rename 1113 1116
OK
#현재 저장되어 있는 모든키 조회
127.0.0.1:6379> keys *
1) "1116"
2) "1112"
3) "1111"

#key:1116의 존재 여부 검색 (존재:1)
127.0.0.1:6379> exists 1116
(integer) 1
127.0.0.1:6379> exists 115
(integer) 0
#key:1111의  value  길이
127.0.0.1:6379> strlen 1111
(integer) 7
127.0.0.1:6379> keys 1111
1) "1111"
127.0.0.1:6379> get 1111
"KH PARK"
```



```cmd
127.0.0.1:6379> mset seq_no 201912001 #연속번호 발행을 위한 key/value저장
OK
127.0.0.1:6379> incr seq_no #incremental 증가값 +1
(integer) 201912002
127.0.0.1:6379> decr seq_no #decremental 감소값 -1
(integer) 201912001
127.0.0.1:6379> incrby seq_no 2 #incremental 증가값 +2
(integer) 201912003
127.0.0.1:6379> decrby seq_no 10 #decremental 감소값 -10
(integer) 201911993

127.0.0.1:6379> append 1115 " co."
(integer) 7
127.0.0.1:6379> keys *
1) "1116"
2) "seq_no"
3) "1115"
4) "1113"
5) "1111"
6) "1112"
127.0.0.1:6379> get 1115
"PIT co."
127.0.0.1:6379> save
OK
127.0.0.1:6379> flushall
OK
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> info
# Server
redis_version:3.2.100
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:dd26f1f93c5130ee
redis_mode:standalone
os:Windows
arch_bits:64
multiplexing_api:WinSock_IOCP
process_id:35680
run_id:f0aa3c8c632262d58b60021358b0264d22893fb8
tcp_port:6379
uptime_in_seconds:144385
uptime_in_days:1
hz:10
lru_clock:15070081
.....
127.0.0.1:6379> exit
```

- save 명령어에 의해 생성된 rdb 파일

![1575351489179](/static/assets/img/blog/database/1575351489179.png)



### 데이터 타입

- strings : 문자(text), Binary 유형 데이터를 저장
- List : 하나의 key에 여러 개의 배열 값을 저장
- Hash : 하나의 Key에 여러개의  Fields 와 Value로 구성
- Set Sorted set : 정렬되지 않은  String타입, Set과  Hash 를 결합한 타입
- Bitmaps : 0,1 로 표현하는 데이터 타입
-  HyperLogLogs :  Element  중에서  Unique 한 개수의  Element 만 계산
- Geospatial : 좌표 데이터를 저장 및 관리하는 데이터 타입

#### Hash 타입

-  Hash 타입은 기존 관계형 DB 에서   Primary-Key 와 하나 이상의 컬럼으로 구성된 테이블 구조와 매우 흡사한 데이터 유형
- 하나의 key는 오브젝트명과 하나 이상의 필드값을 콜론(:) 기호로 결합하여 표현할 수 있다 (예, order:201912031, order_detail:201912031:01)
- 문자값을 저장할 때는 인용부호("")를 사용, 숫자값을 저장할때는 인용부호 가 필요없다
- 기본적으로 필드 개수는 제한 없다
- 실습

```cmd
#Hash 타입은 하나의 key에 여러 개의 field와 value를 저장할 수 있음
127.0.0.1:6379> hmset order:2019120301 custome_name "Wonman & Sports" emp_name "Magee" total 601100 payment_type "Credit" order_filled "Y" ship_date 20191203
OK

#order:2019120301에 대한 필드값 검색 결과
127.0.0.1:6379> hget order:2019120301 custome_name
"Wonman & Sports"
127.0.0.1:6379> hmset order:2019120301 customer_name "Wonman & Sports" emp_name "Magee" total 601100 payment_type "Credit" order_filled "Y" ship_date 20191203
OK
127.0.0.1:6379> hget order:2019120301 customer_name
"Wonman & Sports"
127.0.0.1:6379> hget order:2019120301 ship_date
"20191203"

#order:2019120301 key에 대한 모든 필드와 value값 조회
127.0.0.1:6379> hgetall order:2019120301
 1) "custome_name"
 2) "Wonman & Sports"
 3) "emp_name"
 4) "Magee"
 5) "total"
 6) "601100"
 7) "payment_type"
 8) "Credit"
 9) "order_filled"
10) "Y"
11) "ship_date"
12) "20191203"
13) "customer_name"
14) "Wonman & Sports"

#custome_name 필드 제거
127.0.0.1:6379> hdel order:2019120301 custome_name
(integer) 1
127.0.0.1:6379> hgetall order:2019120301
 1) "emp_name"
 2) "Magee"
 3) "total"
 4) "601100"
 5) "payment_type"
 6) "Credit"
 7) "order_filled"
 8) "Y"
 9) "ship_date"
10) "20191203"
11) "customer_name"
12) "Wonman & Sports"

#product_name 필드 존재하는지 여부 확인
127.0.0.1:6379> hexists order:2019120301 product_name
(integer) 0
127.0.0.1:6379> hexists order:2019120301 customer_name
(integer) 1

#ship_date 필드 제거
127.0.0.1:6379> hdel order:2019120301 ship_date
(integer) 1
127.0.0.1:6379> hgetall order:2019120301
 1) "emp_name"
 2) "Magee"
 3) "total"
 4) "601100"
 5) "payment_type"
 6) "Credit"
 7) "order_filled"
 8) "Y"
 9) "customer_name"
10) "Wonman & Sports"

#ship_date 필드 다시 추가
127.0.0.1:6379> hmset order:2019120301 ship_date 20191203
OK
127.0.0.1:6379> hgetall order:2019120301
 1) "emp_name"
 2) "Magee"
 3) "total"
 4) "601100"
 5) "payment_type"
 6) "Credit"
 7) "order_filled"
 8) "Y"
 9) "customer_name"
10) "Wonman & Sports"
11) "ship_date"
12) "20191203"

#order:2019120301 키에 대한 모든 필드명 만 출력
127.0.0.1:6379> hkeys order:2019120301
1) "emp_name"
2) "total"
3) "payment_type"
4) "order_filled"
5) "customer_name"
6) "ship_date"

#order:2019120301 키에 대한 모든 value 만 출력
127.0.0.1:6379> hvals order:2019120301
1) "Magee"
2) "601100"
3) "Credit"
4) "Y"
5) "Wonman & Sports"
6) "20191203"

#필드명 개수 출력
127.0.0.1:6379> hlen order:2019120301
(integer) 6

#특정필드 value만 출력
127.0.0.1:6379> hmget order:2019120301 emp_name total
1) "Magee"
2) "601100"
```

#### List 타입

- 배열(Array) 변수와 유사한 데이터 구조
- 기본적으로 String 타입의 경우 배열에 저장할 수 있는 데이터 크기 512MB
- List 타입의 데이터 처리할 때 lpush, lrange, rpush, rpop, llen, lindex 명령어를 사용

#### Set 타입

- List타입은 하나의 필드에 여러개의 배열(Array)값을 저장할 수 있는 데이터 구조라면 Set타입은 배열구조가 아닌 여러개의 엘리먼트(Element)로 데이터 값을 표현하는 구조
- Set 타입의 데이터 처리할 때는 sadd, smembers, scard, sdiff, sunion명령어를 사용

#### Sorted Set 타입

- Sorted Set 타입은 Set타입과 동일한 데이터 구조이며 차이점은 저장된 데이터 값이 분류(Sorting)된 상태이면 Sorted Set 타입이고 분류되지 않으면 Set타입
- 데이터 처리할때 zadd, zrange, zcard, zcount, zrank ,zrevrank 명령어 사용함

####  Bit 타입

- 일반적으로 사용자가 표현하는 데이터는 문자,숫자,날짜인데 이를 Ascii값이라고 표현하는데 컴퓨터는 이를 최종적으로 0과 1로 표현되는 Bit값으로 변환하여 저장한다. Redis에서 제공되는 Bit타입은 사용자의 데이터를 0과 1로 표현하며 컴퓨터가 가장 빠르게 저장할 수 있고 해석할 수 있도록 표현하는 구조
- 데이터 처리할때는 setbit, getbit, bitcount 명령어를 사용함

#### Geo 타입

#### HyperLogLogs 타입



## Redis 확장 Module

- REJSON : JSON 데이터 타입을 이용하여 데이터를 처리할 수 있는 모듈
- RediSQL : Redis Server에서 SQLite(관계형DB)로 데이터를 저장할 수 있는 모듈
- RediSearch : Redis DB내에 저장된 데이터에 대한 검색엔진을 사용할 수 있는 모듈
- Redis-ML : Machine Learing Model Server를 Redis 서버에서 사용할 수 있는 모듈
- Redis-sPiped : Redis Server로 전송되는 데이터를 암호화할 수 있는 모듈

