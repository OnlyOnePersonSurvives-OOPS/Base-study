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

