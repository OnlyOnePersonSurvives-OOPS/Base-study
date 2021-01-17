# Database2- MySQL(1/3)
## 데이터베이스의 목적
SpreadSheet, 관계형 DB **공통점: data를 '표'의 형태로 저장**해주는 공통점.  
관계형 **DB는 컴퓨터 언어를 통해서 제어**할 수 있다는 차이점이 있다.  

## MySQL의 구조
- 스프레드시트와 비슷한 형식의 '표' 이다.
- '폴더>파일'로 그룹화하는 것 처럼 '데이터베이스>표' 혹은 '스키마>표' 가 있다
- '데이터베이스(스키마)>표'를 저장하고 관리하는 것이 바로 '데이터베이스 서버' 이다
- - 'MySQL'은 스키마와 표를 관리하는 '데이터베이스 서버'이다.

### MySQL서버 접속
- 데이터베이스를 사용한다면
- - 파일보다 보안이 좋다 (자체 보안 체계를 가지고 있음)
- - 권한체계 (여러 계정 권한별로 만들고 관리할 수 있다)

- `./mysql -u[사용자] -p[패스워드]` 로 '데이터베이스 서버'에 접속할 수 있음 (ex. ./mysql -uroot -p111111)
- - '데이터베이스 서버'에 접속했다면 이제 '데이터베이스(스키마)'를 마주한 것이다!!

### MySQL 스키마(schema)의 사용
- 서버에 접속했다면 `mysql>`이라는 cmd가 뜬다!
- [mysql create database]라고 검색해보자
- - `CREATE DATABASE 데이터베이스이름;` 이라고 하면 된단다
- [mysql delete database]라고 검색해보자
- - `DROP DATABASE 데이터베이스이름;`이라고 하면 된단다
- [How to show database list in mysql]라고 하면
- - `SHOW { DATABASES | SCHEMAS }` 이라고 하면 된대

- `USE 데이터베이스이름`
- - 지금부터의 모든 명령은 해당 데이터베이스 내부 표에 대해서 작용할거다

### SQL과 테이블 구조
- 드디어 '표'를 다룰 준비가 되었다...!
- SQL은 일종의 '언어'이다
- - Structured Query Language
- - - Structured: 표로 정리정돈 해서 '구조화' 한당
- - - Query: 데이터베이스에게 CRUD를 '요청'하는 행위를 의미
- - - Language: 사용자와 서버 모두 이용할 수 있는 공통 '언어'
- - SQL은 ㅈㄴ 쉽다
- - SQL은 ㅈㄴ 중요하다 - 모오오오든 관계형DB에서 쓰이는 언어다
- Table (표)
- - row, record, 행
- - column, 열

### 테이블의 생성
- 표를 만드는 방법을 아는 방법
- - 참고로 '데이터베이스(스키마)' 에 들어가서 (USE사용) 만들어야 되는거임 알았지?
- - [create table in mysql]이라 검색하면 다 나옴 -> `CREATE TABLE` syntax
- - [create table in mysql **cheat sheet**]이라 검색하면 정리정돈된 자료가 많이 나올거임 (여러 분야에서 활용 가능)
- column별로 이름, 타입, 옵션을 설정해줘야 함
- `CREATE TABLE 표이름` -> 표 만드는 키워드
- 각 column당 데이터가 저장되는 '형식을 강제'할 수 있다 / [mysql data type number]라고 검색해보자

- id 열을 만들때는
- - 뒤에 `NOT NULL`이라고 쓰면 '값이 없음을 허용하지 않는다'라는 의미,
- - 뒤에 `AUTO_INCREMENT`라고 하면 자동으로 값이 1씩 증가하게 됨
- - `id INT(11) NOT NULL AUTO_INCREMENT`

-  title 열을 만들 때는
- - `title VARCHAR(100) NOT NULL`

- description 열을 만들 때는
- - `NULL`은 '값이 없는 것을 허용한다' 라는 의미
- - `description TEXT NULL`

- Primary Key
- - '성능'과 '중복'
- - 가장 중요한 column, 식별자가 되는 column 을 설정하는 것
- - 여기선 'id'를 식별자로 하려고 했으니 `PRIMARY KEY(id)` 
~~~sql
CREATE TABLE topic ( //엔터 치기전에;를 입력하지 않으면 줄바꿈이 됨
    id INT(11) NOT NULL AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    description TEXT NULL,
    created DATETIME NOT NULL,
    author VARCHAR(30) NULL,
    profile VARCHAR(100) NULL,
    PRIMARY KEY(id)
);
~~~

## MySQL의 CRUD
- 다시 말하지만 Create / Read / Update / Delete
- - 이 중 가장 중요한 것은 **Create**
- - 그 다음 중요한 것은 **Read**
- `USE`를 이용해 데이터베이스에 접근해서 하는거다잉
- `SHOW TABLES`하면 테이블 리스트를 볼 수 있음
- `DESC 표이름` 하면 해당 표를 볼 수 있음
### SQL의 INSERT 구문
- 데이터를 CREATE하는 구문
- [mysql create row]라고 검색해보자
- `INSERT INTO 표이름 (column이름1, 이름2, 이름3, ...) VALUES(해당 열의 내용1, 내용2, 내용3, ...)`
- - 시간의 경우 VALUES로 `NOW( )`를 사용할 수 있음
### SQL의 SELECT 구문 (사실 이게 제일 중요함)
- 읽는(READ) 방법
- [mysql select syntax]
- 모든 데이터 출력
- - `SELECT \* FROM 표이름`
- 해당 column출력
- - `SELECT column이름1,이름2,이름3, ... FROM 표 이름`
- 해당 column의 값이 X인 row만 출력
- - `SELECT column이름1,이름2,이름3, ... FROM 표이름 WHERE column이름='X'`
- 해당 column에 대해 내림차순 정렬되어 출력
- - `SELECT column이름1,이름2,이름3, ... FROM 표이름 ORDER BY column이름 DESC`
- 2개만 출력
- - `SELECT column이름1,이름2,이름3, ... FROM 표이름 LIMIT 2`
### SQL의 UPDATE 구문
- 검색해보는걸 추천
- WHERE문을 빠뜨리지만 말자 제발
### SQL의 DELETE 구문
- 검색해보는걸 추천
- WHERE문을 빠뜨리지만 말자 제발 너도

