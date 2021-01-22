# Node.js and App

## App - 동적인 웹페이지 만들기

실습!!  
[동적으로 title 바꾸기](https://opentutorials.org/course/3332/21047)

---

## 7. Node.js 파일 읽기

**Create**  
Read  
Update  
Delete  

정보를 다루는 핵심적인 처리 방법  
생성과 읽기를 아는 것은 50퍼 이상을 아는 것  
업데이트와 삭제를 추가적으로 아는 것은 75퍼 이상을 아는 것  

File System module (fs)  


~~~
var fs = require('fs');

fs.readFile('sample.txt', function(err, data){
    console.log(data);
});
~~~

node nodejs/fileread.js 쳐보면 결과가 undefined로 나온다.  
왜?  
nodejs 상위 폴더에서 명령어를 실행시켰고 그 상위폴더엔 sample.txt라는 파일이 없기 때문이다.  


~~~
fs.readFile('sample.txt', 'utf8', function(err, data){
    console.log(data);
});
~~~

추가적으로 'utf8'을 인자로 넣어주면 성공적으로 sample.txt를 읽는다.  

---

## App - 파일을 이용해 본문 구현

[실습!!](https://opentutorials.org/course/3332/21049)


---

## 8. Node.js - 콘솔에서의 입력값

**input**
Parameter: 입력되는 정보의 형식  
Argument: 입력된 값

console에서 입력값을 주는 방법 배우기  

~~~
var args = process.argv;
console.log(args); 
~~~
->  
node syntax/input.js egoing

[
  'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Users\\rjsgh\\Desktop\\web\\node.js\\syntax\\input.js',
  'egoing'
]  

args라는 변수 안에 이러한 값들이 있다.  
args[0]: node.js 런타임이 어디에 있는가  
args[1]: 실행시킨 이 파일의 경로  
**args[2]**: 우리가 입력한 값!  

console.log(args[2]); -> egoing

---

## App

