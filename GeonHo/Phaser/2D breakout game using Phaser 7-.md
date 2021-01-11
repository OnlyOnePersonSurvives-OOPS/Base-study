# 2D breakout game using Phaser 7-

---

## 7. paddle 조작

### Rendering the paddle

- 패들 Load하기
쉽다!
ball을 preload에서 load하고 create에서 sprite로 추가한 것 그대로 하면 된다!

- physics와 함께 패들 render하기

여기서 ball과 다른점은 다음과 같다.
~~~
paddle = game.add.sprite(game.world.width*0.5, game.world.height-5, 'paddle');
~~~
game.world 는 canvas를 의미한다.
game.world.width - canvas의 너비

이렇게 하면 paddle이 정중앙에 오지 않는다.
왜? -> anchor는 항상 좌상단을 기준으로 하기 때문이다.
때문에 anchor를 paddle의 중앙으로 맞춰줄 필요가 있다.

~~~
paddle.anchor.set(0.5,1);
~~~

0.5는 paddle 너비의 절반
1은 paddle 높이를 의미한다.


이제 paddle을 공과 부딪히게 해야한다.
그러기 위해선 paddle에도 physics를 추가하면 된다.

~~~
game.physics.enable(paddle, Phaser.Physics.ARCADE);
~~~

그런 다음 프레임워크가 매 프레임마다 충돌 감지를 확인할 수 있게끔 관리해야 한다.
update 함수에 다음과 같은 코드를 추가하면 ball과 paddle간의 충돌이 매 프레임마다 감지된다.

~~~
game.physics.arcade.collide(ball, paddle):
~~~

실행해보면
공과 패들이 부딪혔을 때
패들이 화면 아래로 사라지는 것을 발견할 수 있다.

패들이 움직이지 않게 만들어야 한다.

~~~
paddle.body.immovable = true;
~~~

### Controlling the paddle

플랫폼마다 input의 디폴트값이 다르다.
보통 마우스로 설정되어 있을 것이다.

update에 다음과 같은 코드를 추가해보자

~~~
paddle.x = game.input.x
paddle.y = game.input.y
~~~

그렇다면 마우스로 paddle의 위치를 자유자재로 조절할 수 있다.
game.input.x : 마우스를 좌우로 움직이면
paddle.x : paddle의 x 좌표가 바뀐다
정도로 해석하면 된다.

그런데 여기서 웹 브라우저를 켰을 때
paddle이 우리가 설정했던 것처럼 중앙에 오지 않는 것을 볼 수 있다.

이것은 input의 위치가 아직 정의되지 않았기 때문이다.

~~~
paddle.x = game.input.x || game.world.width*0.5;
~~~
이렇게 하면 해결되지만 이 코드가 정확히 어떻게 의미하는지는 모르겠다.
우선 받아들이자.

### Position the ball

이제 공의 초기 위치를 정해보자

~~~
ball = game.add.sprite(game.world.width * 0.5, game.world.height - 25, 'ball');
ball.anchor.set(0.5);
~~~

공의 velocity도 다음처럼 바꿔보자

~~~
ball.body.velocity.set(150, -150);
~~~
