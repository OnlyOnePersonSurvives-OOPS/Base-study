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

---

## 5. URL 
php라는 app이 client(browser)에게 서로 다른 페이지를 보내주는 것!  
URL의 형식  
http://optutorials.org:3000/main?id=HTML&page=12

- protocol: http, 사용자가 서버에 접속할 때 어떤 방식으로 접속할 것인가. 통신 규칙
- host(domain): open~.org, 인터넷에 접속되어 있는 각각의 컴퓨터.
 특정한 인터넷에 연결되어있는 컴퓨터를 가르키는 주소  
- port: 3000, 한대의 컴퓨터 안에 여러대의 서버가 있을 수 있음.  클라이언트가 접속할 때 그중 어떤 서버에 연결할 것인지 정하는 것.
default: 80
- path: main, 그 컴퓨터 안에 있는 어떤 디렉토리 어떤 파일인지  
- query string: ?id=HTML~, 시작은 ?. 값과 값은 %&, 값의 이름 = 로 구분  
내가 읽고싶은 정보는 html이고 12페이지이다. 라고 전달!

---

## 6. URL을 통해서 입력된 값 생성하기

주소창에 localhost:3000/?id=CSS를 치기전에 

~~~
console.log(request.url);

>>> node main.js
~~~

하고 결과를 보자  
에러가 뜨지만, /id?=CSS라는 결과가 뜬다!  


nodejs url parse(분석) query string  

~~~
var url = require('url');
~~~
url이라는 모듈을 불러올 것이다.  

~~~
    var queryData = url.parse(request.url, true).query;
    console.log(queryData)
~~~
url모듈의 parse 메서드를 이용해서 queryData엔 무엇이 담기는지 확인해보자.  

{ id: 'HTML' }

라는 결과가 나온다.

~~~
console.log(queryData.id)
~~~
HTML이라는 결과가 나온다.

이제
~~~
response.end(fs.readFileSync(__dirname + _url));
~~~
이를

~~~
response.end(queryData.id);
~~~
로 바꾸고 localhost:3000?id=HTML로 접속하면  
HTML이라는 글자가 화면에 나온다.

queryString에 따라 다른 결과를 만들어내는 App을 만들어 본 것이다.  
