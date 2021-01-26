# MySQL

## 0. 들어가기 전

파일  
입력 저장 출력에 어려움  
정리정돈해서 쓰고싶을 때 편하게 쓰고 싶은 욕구  

**관계형 데이터베이스**  
표 형태, 정렬, 검색

웹의 성장 -> 데이터베이스를 찾음 -> MySQL의 동반 성장

---

## 1. 데이터베이스의 목적

관계형 데이터 베이스 & spreadSheets
표의 형태로 나타낸다.  

데이터베이스 -> 코딩을 통해 제어할 수 있다. 말을 걸 수 있다.  
spreadsheet -> 클릭 클릭 클릭...해서 원하는 결과를 도출한다.  

데이터베이스에 있는 정보를 누구나 웹 사이트를 통해 볼 수 있다.  
유저가 글을 쓰면 이는 데이터베이스에 저장된다.  

---

## 2. MySQL 설치

MySQL community edition download  

설치하기 싫으면 codeanywhere  

**cmd**창에서 해야될 것 같다.

cd C:\Bitnami\wampstack-7.4.13-0\mysql\bin

mysql -uroot -p

---

## 3. MySQL의 구조
글들을 저장하는 표, 회원정보를 저장하는 표...   
많아진 표들을 정리정돈할 필요도 생길 것이다.  

database: 연관된 표를 그루핑한 것   (= **스키마**)  

데이터베이스 서버: 많아진 스키마를 또 묶어 놓는 곳  

MySQL을 설치한 것 -> 데이터베이스를 설치하고 제공해주는 그 기능들을 이용할 수 있는 것  

---

## 4. 서버 접속

-u root: root라는 user로 접속하겠다. root(관리자/모든권한)  
-p: password

---

## 5. 스키마(schema)의 사용

~~~
CREATE DATABASE name;
~~~

~~~
DROP DATABASE name;
~~~

~~~
SHOW DATABASES;
~~~

그 전에 MySQL에게 우리가 사용하고 싶은 db를 알려줘야함.  
~~~
USE name;
~~~

이후에 내리는 명령은 name에게 적용된다.  

---

## 6. SQL과 테이블의 구조

Structured  
Query  
Language  

정리정돈    
요청/질의  
약속  

**두 가지 특징**  
1. 쉽다.
2. 중요하다.


- table
- row, record (data 하나)
- column (data type)

---

## 7. 테이블의 생성

create table in mysql cheat sheet(커닝페이퍼)  

컬럼의 데이터타입을 강제할 수 있다.  
~~~
CREATE TABLE topic(
	id DATATYPE(N) NOT NULL AUTO_INCREMENT,
	title VARCHAR(100) NOT NULL,
	description TEXT NULL,
	created DATETIME NOT NULL,
	author VARCHAR(30) NULL,
	profile VARCHAR(100) NULL,
	PRIMARY KEY(id);
}
~~~

- 값이 있게 강제하거나 없어도 되게 허용할 수 있다.
- AUTO_INCREMENT: id컬럼이 자동으로 1씩 증가한다. (고유값, 중복되지 않는 식별값을 갖음)
- PRIMARY KEY(id): 1. 성능, 2. **중복 방지**

---

## 8. CRUD

**Create**
**Read**
Update
Delete

---

## 9. Insert

~~~
SHOW TABLES;

DESC topic;

INSERT INTO topic (title, description, created, author, profile) VALUES('MySQL', 'MySQL is... ', NOW(), 'geonho', 'developers');

**SELECT * FROM topic;**
~~~

---

## 10. Read

SELECT syntax
~~~
SELECT * FROM topic;
SELECT id, title, created, author FROM topic;
SELECT "egoing", 1+1;

SELECT SELECT * FROM topic WHERE author='egoing';

SELECT * FROM topic ORDER BY id DESC;

SELECT * FROM topic LIMIT 2;
~~~

---

## 11. Update

UPDATE syntax
~~~
UPDATE topic SET description='ORACLE is relational...', title='Oracle';
이렇게하면 모든 행들이 다 이렇게 바뀐다!!  
UPDATE topic SET description='ORACLE is relational...', title='Oracle' WHERE id=2;
~~~

---

## 12. DELETE

DELETE syntax
~~~
DELETE FROM topic WHERE id=5;
~~~

---

## 13. 관계형데이터베이스의 필요성

중복되는 데이터 -> 개선할 여지가 있는 중요한 신호  

tradeoff  
중복제거, 유지보수, <-> 직관성  

저장은 분산 보여줄 땐 합쳐서!  

JOIN문 이용  

~~~
SELECT*FROM topic LEFT JOIN ON author ON topic.author_id = author.id;
~~~

---

## 14. 테이블 분리하기

~~~
RENAME TABLE topic TO topic_backup;

CREATE TABLE topic(
	id INT(11) NOT NULL AUTO_INCREMENT;
	title VARCHAR(30) NOT NULL,
	description TEXT NULL,
	created DATETIME NOT NULL,
	author_id INT(11) NULL,
	PRIMARY KEY(id)
);

DESC topic;

CREATE TABLE author(
	id INT(11) NOT NULL AUTO_INCREMENT,
	name VARCHAR(20) NOT NULL,
	profile VARCHAR(200) NOT NULL,
	PRIMARY KEY(id)
);

INSERT INTO author (id, name, profile) VALUES(1, 'egoing', 'developer');

INSERT INTO topic (id, title, description, created, author_id) VALUES(1, 'MySQL', 'MySQL is...', '(copy)', 1);

INSERT INTO author (id, name, profile) VALUES(2, 'duru', 'data administrator');
~~~

---

# 15 JOIN

데이터베이스의 꽃
~~~
SELECT * FROM topic LEFT JOIN author;
-> 기준이 없어서 안된다.  
SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.id;

SELECT id, title, description, created, name, profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
-> 에러! 'id' is ambigous

SELECT topic.id AS topic_id, title, description, created, name, profile FROM topic LEFT JOIN author ON topic.author_id = author.id;

UPDATE author SET profile='database administrator' WHERE id=2;

하나를 바꾸면 전체가 바뀐다.
~~~

---

# 16 인터넷과 데이터베이스

**Internet**
client와 server

MySQL을 설치하면  
database client와 database server가 설치된다.  
databse server를 직접 다룰 순 없다.  

**database server**
databse client를 통해 (mysql 명령문) server에 접속해야 한다.  
MySQL monitor: 명령어를 통해 database server를 제어하는 프로그램이자 클라이언트  

Workbench -> GUI환경에서 엑셀을 다루듯이 database를 다룰 수 있음.  

database server를 중심으로 전세계 수많은 client들이 데이터들을 넣고 빼고 하는게 가능해짐.  

---

# 17 MySQL 클라이언트

MySQL monitor  
어디에서나 사용 가능  
명령어 기반 프로그램(CLI) <-> (GUI)  
명령어 모르면 쓰기 힘듦  

---

# 18. MySQL workbench

mysql -u root -p -h localhost  
h: host 인터넷에 연결되어있는 각각의 컴퓨터  

---

# 19. 수업을 마치며

- SQL
- index
- modeling
- backup ex) mysqldump, binary log
- **cloud** - 큰 기업이 운영하는 인프라 위 컴퓨터 임대 ex) AWS RDS, Google Cloud SQL for MySQL, AZURE Database for MySQL
- Programming ex) python mysql api, php mysql api, java mysql api

데이터베이스 자체를 쓰는 경우는 드뭄  
부품으로서 씀  

