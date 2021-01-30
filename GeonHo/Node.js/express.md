# express

## 1. 수업소개

순수한 Node.js -> 어플리케이션 구현  
세련되지 못하다! 다소 불편하다!   

framework: 반복적으로 등장하는 일들을 자동적으로 처리해주는 기능들을 모아둔 것  

사용하기 편하나 배우기 어렵다.  

---

## 2. 실습 준비
~~~
npm install
~~~

---

## 3. Hello World

~~~
npm install express --save
~~~

~~~
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(3000, () => console.log('Example app listening on port 3000!'))
~~~

express는 함수다!  
app = express()  
application이라는 객체가 담긴다  

app.get(경로, 호출될 함수)

~~~
app.get('/', function(req, res){
	return res.send('Hello World')
});
~~~

**get**: routing, rout -> 갈림길에서 적당한 곳으로 방향을 잡는 것  

if문을 줄여준다.  

app.listen(3000, ~)
listen이란 메서드가 실행될 때 비로소 웹서버가 실행되고 3000 port에 리슨하고 리슨에 성공하면 =>다음 코드를 실행한다.  
app.listen(3000)과 똑같다.  

---

## 4. 홈페이지 구현

~~~
app.get('/page', (req, res) => 
  res.send('/page'))
~~~

---

## 5. 상세페이지 구현

Route Parameter

쿼리 방식이 아닌 path 방식
~~~
app.get('/page/:pageId', (req, res) => 
  res.send(req.params))
~~~

{'pageId': 'HTML'}


---

## 정적인 파일의 서비스

정적인 사진 및 CSS를 express로 띄우기  
