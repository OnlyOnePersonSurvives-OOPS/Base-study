# Express Session & Auth

## 수업 소개

쿠키의 등장 -> 이전의 통신 내용 기억  
개인화, 인증

쿠키 인증 구현은 매우 위험한 일  
조작, 유출 가능성  

세션 사용  
sessionid가 쿠키로 옴 -> 의미없는 식별자  
실제 정보는 세션 폴더 안 파일에 기억됨!  
서버쪽에 저장하는 것  

쿠키를 통해 직접 인증 X

쿠키: 사용자 식별 용도
세션: 실제 데이터를 안전하게 파일이나 데이터베이스 형태로 서버쪽에 저장  

## 1. 예제 구동  
express-session 미들웨어

npm install -s express-session  
이 프로젝트에선 저 모듈에 의존하고 있다!  

---

## 2. 옵션
app.use -> 사용자 요청이 있을 때 마다 안의 코드를 실행  
session함수 실행 -> 세션 시작  

secret: ~~  
필수 옵션  
다른사람들에게 노출되면 안되는 코드!!  
버전관리할 때, 소스코드에 포함시키면 안된다.  

---

## 3. session 객체

console.log('req.session');

app.use (session)을 주석처리하고 다시 reload하면 'undefined'가 뜬다!!  
->  
session 미들웨어는 req객체의 property로 session이라는 객체를 추가시켜준다.  

~~~
    if (req.session.num === undefined){
        req.session.num = 1
    }
    else{
        req.session.num += 1
    }

    res.send(`Hello Session: ${req.session.num}`);
~~~

노드js를 끄면 메모리에 저장되어있던 세션이 휘발된다.  
session데이터는 메모리가 아닌 휘발되지 않는 저장공간에 저장해야 한다!

---

## 4. session store

session-file-store

npm install session-file-store

~~~
var express = require('express')
var parseurl = require('parseurl')
var session = require('express-session')
var FileStore = require('session-file-store')(session)
 
var app = express()
 
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true,
  store:new FileStore()
}))
~~~

sessions라는 폴더안에 json파일이 생긴다!  

다음에 요청할 때도 num의 값이 저장되어 있다.  

---

## 5. 로그인 기능 만들기

### UI 만들기

실습!!

### 인증 기능 구현

실습!!!

### 세션 미들웨어 설치

~~~
var session = require('express-session')
var FileStore = require('session-file-store')(session)
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true,
  store:new FileStore()
}))

var indexRouter = require('./routes/index.js');
var topicRouter = require('./routes/topic.js');
var authRouter = require('./routes/auth.js');
~~~

이 순서가 되어야 한다!!

### 인증 상태를 UI에 반영

따로 auth.js 파일로 만들고  
module.exports하기!!  

### 로그아웃

~~~
req.session.destroy(function(err){
})

### 접근제어
~~~
if (!auth.isOwner(req, res)){
	res.redirect('/');
	return false;
}
~~~

alert 메세지 띄우기
~~~
  if (!auth.IsOwner(req, res)){
    res.send('<script type="text/javascript">alert("권한이 없습니다."); window.location.href = "/auth/login"</script>');

    return false;
  };
~~~

### 세션 저장

~~~
req.session.save(function(){
	res.redirect('/');
});
~~~

세션 스토어에 프로퍼티들을 반영한 후에 콜백 함수를 실행하도록 하는 것


---

## 수업을 마치며

여전히 보안해야할 점은 많이 남아있음  
http로 통신한다는 것 -> 누군가 우리의 통신 내용을 보고 있다는 것  

app.use(session({
	secure: true,
	HttpOnly: true
}))
-> https로만 통신 가능하게 만든다.  

-> js를 통해서 세션 쿠키를 감지할 수 없도록 만든다.  

- 다중사용자 수용 서비스  

- oauth: federation authentication -> 타사에게 인증을 맡김  
자사는 회원의 식별자만을 유지  
- passport.js 
