# 쿠키와 인증

HTTP와 Node.js 선행 지식

## 1. 수업 소개
개인화: 사람마다 다른 웹 페이지를 보여주기  
EX) 장바구니 기록,
한번 로그인하면 쭉 로그인 상태,

쿠키 -> 웹 브라우저 이전 사용자 정보 웹서버에 전송 가능
웹 서버 -> 이 사용자가 누군지 알게 됨  

교육용 영상!!!


---

## 2. 쿠키의 생성

쿠키: http에 포함되어 있는 기술  
모질라 참고!  

- 인증
- 개인화
- 방문자 상태 Check

 개발자 도구 -> network -> localhost통신내역 -> responseHeader

~~~
response.writeHead(200, {
    'Set-Cookie': ['yummy_cookie=choco', 'tasty_cookie=strawberry']
})
~~~

리스폰스 헤더에 쿠키셋이 추가된 것을 볼 수 있다!!

주석으로 하고 다시 들어가면 response Header에선 사라지고  
request Header에 Cookie가 생겼다!  

응답 -> 쿠키 X  
요청 -> 쿠키 O  

SetCookie로 인해서 구워진 쿠키를 저장된 쿠키를 쿠키라는 헤더값을 통해서 서버로 전송한다  
Cookies 탭으로 확인 가능   
Application 탭-> 삭제 가능  

create!!

서버쪽으로 웹브라우저가 전송한 쿠키를 어떻게 볼 수 있는가??

---

## 3. 쿠키 읽기

~~~
console.log(req.headers.cookie);
~~~
cookie handling npm module  

npm install -s cookie

~~~
var cookie = require('cookie');

var cookies = cookie.parse('req.headers.cookie');

console.log(cookies);
~~~
객체를 만들어준다  

서버쪽에서 우리가 전달받은 쿠키를 읽을 수 있다.  

---

## 4. 쿠키의 활용

이거 해서 어따 쓰는가

모질라 영문 한글 개인화  

로그인 식별자  
로그인 성공한 사람의 식별자  
쿠키값이 훔쳐지면 보안사고 발생  

---

## 5. Session cookie vs Permanent cookie

세션 쿠키  
웹 브라우저 껐다 키면 사라짐

Permanent
껐다 켜도 안사라짐  

Max-age=${60*60*24*30}
쿠키가 얼마동안 살 것인가

Expires  
쿠키가 언제 만료될 것인가

---

## 6. 쿠키 옵션 - Secure & HttpOnly

Secure  
Https를 통해서 통신하는 경우에만 쿠키를 전송하게 만듦.  
왜 필요한가?   
값을 훔쳐갈 수 있기 때문에

HttpOnly  
웹브라우저와 웹서버가 통신할 경우에만 뜨게 만든다.    
콘솔창으로 js를 이용해서 document.cookie하면 뜨지 않는다.  
왜 필요한가?  
js를 통해 값을 훔쳐갈 수 있기 때문에  


