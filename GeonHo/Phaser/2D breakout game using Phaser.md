---
# 2D breakout game using Phaser

## 1. Initialize the framework

### Phaser Code 다운로드 하기
1. 다운로드 페이지 http://phaser.io/download/release/2.16.0 (본 페이지에선 V3.0이 아닌 V2.0을 다룰 것이다.)
2. 자신에게 맞는 옵션을 선택한다. min.js가 가볍고 우리는 소스 코드를 뜯어볼 필요가 없기 때문에 이를 쓰길 추천한다.
3. index.html 파일이 위치해 있는 곳에 js 폴더를 만들고 그 안에 넣어둔다.
4. <script src="js/phaser.min.js"></script> -> javscript에 src 속성을 이용해서 적용시킨다.

### Walking through what we have so far
canvas element는 pahser 프레임워크에 의해 자동적으로 생성된다.
-> Phaser.Game 객체를 생성하면 된다.
~~~
<script>
    var game = new Phaser.Game(
        480, 320, Phaser.CANVAS, null, {
      preload: preload, create: create, update: update
        }
    );
    function preload() {}
    function create() {}
    function update() {}
</script>
~~~
각각의 변수들을 알아보자
- width & height -> canvas의 너비와 높이
- Rendering Method -> option : AUTO, CANVAS, WEBGL -> 우리는 CANVAS를 사용할 것이다.
- canvas의 id -> 그러나 우리는 하나만 쓸거기 때문에 null값을 줬다.
- phaser가 사용할 세 가지 주요 함수들의 이름을 적는다.
 1. preload, 2. create, 3. update
 -> 아직 와닿지 않지만 단계 거쳐가며 익숙해지기.
 

## 2. The Phaser scale object
* scaling -> 우리는 크기가 제각각인 스크린에서도 자동적으로 canvas의 크기가 조절되길 원한다.
-> 이는 preload에서 배울테니 차근차근 알아보자

