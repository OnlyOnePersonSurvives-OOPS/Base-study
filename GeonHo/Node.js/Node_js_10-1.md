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

