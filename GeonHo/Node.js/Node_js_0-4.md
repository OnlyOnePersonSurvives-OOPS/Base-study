# Node.js

## 0. 수업 소개

한계점.  
html을 직접 타이핑해서 웹 페이지를 **수동적**으로 만드는 것에 지쳤다.  
웹페이지의 소유자만이 html contents를 수정할 수 있었다.  

Node.js - 자바스크립트를 이용해서 컴퓨터 자체를 제어!  

---

## 1. 수업의 목적

생산성의 한계  
수정해야 할 웹 페이지가 1억개라고 상상  

template.js라는 파일 안에 html 코드가 있다. 이를 수정하면 1억개 페이지가 동시에 바뀜  

사용자의 참여도 유연하게 가능  

CRUD를 웹을 통해서 가능하게 해준다.  

php, jsp, django, ruby on rails는 Node.js와 경쟁관계에 있음.  

---

## 2. 설치

콘솔 창에서 실행하기!!

console.log(1+1);

---

## 3. 공부 방법

Node.js로 만든 App 만들기

JavaScript -> Node.js runtime -> Node.js Application

반복 진행

---

## 4. 웹 서버 만들기

Node.js 웹 서버 기능 내장 (Apache가 할 수 없는 일도 가능)  

main.js를 만들고 주소창에 localhost:3000를 치면 html이 실행됨  
Node.js가 웹서버로 실행된다.  

~~~
console.log(__dirname + url);
~~~
이 코드를 추가해보자.  

C:\Users\rjsgh\Desktop\web\node.js/index.html  

터미널에 이와같은 결과가 뜬 것을 볼 수 있다.  

사용자가 요청할 때마다 js의 코드를 통해 우리가 읽어올 파일들을 만들게 된다.  

사용자에게 전송할 데이터를 생성한다는게 Node.js의 장점!  


