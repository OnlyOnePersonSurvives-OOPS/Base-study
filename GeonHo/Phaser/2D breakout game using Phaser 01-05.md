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
 
---
## 2. Scaling

### The Phaser scale object
* scaling -> 우리는 크기가 제각각인 스크린에서도 자동적으로 canvas의 크기가 조절되길 원한다.
-> 이는 preload에서 배울테니 차근차근 알아보자

~~~
function preload() {
    game.scale.scaleMode = Phaser.ScaleManager.SHOW_ALL;
    game.scale.pageAlignHorizontally = true;
    game.scale.pageAlignVertically = true;
}
~~~

game.scale.**scaleMode**
- NO_scale : 아무것도 scale되지 않는다.
- EXACT_FIT
- **SHOW_ALL** : 가로 세로 비율을 유지한 채 캔버스의 배율을 조정한다.
- RESIZE
- USER_SCALE

game.scale.pageAlignHorizontally = true;
game.scale.pageAlignVertically = true;
- 수평, 수직 가로 정렬을 해준다.

### canvas 배경 색 바꾸기
    game.scale.pageAlignHorizontally = true;
    game.scale.pageAlignVertically = true;
    
---

## 3. assets을 불러오고 화면에 출력하기

### 공 만들기
간단하다.
~~~
var ball;
~~~
끝
변수설정

preload에서 load한 뒤 create에서 화면에 추가하는 꼴

~~~
function preload() {
    // ...
    game.load.image('ball', 'img/ball.png');
}
~~~
'ball' : asset의 이름 설정해주는 것
'imgball.png' : 이미지 파일의 경로 작성

~~~
function create() {
    ball = game.add.sprite(50, 50, 'ball');
}
~~~
- x, y 좌표
- asset 이름

※ 참고 : ball.scale.setTo(0.05,0.05); sprite의 크기를 설정해준다.

---

## 4. 공 움직이기

### 프레임마다 공의 위치 업데이트 하기
Update 함수의 기능은 무엇인가?
-> 이 함수안의 있는 코드들은 매 프레임마다 실행된다!

~~~
function update() {
    ball.x += 1;
    ball.y += 1;
}
~~~

pure js와는 다르게 자취가 남지 않는다!

---

## 5. Physics

### physics 추가하기
phaser는 다음과 같은 physics를 제공한다.
- **Arcade** ( 간단한 물리 법칙만 적용되면 되니 가벼운 엔진을 쓸 것이다. )
- P2
- Ninja
- Box2D (상업용)

create 함수 첫 줄에 다음과 같이 코드를 작성한다.
~~~
game.physics.startSystem(Phaser.Physics.ARCADE);
~~~
-> 우리의 게임에 물리 엔진을 initialize 한다.

Phaser 객체 물리엔진은 기본적으로 활성화되지 않기 때문에 다음과 같은 코드를 추가한다.
~~~
game.physics.enable(ball, Phaser.Physics.ARCADE);
~~~

속도 설정은 다음과 같다.
~~~
ball.body.velocity.set(150, 150);
~~~
ball.x += 150
ball.y += 150
수치가 정확하게 맞진 않겠지만 다음과 같은 의미라 보면 된다.

---

## 6. 벽에 튕기기

### 경계에 튕기기
가장 쉬운 방법은 프레임워크(phaser)에 canvas를 벽으로 만들고 ball같은 element들이 이를 못지나가게끔 알리는 것이다.
**collideWorldsBound** 속성을 사용할 것이다.
이를 create 함수에 다음과 같이 작성한다.

~~~
ball.body.collideWorldBounds = true;
~~~

이렇게 하면 공이 canvas 경계 뒤로 사라지지 않는 결과를 볼 수 있다.
그러나 우리가 원하는 것은 공이 벽에 닿았을 때 튕기는 것이다.

이를 위해선 저 코드 뒤에 다음과 같이 작성하면 된다.

~~~
ball.body.bounce.set(1);
~~~



---

## 사견

확실히 프레임워크 쓰는 것이 내가 원하는 것을 바로 구현하기엔 편한 것 같다.
pure하게 js를 사용했을 때는 원하는 것을 구현할 때 많은 조건문을 썼어야 했는데 프레임워크는 그를 위한 코드가 한줄로 정리될 수 있다.

그렇지만 pure한 js 사용이 나쁜 것은 아니다.
js를 이용해서 구현을 해봐야 밑바닥부터 스스로 설계해가며 보다 js를 체감할 수 있다.
그리고 그렇게 애먹어가며 구현하다가 프레임워크를 만났을 때의 기쁨 또한 얻어갈 수 있다.
