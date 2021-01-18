# [Getting Started with Phaser 3 1-](https://phaser.io/tutorials/getting-started-phaser3/index)

## Part1. Introduction

### 웹서버가 필요한 이유?

"html파일을 내 브라우저에 드래그해서 놓으면 되는거 아냐?"  

**브라우저 보안** 때문이다!  

static html을 만든다면, 드래그해도 상관 없다.  
왜 안될까.  

이는 파일에 접근하기 위한 프로토콜 때문이다.  
웹페이지에 http를 통해 어떤 것을 요청했을 때, 웹 서버 레벨은 내가 원하는 것에 대한 접근을 준다.  
하지만, 드래그하는 경우 로컬 파일 시스템을 통해 불려와지며, (file://) 이는 매우 제한적이다.  
이것은 도메인과 서버 레벨 보안이 없으며 단지 raw한 파일 시스템일 뿐이다.  

생각해보자.  
자바스크립트가 내 로컬 파일 시스템에 있는 어떤 파일이든 가져오는 기능이 있을 것이라 원하는가?  
백만년은 이르다!  

만약 자바스크립트가 file:// 아래에 대한 모든 제어권을 가진다면, 그 아래 파일들을 어디사는 누구에게 얼마나 많이 보내더라도 막을 방도가 없다.  

이런 상황이 매우 위험하기 때문에, file:// 에서 실행되면 브라우저가 스스로에게 엄격한 잠굼을 건다.  
[더 자세한 정보](https://blog.chromium.org/2008/12/security-in-depth-local-web-pages.html)

우리의 게임은 많은 resource들을 불러와야 한다. (이미지, 오디오, JSON, 다른js파일)  
이를 위해선 브라우저 보안이라는 족쇄가 없어야 한다!  
이것이 http:// 를 통해 게임에 접근해야하는 이유이며, 우리가 웹 서버를 설치해야 하는 이유다.

---

## Part2. Installing a web server

### windows

윈도우엔 하나의 exe 파일로부터 유명한 웹 기술인, 아파치. php, MySQL을 한군데 묶어 설치해주는 Bundle Installers가 많다.

ex)
**WAMP Server**
Cesanta의 [Mongoose web server](https://cesanta.com)

번들이 아닌경우
[Microsoft IIS](https://www.iis.net/)
[Apache](https://www.iis.net/

**참고**: WAMP 서버를 사용한다면 IE11가 프리즈되는 현상이 일어날 수 있습니다. 이를 고치려면 이 사이트의 링크 (https://stijndewitt.wordpress.com/2014/01/10/apache-hangs-ie11/) 를 참고하세요. 

### [파이썬을 이용한 간단한 웹서버](https://www.linuxjournal.com/content/tech-tip-really-simple-http-server-python)

파이썬은 간단한 내장 http서버를 가지고 있으며, 아무 로컬 폴더에서나 파일들을 처리할 수 있다.  

### Node.js 기반의 http-server

http서버는 단순하고 설치할 필요도 없는 node.js기반의 커맨드라인 http서버이다.  
실제 프로덕션으로 사용할 만큼 강력하지만, 테스팅, 개인 개발, 학습 용도로도 쓰기에 충분하다.  
정적 파일들을 로켓달린 거북이처럼 만들어주는 일도 가능하다.  
[http-server](https://npmjs.org/package/http-server)

### 만들어져 있는 php 서버

PHP 5.4.0의 CLI SAPI는 이미 만들어져 있는 웹서버를 제공한다.  
이것은 오직 개발 목적에 적절하고 모든 파일들을 순차적으로 처리하지만, html5 웹게임을 테스팅 하기에 충분하다.  
한 줄의 커맨드 라인 호출로 실행할 수 있으며, 상세한 설명과 사용법은 [PHP 매뉴얼](https://npmjs.org/package/http-server)에 나와 있다.  

어떻든, 파일들을 로컬에서 처리하는 것도 중요하다. PART3에선 어떤 IDE를 고를지 확인해보자.  

---

## Part3 - Choosing an Editor

### Visual Studio Code
가볍지만 강력한 소스코드 에디터이다.  
기본적으로 JavaScript, TypeScript, Node.js를 제공하며 다른 언어들과 런타임에 대해서도 확장된 환경을 가지고 있다.  


---

## Part 4 - [Downloading Phaser](https://phaser.io/download/stable)

### Can I just get a zip/tar file?

가능하다. 깃헙은 repo전체를 zip 또는 tar로 파일 다운을 제공한다.  
근데 git부터 배우고 와라.  
phaser업데이트를 쉽게 제공해준다.  

커맨드 라인을 통해 깃을 사용하는 법을 배울 생각이 없다면, 대신 쓸 수 있을 법한 좋은 어플리케이션이 몇 가지 있다.
Windows라면 [SmartGIT](https://www.syntevo.com/)
OS X라면 [Git Tower](https://www.git-tower.com/)

### Part 5 - Hello World!