---
# Database2- MySQL (2/3)
## 관계형 데이터베이스( Relational Database )
### 혁신과 본질
- RDB(Relational Database)의 '혁신'과 '본질'
- 혁신 -> Relational / 본질 -> Database
### 관계형 데이터베이스의 필요성
> 저장은 분산, 출력은 합쳐서  
- 데이터의 중복 -> 무엇인가 잘못됐다 / 개선할 것이 필요하다
- 아래 topic표에서 두 개의 egoing이 동일인물임을 확신할 수 있는가?   

topic>  

| id | title | desc | author | profile |  
|---|:---:|:---:|:---:|:---:|---:|  
| 1 | a | a is ... | egoing  | developer |  
|2   |b   | b is ...  | egoing  | developer |  
|3  |c   | c is ...  |abba  | developer, CEO |  
  
- 데이터를 분산해서 저장해보자  

author>  

| id  | name | profile |  
|---|---|---|  
|1 | egoing  | developer |  
|2 | abba  | developer, CEO |  
|3 | egoing  | developer |  

topic>   

| id  | title | desc | author_id |  
|---|---|---|---|  
|1   |a   | blah blah ...  |1 |  
|2   |b   | blah blah ...  |1 |  
|3  |c   | blah blah ...  |2 |   
-> author의 수정, 구분에 더 장점이 생겼다  
-> 직관성이 조금 떨어졌다 (trade-off)  
- 하지만 이를 '합쳐서'보여줄 수 있다면, 데이터의 관리상에 많은 이점이 있을 것이다
- - `JOIN`에 관해서는 아래에서 살펴보자

### 테이블 분리하기
- `JOIN`을 위한 실습 자료
~~~sql
--
-- Table structure for table `author`
--
 
 
CREATE TABLE `author` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `profile` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
 
--
-- Dumping data for table `author`
--
 
INSERT INTO `author` VALUES (1,'egoing','developer');
INSERT INTO `author` VALUES (2,'duru','database administrator');
INSERT INTO `author` VALUES (3,'taeho','data scientist, developer');
 
--
-- Table structure for table `topic`
--
 
CREATE TABLE `topic` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(30) NOT NULL,
  `description` text,
  `created` datetime NOT NULL,
  `author_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
 
--
-- Dumping data for table `topic`
--
 
INSERT INTO `topic` VALUES (1,'MySQL','MySQL is...','2018-01-01 12:10:11',1);
INSERT INTO `topic` VALUES (2,'Oracle','Oracle is ...','2018-01-03 13:01:10',1);
INSERT INTO `topic` VALUES (3,'SQL Server','SQL Server is ...','2018-01-20 11:01:10',2);
INSERT INTO `topic` VALUES (4,'PostgreSQL','PostgreSQL is ...','2018-01-23 01:03:03',3);
INSERT INTO `topic` VALUES (5,'MongoDB','MongoDB is ...','2018-01-30 12:31:03',1);
~~~

### RDB의 꽃 `JOIN`
> RDB를 RDB답게 해주는 키워드  
- table을 합쳐서 출력하는 방법
- 일단 말로 설명해보자
- - topic표에 있는 author_id열의 값을 id열로 가지고 있는 author표에서, 해당 행을 가져와서 topic표에 붙여줘
- - `SELECT \* FROM topic LEFT **JOIN author**` 
- - ->error: 뭘 기준으로 결합하라는거야..?
- - `SELECT \* FROM topic LEFT JOIN author **ON topic.author_id = author.id**` //topic의 author_id와 author의 id를 이어서 붙여줘
- - -> 근데, 진짜 다 이어 붙임. 우리는 topic의 author_id열과 author의 id열은 출력할 필요가 없음
- - `SELECT **id**,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id`
- - >error: id가 topic의 id를 말하는거야, author의 id를 말하는거야
- - `SELECT **topic.id**,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id`
- - -> 오오 좋아, 근데 출력하고 보니깐, id가 topic의 id열인지 author의 id열인지 헷갈려
- -  `SELECT **topic.id AS topic_id**,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id`
- - -> 오 이제 topic_id라고 출력되는구나! 좋아!!


---
# Database2- MySQL (3/3)
## 인터넷과 데이터베이스
> Internet과 Database의 관계  
- MySQL의 **Database Server**가 왜 server일까?
- Internet: 두 개 이상의 컴퓨터 사이에 구성된 통신 시스템
- database의 클라이언트: 데이터를 요청하는 거
- - 우리가 실습했던 MySQL은 사실 'MySQL Monitor'라는 'database 클라이언트'이다
- database의 서버: 데이터를 실제로 가지고 있는 것
- - MySQL Monitor가 요청을 보낼때 이를 처리하고 응답하는 주체

## MySQL클라이언트
- 여러 종류가 있음
- 그 중 GUI기능이 있는게 **MySQL-workbench**


| 값 | 의미 | 기본값 |
|---|:---:|---:|
| `static` | 유형(기준) 없음 / 배치 불가능 | `static` |
| `relative` | 요소 자신을 기준으로 배치 |  |
| `absolute` | 위치 상 부모(조상)요소를 기준으로 배치 |  |
| `fixed` | 브라우저 창을 기준으로 배치 |  |