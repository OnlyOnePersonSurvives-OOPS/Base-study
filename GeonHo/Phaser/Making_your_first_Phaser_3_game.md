# Making your first Phaser 3 game

## Part1. Introduction

달리기, 점프기능을 가진 player로 별 먹기, 피하기 게임을 만들어 볼 것이다.  
이를 만들며 프레임워크의 핵심 특징들을 배워보자.

### 페이저란?

개발자들이 브라우저에서 호환되는 HTML5 게임을 빠르게 만들 수 있도록 도와주는 프레임 워크  
이는 데스크탑, 모바일의 현대 브라우저 모두 이용할 수 있도록 하기 위해 만들어졌다.  

브라우저의 오직 한 가지 필요 조건은 canvas tag 지원 뿐이다.  

### Requirements

part1.html엔 이러한 코드가 있다.

~~~
var config = {
    type: Phaser.AUTO,
    width: 800,
    height: 600,
    scene: {
        preload: preload,
        create: create,
        update: update
    }
};

var game = new Phaser.Game(config);

function preload ()
{
}

function create ()
{
}

function update ()
{
}
~~~

Config 객체: 나의 Phaser Game을 조정할 수 있다.  
break-out game에서 한 거랑 똑같다. 그러나, 변수에 key-value처럼 쌍이 맺어져 있다.  

Phaser.Game 객체가 game 변수에 할당 되고 그 Phaser.Game 객체엔 config 객체가 넘겨진 것을 볼 수 있다.
이것은 Phaser에 생명을 불어넣는 프로세스의 시발점이 될 것이다.  

---

## Part2. Loading Assets

preload function: Phaser는 시작될 때 자동적으로 preload 함수를 찾고 이 안에 정의된 것들을 불러온다.  

~~~
function preload ()
{
    this.load.image('sky', 'assets/sky.png');
    this.load.image('ground', 'assets/platform.png');
    this.load.image('star', 'assets/star.png');
    this.load.image('bomb', 'assets/bomb.png');
    this.load.spritesheet('dude', 
        'assets/dude.png',
        { frameWidth: 32, frameHeight: 48 }
    );
}
~~~

다 배웠던 것!  
근데 2 때와 다르게 game.load.image가 아니라 this.load.image이다.  


create function:
asset 불러오기

2 땐,
~~~
        paddle = game.add.sprite(game.world.width*0.5, game.world.height - 5, 'paddle');
        paddle.anchor.set(0.5, 1);
~~~
game.add.sprite를 paddle변수에 담고 anchor.set으로 sprite의 중앙이 설정한 좌표에 오도록 했다.

그러나 3에선
~~~
function create ()
{
    this.add.image(400, 300, 'sky');
    this.add.image(400, 300, 'star');
}
~~~

sky의 이미지가 800x600 픽셀이어서 그 중앙인 좌표를 add.image에서 설정해주면 딱 맞게 떨어진다.  
anchor를 해줄 필요가 없다는 뜻!  


2처럼 하는법!!
~~~
this.add.image(0, 0, 'sky').setOrigin(0, 0)
~~~

주의!!  
star를 먼저 넣으면 sky이미지가 그를 덮는다!  

## Part 3 - World Building  

참고로 2보다 3에서 카메라 기능이 더 강력해졌는데 이는 다음에 알아보도록 하자!  


~~~
var platforms;

function create ()
{
    this.add.image(400, 300, 'sky');

    platforms = this.physics.add.staticGroup();

    platforms.create(400, 568, 'ground').setScale(2).refreshBody();

    platforms.create(600, 400, 'ground');
    platforms.create(50, 250, 'ground');
    platforms.create(750, 220, 'ground');
}
~~~

this.physics: Arcade Physics 시스템을 쓰고있다는 뜻.  
쓰기 전엔 Phaser에게 우리의 게임이 physics가 필요하다고 말해줄 코드를 Game Config에 넣어줘야 한다.  

~~~
var config = {
    type: Phaser.AUTO,
    width: 800,
    height: 600,
    physics: {
        default: 'arcade',
        arcade: {
            gravity: { y: 300 },
            debug: false
        }
    },
    scene: {
        preload: preload,
        create: create,
        update: update
    }
};
~~~

결과를 보자.  

이런 플랫폼들은 정확히 어떻게 작동하는 것일까?  

---

## Part 4 - The Platforms

~~~
platforms = this.physics.add.staticGroup();
~~~

Dynamic: 속도, 가속도 같은 힘에 의해 움직일 수 있음. Coliision 가능 (질량 및 다른 요소에 영향)  
Static: 위치, 크기를 가짐. 중력, 속도 받지 않음, 부딪히더라도 움직이지 않음.  

Group: 비슷한 객체끼리 묶어서 컨트롤 할 수 있게 도와줌. 마치 한 unit인 것 처럼.  
그룹은 쓰기 쉬운 creat()같은 함수를 통해 자기 그룹에 속하게 될 객체를 만들 수 있다.  
물리 그룹 -> 자동적으로, 소속한 자식들에게 물리 성질을 부여해준다!  

~~~
platforms.create(400, 568, 'ground').setScale(2).refreshBody();

platforms.create(600, 400, 'ground');
platforms.create(50, 250, 'ground');
platforms.create(750, 220, 'ground');
~~~

첫번째 코드: 400x32픽셀의 이미지로 800x600의 바닥을 메우기 위해 setScale(2)라는 함수를 적용했다.  
refreshBody()는 우리가 static 물리체를 스케일링 했기 때문에, 물리계에게 우리가 변경한 점을 알려주는 것이다.(대충이정도로 알고있자. 바꿨으면 물리계에게 변경점을 알려줘야 한다.)  


