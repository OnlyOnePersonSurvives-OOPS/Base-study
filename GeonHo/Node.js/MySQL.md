# MySQL

## 0. 수업 소개

기존까지 데이터를 파일로 저장  
그러나, 본문에 Node.js라는 단어가 있는 파일을 모두 찾으려면 너무 비효율적이다.  
이름, 날짜 등 다양한 정보를 표현하기 힘듦.  
유동적인 정렬이 힘듦.  
-> 데이터베이스가 구원자  

---

## 1. 실습 준비

vscode 파워쉘 터미널  

package.json  
dependencies -> sanitize-html  

npm install -> 위를 찾아서 node_modules 디렉토리에 설치해줌  

---

## 2. MySQL 모듈의 기본 사용법

[npm](https://www.npmjs.com/package/mysql)

npm install -S mysql  
npm install --save mysql  
package.json의 디펜던시즈에 추가  

~~~
var mysql      = require('mysql'); # mysql변수로 모듈 사용 선언
var connection = mysql.createConnection({ 
  host     : 'localhost', #데이터베이스 서버가 어떤 컴퓨터에 있는가
  user     : 'root', 
  password : 'as3429',
  database : 'opentutorials'
});
 
connection.connect(); # 접속이 될 것이다.
 
connection.query('SELECT * FROM topic', function (error, results, fields) {
  if (error) {
	console.log(error);
}
  console.log(results);
});
 
connection.end();
~~~

topic에 저장된 결과가 객체 형태로 반환되는 것을 볼 수 있다.  

node.js가 mysql서버에 접속하려면 접속하기위한 정보를 전달해야한다.  
in mysql-monitor  
접속하려면 비번 입력 = password: '111111'  
use opentutorials = database: 'opentutorials'  

접속->질의->exit 이 과정을 mysql 서버에 접속하는 모든 클라이언트는 다 거친다.  

저 mysql.js안에 쿼리문을 다이내믹하게 바꿔준다면 node.js에서 mysql을 다룰 수 있게 된다.  

---

## 3. MySQL로 홈페이지 구현

MySQL 모듈을 웹 애플리케이션에 적용 -> 파일 대상이 아니라 데이터베이스 대상으로 가져와보기  

실습!!

result에 배열로 받아온 topic 테이블에 관련된 객체를 이용하여 홈페이지를 만들 것임  

---

## 4. MySQL로 상세보기 구현

더 안전한 방법
~~~
db.query(`SELECT * FROM topic WHERE id=?, [queryData.id], function(error2, topic){})
~~~

---

## 5. 글 생성하기 구현

실습!

---

## 6. 글 수정하기 구현 / 삭제하기 구현

실습!

---
## 8. Join - 상세보기 구현
## 9. Join - 글 생성 구현

콤보박스
~~~
<select name="author">
	<option value="1">egoing</option>
	<option value="2">duru</option>
	<option value="3">taeho</option>
</select>

~~~

delete하고 create하면 id가 계속 늘어나는 현상 발생  

---
## 10. Join - 글 수정 구현
selected!!
---


