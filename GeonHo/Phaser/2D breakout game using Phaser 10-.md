# 2D breakout game using Phaser 10 -

## 10. 충돌감지

### 벽돌 - 공 충돌 감지

~~~
function update() {
    game.physics.arcade.collide(ball, paddle);
    game.physics.arcade.collide(ball, bricks, ballHitBrick);
    paddle.x = game.input.x || game.world.width*0.5;
}
~~~

update 함수 안 두번째 라인을 보자.
collide 안에 인자가 세개있다.
ball과 bricks가 충돌하게끔 physics를 주는 것은 알겠다.

세번 째 인자는 바로, ball과 bricks가 부딪히면 이 세번째 인자가 실행되는 것을 나타낸다.
즉, ballHitBrick은 함수이다.

~~~
function ballHitBrick(ball, brick) {
    brick.kill();
}
~~~
