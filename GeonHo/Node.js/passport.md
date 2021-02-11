# passport

## 0. 수업 소개

트위터  
외부에서 자사의 서비스 데이터를 읽어갈 수 있는 API 공개  

문제는 인증  
제3의 서비스가 자사의 데이터를 가져가려면 사용자의 허가가 필요  
-> 이를 위해선 사용자의 비밀번호 공개 -> 심각한 보안 문제  
대안: OAuth, OpenID  
사용자의 개인정보를 제3의 서비스에게 노출하지 않고도 인증기능 구현  

-> 구글로 로그인하기, 페이스북으로 로그인하기, 구글로 로그인하기  
-> 그러나, 이를 안전하게 구현하는건 매우 어려운 일  
-> Passport 프레임워크 등장  

---

## 1. 설치

Passport 배우기 어려우나 사용하기 편리하다.  

npm install -s passport  

아이디 비밀번호 로그인 Stragy  
npm install passport-local  

주의!!  
passport는 session을 내부적으로 사용하기 때문에  
session 다음에 passport코드가 위치해야 한다.  

---

## 2. 인증구현

login 정보를 받는 것을 passport 체계로 바꿔야 한다. (/auth/login_process)  

~~~
app.post('/auth/login_process',
  passport.authenticate('local', {
    successRedirect: '/',
    failureRedirect: '/login'
}));
~~~

local 방식  
-> username과 pwd를 이용하는 것  
local이 아닌 방식
-> 구글 or 페이스북 or ... 로그인  

성공했을 땐, '/'  
실패했을 땐, '/login'  

---

## 3. 자격확인

어떻게 로그인 성공 실패 여부를 판가름 하는가?  

~~~
passport.use(new LocalStrategy(
  {
    usernameField: 'email',
    passwordField: 'pwd'
  },
  function (username, password, done) {
    console.log('LocalStrategy', username, password);
}))
~~~

안된다? -> 코드 순서가 문제  
bodyparse 미들웨어를 위로 올린다.  


done이라는 함수를 어떻게 호출하느냐에 따라 로그인의 성공 실패 여부를 passport에게 알려줄 수 있다.  

조건문!  

몽고DB로 id pwd를 관리하는 것 같다.  

---

## 4. 세션이용

구현하면 에러가 뜰것이다.  

~~~
app.use(passport.initialize());
app.use(passport.session());
~~~
가 필요  

passport는 session 미들웨어 위에서 동작한다!  

그래도  
Failed to serialize user into session  
사용자 정보를 세션으로 저장해야되는데 session에 대한 setting이 안되어있다!  

~~~
passport.serializeUser(function(user, done) {
//  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
//  User.findById(id, function(err, user) {
//    done(err, user);
  });
});
~~~
->
세션을 처리하기 위한 방법!  


로그인에 성공했을 때,
~~~
return done(null, user);
~~~
user를 반환한 게 serializeUser의 콜백함수의 첫번째 인자로 주입하도록 약속되어 있다.  

~~~
done(null, user.email);
~~~
이런거 약속으로 정해진거니까 너무 따져들지 말자.  

로그인하고 sessions폴더에 생긴 파일을 보면 유저의 id(email)이 담겨있다!  

deserialize는 로그인 하고 페이지에 방문할 때 마다 실행된다!  
세션 저장소에서 사용자의 실제 데이터를 조회해서 가져온다.  


**이해보다 익숙해지자**


serializeUser: 로그인에 성공했을 때, 성공했단 사실을 세션 스토어에 저장하는 기능  
딱 한번 호출

desiralizeUser: 사용자가 페이지에 방문할 때 마다 로그인한 유저인지 확인해주는 기능
매 페이지 방문마다 호출됨.  


~~~
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false,
  store:new FileStore()
}))
~~~
saveUninitialized를 false로 바꿔줘야 잘 실행된다.  


