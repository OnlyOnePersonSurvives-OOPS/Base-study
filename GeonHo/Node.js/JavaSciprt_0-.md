# JavaScript

## 1. 문법 - Number

Data를 어떻게 처리할 것인가: 프로그래밍을 배우는 이유  

1. 어떠한 데이터가 있는가 (종류)
2. 어떻게 처리하는가  

Number, String, Boolean, Array, Obejct....

numbers.js

~~~
console.log(1+1);
~~~
콘솔창에  
node syntax/numbers.js

---

## 2. 문법 - String
'1' or "1"

~~~
console.log("1" + "1");
~~~

더하기는 산술 연산자가 아니라 **결합 연산자** 이다.

어떤 데이터를 사용하느냐에 따라 연산자의 의미가 달라짐.

~~~
var str = "Hello World";
var n = str.length;

---

console.log(n);
~~~


---

## 3. 문법 - 변수의 형식

변수와 상수.

변수 앞에다
~~~
var a = 1;
~~~
var을 붙여주는 습관을 갖자. (나중에 유효범위 배울 때 중요)  

변수를 왜 쓰는가  

변수에게 이름을 붙인다.

코딩을 깔끔하게 하는법: **중복**을 제거하라.  

---

## 4. 문법 - Template Literal
js의 string 편리한 기능

1. 줄바꿈을 하고 싶을 때,
var letter = 'Dear ' + name + '\
\ asdfavavas ds a sad s';
역 슬래쉬 두번!
그러나 코드상에서만 줄바꿈이 되었을 뿐, 문자로 표현했을 땐, 똑같이 한줄로 나온다.

2. '\n'
실제로 줄바꿈 표현해주기.  

3. 비교적 최신 문법의 Template
literal: 정보를 표현하는 방법, 기록 (1: number, '1': string)  

` -> template의 시작과 끝을 나타내는 기호  

`lorem ${name} 

lorem lorem lorem ${num} lorem ${name}`

참 쉽다!

${} 안엔 숫자 연산도 가능하다.

---

## 5. 문법 - Boolean, 비교 연산자. 제어문, 조건문

### Boolean
true or false

### Comparison Operator

==, >=, <=, !=

==와 ===의 차이?

### 제어문

Program??  
시간의 순서에 따라 오페라가 진행 ( 프로그램 )  

프로그래머:  
시간의 순서에 따라서 실행되야 할 컴퓨터의 명령어를 실행되도록 설계하는 사람  

기계에게 맡기고 싶어함.  

조건문과 반복문  

### 조건문

if (true) {
console.log("123");
} else(false) {
console.log("456");
}
