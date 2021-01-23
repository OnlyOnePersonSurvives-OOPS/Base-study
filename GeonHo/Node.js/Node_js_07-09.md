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

## App - Not found 제작 구현

~~~
    console.log(url.parse(_url, true));
~~~

Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '?id=CSS',
  query: [Object: null prototype] { id: 'CSS' },
  pathname: '/',
  path: '/?id=CSS',
  href: '/?id=CSS'
}

Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: null,
  query: [Object: null prototype] {},
  pathname: '/favicon.ico',
  path: '/favicon.ico',
  href: '/favicon.ico'
}

이런 결과가 나온다.

path -> query string 포함
pathname -> query string 제외한 나머지

웹 브라우저와 웹 서버 사이 요청 응답이 잘 이뤄졌는지 알려주는 통신 약속  
200(성공), 404(파일을 찾을 수 없다.) ....

---

## App - 홈페이지 구현

pathname으로는 home과 각각의 페이지를 구분할 수 없다. (모두 '/')

실습!!!

---

## 9. Node.js - 파일목록 알아내기

문제점 봉착!  
페이지를 하나 찍어낼 때마다 <li>태그에도 일일이 추가해야한다.  

생각해보자.

Data 폴더에서 파일이 생성되고 삭제되고 업데이트 될 때마다 어떻게 Node.js에서 알아낼 수 있는가?  

~~~
var testFolder = './data/';
var fs = require('fs');

fs.readdir(testFolder, function(error, filelist){
    console.log(filelist);
})
~~~

***주의할 점!!!***
js파일이 있는 위치 기준으로 testFolder의 경로를 설정하는 것이 아니라  
실행할 위치 기준으로 testFolder의 경로를 설정해야 한다!!  

결과값  
[ 'CSS', 'HTML', 'JavaScript', 'WEB' ]

그렇다면 반복문을 쓰면 되지 않을까??

---

## App - 제작 - 글목록 출력하기

실습!!
좀 햇갈리는 부분이다. 복습 철저히  

이제 파일을 추가해보자.  
혁명적인 변화가 있을 것이다.  

난 web이란 파일을 추가해서 메인 페이지를 만들었기 때문에 불안정한 코드다.  
WEB보다 사전순으로 더 뒤에오는 파일이 왔을 때, 문제가 될 수 있다. 

---

## App - 제작 - 함수를 이용해서 정리 정돈하기

코드가 많아진다. -> 사람한테 까다롭다.  

함수를 어떻게 사용하는가  

실습!!

--- 
