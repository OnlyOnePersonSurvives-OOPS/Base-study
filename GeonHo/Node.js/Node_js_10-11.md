# Node.js

## 10. 동기와 비동기

synchronous & asynchronous  
동기적 & 비동기적  

비동기적: 병렬적으로 여러 일을 처리  

Node.js는 비동기적 처리를 위한 좋은 기능들을 가지고 있음  
효율적이지만 매우 복잡하다.  

readFile vs readFileSync
콜백이 있고 vs 없고

### 동기적
~~~
var fs = require('fs');

// readFileSync

console.log('A');
var result = fs.readFileSync('syntax/sample.txt', 'utf8');
console.log(result);
console.log('C');
~~~
->
A
B
C

### 비동기적
~~~
console.log('A');
fs.readFile('syntax/sample.txt', 'utf8', function(err, result){
    console.log(result);
});
console.log('C');
~~~
->
A
C
B
비동기적 결과

readFileSync는 리턴값을 주지만 readFile은 조금 다르다.

readFile을 실행하면 'sample.txt'를 읽고 그 내용을 저 function에 인자로 공급한다. (error라면 error를 인자값으로 준다.)

Node.js의 성능을 끌어올리기 위해선 비동기적 처리를 최대한 끌어올려야 한다.  

---

## 10+ callback

~~~
fs.readFile('syntax/sample.txt', 'utf8', function(err, result){
    console.log(result);
});
~~~

파일을 읽은다음에 나중에 전화해! (나중에 function을 실행시켜!)  


~~~
var a = function(){
	console.log('A');
}
~~~
JavaScript에서는 함수가 **값**이다!

~~~
function slowfunc(callback){
    callback();
} // 굉장히 오래 걸리는 함수


slowfunc(a);
~~~

지금은 무슨 말을 하려는건지 모르겠다. 나중에 다시 돌어오자.  

---

## 11. 패키지 매니저와 PM2

패키지: 소프트웨어를 부르는 또 다른 말.  

**NPM** 

**PM2**
- 우리가 만든 프로세스(main.js)가 의도치않게 down되면 자동적으로 켜주는 기능
- 파일의 수정을 관찰하고 자동적으로 프로그램 on/off

npm install pm2 -g  
-g: 내가 설치하는 프로그램은 독립된 소프트웨어여서 이 컴퓨터 어디에서든 사용가능해야 한다.
sudo: 이 이후의 명령어는 관리자 권한으로 실행된다.  

pm2 start main.js  

pm2 monit

pm2 list

pm2 stop main

pm2 start main.js --watch

pm2 log (문제점 및 에러를 화면에 바로바로 보여줌)


---

## HTML - form

~~~
<form action="http://localhost:3000/process_create">
    <p><input type="text" name="title"></p>
    <p>
        <textarea name="description"></textarea>
    </p>
    <p>
        <input type="submit">
    </p>
</form>
~~~
form태그 안에 있는 정보들을 action의 주소로 보내겠다!  
아무거나 치고 제출했을 시  
url: http://localhost:3000/process_create?title=hi&description=lorem
query string이 만들어진다!!

form 재정의  
-> form안의 각각의 컨트롤에 사용자가 입력한 정보를 action속성이 가르키는 서버로 query string형태로 보내는 HTML의 기능!!  

서버에서 data를 get할 땐!  
/?id=~~ 쿼리스트링 사용  

서버에서 data를 수정 삭제 할 땐!  
url에 다 보이게 담으면 안됨!!  
보이지 않는 방식으로 보내야 한다.  

~~~
<form action="http://localhost:3000/process_create" method='POST'>
~~~
결과!
http://localhost:3000/process_create


사람눈에 보이지 않는 방식으로 은밀하게 서버에 보낸다.  

서버로부터 사용자가 데이터를 가져올 땐, method가 GET (or 생략)

---

## App - 글생성 UI 만들기

실습!! 

실제로 submit 눌렀을 때,  
Query string parameters를 확인해볼 것!  

## App - POST방식으로 전송된 데이터 받기

post방식으로 전송된 데이터, Node.js안에선 어떻게 가져오는가  

서버에 접속이 있을 때 마다, createServer에 콜백함수를 Node.js가 호출한다.  
request, response 정보  

~~~
var qs = require('querystring');
~~~

~~~
      var body = '';
      request.on('data', function(data){
        body += data;
      })
사용자가 보낸 data를 조각조각 받아서 body에 담겠다.  

      request.on('end', function(){
        var post = qs.parse(body);
        console.log(post);
      })
body에 다 담으면 post에 그 body에 대한 쿼리스트링 정보를 담겠다.  
~~~


콘솔 창  
{ title: 'ads', description: 'avds' }
post.title, post.description을 쓰면 되겠지?  
**정보의 객체화!!**

---

## App - 파일 생성과 리다이렉션  

받은 data를 data directory안에 파일로 저장하는 법  

~~~
        fs.writeFile(`data/${title}`, description, 'utf8', function(err){
          response.writeHead(200);
          response.end('success');
        })
~~~

redirection: 데이터를 제출한 사용자를 다른 페이지로 튕겨내는 것  

~~~
          response.writeHead(302, 
            {Location: `/?id=${title}`});
~~~
302: redirection!!  

---

## App - 글 수정

### 수정 링크 생성

home, create 제외한 나머지엔
~~~
<a href="/update?id=${title}">update</a>
~~~
추가하기

### 수정할 정보 전송

form + read기능 필요  

~~~
          <p><input type="hidden" name="id" value=${title}></p>
          <p><input type="text" name="title" placeholder="title" value=${title}></p>
          <p>
            <textarea name="description" placeholder="description">${description}</textarea>
          </p>
~~~

### 수수정할 내용 저장

~~~
        fs.rename(`data/${id}`, `data/${title}`, function(error){
~~~

---

## App - 글 삭제

### 삭제버튼 구현

update하면 update 페이지로 가지만  
delete 누르면 바로 삭제하고 싶음!  
그러니 delete를 링크로 설정하면 잘못된 것  

이것도 역시 get 방식이 아니라 post 방식으로 보낼 것이다!  

~~~
              <form action="delete_process" method="post">
                <input type="hidden" name="id" value=${title}>
                <input type="submit" value="delete">
              </form>
~~~

### 삭제 기능 완성

~~~
        fs.unlink(`data/${id}`, function(){
          response.writeHead(302,
            {Location: `/`});
          response.end();
        })
~~~
