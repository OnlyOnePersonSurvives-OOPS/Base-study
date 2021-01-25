# Node.js

## 12. 객체

### 객체의 형식
- Object vs Array
정보를 정리정돈하는 수납상자

Object: 순서 없이 정리정돈 (식별자는 이름)
Array: 순서에 따라 보관 (식별자(index)는 숫자)

객체의 예
~~~
var roles = {
    'programmer': 'egoing',
    'designer': 'k8805',
    'manager': 'hoya'
}
~~~
console.log(roles.designer);

### 객체의 반복

~~~
for(var name in roles){
    console.log(name, ': ', roles[name]);
}
~~~

### 객체지향 프로그래밍 (oop)

프로그래밍: 데이터 + 데이터처리  

함수는 구문이자 **값!**

함수를 변수에 넣을 수 있음.  

~~~
var f = function(){
    console.log(1+1);
    console.log(1+2);
}

var a = [f];
a[0]();

var o = {
    func: f
}

o.func();
~~~

### 데이터와 값을 담는 그릇으로서 객체

연관된 변수들을 하나의 객체안에 정리 정돈해서 넣을 수 있다.  

~~~
var o = {
    v1: 'v1',
    v2: 'v2',
    f1: function(){
        console.log(this.v1);
    },
    f2: function(){
        console.log(this.v2);
    }
}

o.f1();
o.f2();
~~~

**this**
함수가 객체 안에서 사용될 때, 함수가 자기가 속해있는 객체를 참조할 수 있는 특수한 약속  

객체의 본질은 정리정돈!

---

## App - 객체를 이용해서 템플릿 기능 정리 정돈하기

리팩토링: 동작은 똑같이 하되, 내부의 코드를 효율쩍으로 짜는 행위  

---

### 13. 모듈의 형식

객체가 많아지면 뭐가 또 이를 정리정돈해줄까?  
-> 모듈!!(가장 큰 도구)  

모듈로 정리하면 파일로 쪼개서 웹으로 독립시킬 수 있음  

mpart.js

~~~
var M = {
    v: 'v',
    f: function(){
        console.log(this.v);
    }
}

module.exports = M;
~~~
module.exports = M;
M 객체를 바깥에서도 사용할 수 있도록 하겠다!  

muse.js

~~~
var part = require('./mpart.js');
console.log(part);

prat.f();
~~~
결과  
{ v: 'v', f: [Function: f] }
v

---

## App - 모듈의 활용

라이브러리: 재사용 가능한 작은 로직/프로그램 **들!**

module.exports = {}  
이렇게 해도 동작한다!!

---

## App - 입력 정보에 대한 보안

보안이라는 것?  

우리가 만든것의 보안적 위험 요소  

http://localhost:3000/?id=../password.js  
결과는??...  


~~~
var path = require('path');

...

var filteredId = path.parse(queryData.id).base;
~~~

filteredId = password.js  
파일을 찾을 수 없게되므로 description이 undefined로 뜬다!!  



