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

---


## 8. GAME OVER!

### 지는 조건

공이 바닥에 떨어지면 지게 만들어야한다.

우선 바닥의 충돌 조건을 없애보자

~~~
game.physics.arcade.checkCollision.down = false;
~~~

공이 좌우와 상단엔 부딪히는 것을 볼 수 있지만 바닥으로는 그대로 뚫고 지나간다.

이젠 공이 바닥을 지났을 때 게임 종료가 되는 조건을 만들어보자
~~~
ball.checkWorldBounds = true;
ball.events.onOutOfBounds.add(function(){
    alert('Game over!');
    location.reload();
}, this);
~~~

**와닿지 않는 코드!!!**
그러나 받아들여보자.

우선 ball.checkWorldBounds = ture;는 공이 world(canvas)의 경계를 check할 수 있게끔 만들어 놓은 것이다.

만약 ball이 bound를 넘었다면 다음과 같은 function이 실행되게끔 만든 것이다.

this의 의미도 잘 모르겠다.

---

## 9. 벽돌 쌓기

### Defining new variables

변수는 다음과 같이 설정한다.
~~~
var bricks;
var newBrick;
var brickInfo;
~~~
bricks : group (brick들을 한꺼번에 묶어서 한번에 기능 추가하기 쉽다.)
newBrick : loop가 돌 때마다 그룹에 추가된 새로운 brick
brickInfo : 우리가 필요한 데이터를 저장해둘 곳

### Rendering the brick image

~~~
game.load.image('brick', 'img/brick.png');
~~~

### Drawing the bricks

우리는 initBrikcs라는 함수에서 벽돌 그리기를 관리할 것이다.

~~~
    function initBricks() {
        birckInfo = {
            width: 50,
            height: 20,
            count: {
                row: 3,
                col: 7
            },
            offset: {
                top: 50,
                left: 60
            },
            padding: 10
        };
    }
~~~
이러한 함수를 따로 작성해보자

offset은 canvas로부터 얼마나 떨어져서 bricks를 그려내기 시작할 것인지를 의미하고
padding은 brick간의 간격을 의미한다.

initBricks 함수에 벽돌들을 다 한곳에 묶기 위한 bricks를 정의한다.

~~~
bricks = game.add.group();
~~~

이제 벽돌들을 하나씩 그려보자

~~~
for(c = 0; c < brickInfo.count.col; c++){
    for(r = 0; r< brickInfo.count.row; r++){
        brickX = c * (brickInfo.width + brickInfo.padding) + brickInfo.offset.left;
        brickY = r * (brickInfo.height + brickInfo.padding) + brickInfo.offset.top;
        newBrick = game.add.sprite(brickX, brickY, 'brick');
        game.physics.enable(newBrick, Phaser.Physics.ARCADE);
        newBrick.body.immovable = true;
        newBrick.anchor(0.5);
        bricks.add(newBrick);
        }
    }
~~~
