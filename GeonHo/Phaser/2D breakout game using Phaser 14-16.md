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
