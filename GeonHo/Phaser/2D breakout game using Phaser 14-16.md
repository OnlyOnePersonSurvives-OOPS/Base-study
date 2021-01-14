# 2D breakout game using Phaser 14-16

## 14. 애니메이션과 Tween(??)

### Animations

페이저의 애니메이션 : 외부 소스에서 spritesheet를 가져와서 스프라이트를 순차적으로 표시하는 것.  

~~~
game.load.spritesheet('ball', 'img/wobble.png', 20, 20);
~~~

다음 그림과 같은 표현하고자 하는 애니메이션 frame이 다 들어가 있는 spritesheet를 가져올 것이다.

<img src = 'img/wobble.png'>
왜 첫번째 인자가 'wobble'이 아닌 'ball'인지는 모르겠다.

셋, 네번째 인자는 spritesheet를 어떻게 자를것인지를 의미한다.

다음 그림과같이 spritesheet가 잘라지고 0, 1, 2의 index가 부여된다.  
<img src = 'img/wobble_chopped.png'>

### Loading the animation

이제 object(ball)에 애니메이션을 추가해보자.

~~~
ball.animations.add('wobble', [0,1,0,2,0,1,0,2,0], 24);
~~~

- 'wobble' : 애니메이션의 name
- 우리가 잘랐던 spritesheet을 다음과 같은 순서로 보여주겠다.를 의미한다.
- The framerate, in fps. 1은 느리고 24는 빠르다.

### 볼이 패들에 부딪힐 때 애니메이션 넣기

~~~
function update() {
    game.physics.arcade.collide(ball, paddle, ballHitPaddle);
    game.physics.arcade.collide(ball, bricks, ballHitBrick);
    paddle.x = game.input.x || game.world.width*0.5;
}
~~~

~~~
function ballHitPaddle(ball, paddle) {
    ball.animations.play('wobble');
}
~~~
ball에다 wobble 애니메이션을 실행시킨다!


### Tweens (벽돌 부드럽게 사라지게 만들기)

animations은 외부의 sprites를 가져오는 형식이지만,  
tweens는 게임 안에서 너비나 투명도를 부드럽게 조절한다.

brick.kill(); 대신  

~~~
var killTween = game.add.tween(brick.scale);
killTween.to({x:0,y:0}, 200, Phaser.Easing.Linear.None);
killTween.onComplete.addOnce(function(){
    brick.kill();
}, this);
killTween.start();
~~~

코드를 추가하자.  

1. 벽돌을 일시적으로 없애는 것이 아닌 그들의 너비와 높이를 줄여서 사라지게 만드려고 한다.  
-> add.tween() method를 쓸 것이고 우리가 tween하기 원하는 brick.scale을 인자로 넣을 것이다.  

2. to() method를 사용한다.
-> x와 y는 마지막 너비와 높이를 의미하며 1: 100%, 0: 0%이다.  
-> 200은 tween할 때 걸리는 시간(ms)을 의미한다.  커질수록 느리게 줄어든다.  
-> 마지막 인자는 사라지는 방식 같은 것.  
other) Phaser.Easing.Bounce.Out  

3. onComplete도 추가할 수 있는데 tween이 끝날 때 brick을 없애라는 명령어 이다.  

4. 마지막으로 killTween.start();로 tween을 바로 시작한다.  


~~~
game.add.tween(brick.scale).to({x:2,y:2}, 500, Phaser.Easing.Elastic.Out, true, 100);
~~~

짧게 줄인 코드:  
500: 걸리는 시간(ms)  
100: 발생하기까지의 딜레이  

---

## 15. Buttons

### 변수 선언

~~~
var playing = false;
var startButton;
~~~
- playing: 게임이 현재 진행중인지 아닌지
- startButton: 말 그대로 버튼을 의미

### 버튼 spritesheet 불러오기

~~~
game.load.spritesheet('button', 'img/button.png', 120, 40);
~~~

### 게임에 버튼 추가하기

~~~
startButton = game.add.button(game.world.width*0.5, game.world.height*0.5, 'button', startGame, this, 1, 0, 2);
startButton.anchor.set(0.5);
~~~

- startGame: callback function - 버튼을 눌렀을 때 실행될 함수
- this : 모르겠음
- 올려놨을 때, 아닐 때, 눌렀을 때 sprite frame 의미


이제 startGame() 즉, 버튼이 눌러졌을 때 실행될 함수를 정의해보자.  
~~~
function startGame() {
    startButton.destroy();
    ball.body.velocity.set(150, -150);
    playing = true;
}
~~~
1. 버튼이 사라지게 한다.  
2. ball이 다음과 같은 속력으로 움직이게 한다. (기존의 create의 것은 제거)
3. 게임이 진행중이라는 변수로 바꾼다.  


### Keeping the paddle still before the game starts

~~~
function update() {
    game.physics.arcade.collide(ball, paddle, ballHitPaddle);
    game.physics.arcade.collide(ball, bricks, ballHitBrick);
    if(playing) {
        paddle.x = game.input.x || game.world.width*0.5;
    }
}
~~~
쉽다.  
이제 plaing 변수를 이용할 때다.  
단순 if문만 하면 해결!  

---

## 16. 게임 플레이 randomizing!!

### Making rebounds more random

~~~
function ballHitPaddle(ball, paddle) {
    ball.animations.play('wobble');
    ball.body.velocity.x = -1*5*(paddle.x-ball.x);
}
~~~
쉽다!
