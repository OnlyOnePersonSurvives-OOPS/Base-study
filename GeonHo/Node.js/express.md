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

semantic url
clean url
pretty url

Route Parameter

쿼리 방식이 아닌 path 방식
~~~
app.get('/page/:pageId', (req, res) => 
  res.send(req.params))
~~~

{'pageId': 'HTML'}

req.params.pageId

---

## 6. 페이지 생성 구현

~~~
app.post('/create_process', function(req, res){
}
~~~

app.get과 app.post 둘 다 '/create'로 해도 된다.  

---

## 7. 페이지 수정 구현

실습!!!

---

## 8. 페이지 삭제 구현

express의 redirection 기능  

~~~
res.redirect('/')
~~~

훨씬 간편하다.  


---

## 9. 미들웨어

express는 Route와 미들웨어 두 가지가 중요!!  

다른사람이 만든 소프트웨어를 부품으로 삼아 내 것을 만든다.  

### body-parser  
npm install body-parser  
body: 웹 브라우저 쪽에서 요청한 정보의 전체  

~~~
form 형식으로 왔을 때,  
app.use(bodyParser.urlencoded({ extended: false }));
 

var post = request.body;
~~~
use 안에 미들웨어가 들어온다.  
main.js가 실행될 때 마다, 요청이 들어올 때 마다, 미들웨어가 실행된다.

사용자가 post한 정보를 분석해서 모든 데이터를 가져온 다음 create_procee 안 함수를 callback  
request에 body라는 property를 만들어준다.  

### compression
이 응답은 zip방식으로 압축했으니 이 방식으로 풀면 됩니다.  
네트워크 전송 비용 절감  

npm install compression

~~~
var compression = require('compression');
app.use(compression());
~~~

compression()이 미들웨어를 리턴 -> app.use를 통해 장착  
30kb가 4kb로 줄었다! ctrl + shfit + R  
content-encoding: gzip  
압축된 방식!  

---

## 10. 미들웨어 만들기

~~~
app.use(function(req, res, next){
  fs.readdir('./data', function(error, filelist){
    request.list = filelist
    next();
 });
});
~~~

모든 route 안에서 request.list를 통해 글 목록에 접근 가능해진다.  

그러나!  
app.post(~) 에서는 req.list를 불러올 필요가 없다  

따라서
~~~
app.get('*', function(req, res, next){
  ~
})
~~~
get방식 요청에만 filelist를 불러온다!  

---


## 정적인 파일의 서비스

정적인 사진 및 CSS를 express로 띄우기 

 
