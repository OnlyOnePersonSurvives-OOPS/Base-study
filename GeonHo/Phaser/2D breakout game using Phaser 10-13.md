# 2D breakout game using Phaser 10 -13

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

그렇다면 ballHitBrick이 실행되었을 때 벽돌이 없어지는 코드를 작성해보자.
phaser 프레임워크를 쓰면 이또한 정말 간단하다.

~~~
function ballHitBrick(ball, brick) {
    brick.kill();
}
~~~

brick.kill(); 하나면 충분하다!

---

## 11. 점수

### 변수 선언

~~~
var scoreText;
var score = 0;
~~~

### 화면에 점수 표시하기

~~~
scoreText = game.add.text(5, 5, 'Points: 0', { font: '18px Arial', fill: '#0095DD' });
~~~
위치 좌표  
텍스트  
폰트, 크기, 색상

### 벽돌이 깨지면 점수 올리기

~~~
scoreText = game.add.text(5, 5, 'Points: ' + score, { font: '18px Arial', fill: '#0095DD' });
~~~
이렇게 하면 되지 않을까 해서 해봤지만  
안된다. 

원하는 결과를 내기 위해선  
setText method를 사용해야 한다.  

~~~
score += 10;
scoreText.setText('Points : ' + score);
~~~

를 ballHitBrick함수에 추가시키자

---

## 12. Clear!

### 승리 조건

- bricks.children.length : 벽돌의 개수 (21)
- bricks.children[20] : (object Object)
- bricks.children[20].alive : 그 벽돌이 존재하는지의 여부 (bool)

그냥 ballHitBrick 함수에
~~~
count_alive -= 1;

if(count_alive == 0){
    alert('win!');
}
~~~
를 넣어봤는데 됐다.  

그러나 튜토리얼은 그렇지 않다.  


~~~
    function ballHitBrick(ball, brick) {
        brick.kill();
        score += 10;
        scoreText.setText('Points: ' + score);

        var count_alive = 0;

        for(i = 0; i < bricks.children.length; i++){
            if (bricks.children[i].alive == true){
                count_alive += 1
            }
        }

        if (count_alive == 0){
            alert('You won the game, congraturations!');
            location.reload();
        }
    }
~~~
이런식으로 벽돌이 깨질 때 마다 for문을 돌며 count_alive를 설정해주고 있다.
왜 이렇게 하는지는 잘 모르겠다.


---

## 13. 남은 목숨

### 변수 선언

~~~
var lives = 3;
var livesText;
var lifeLostText;
~~~
- lives : 남은 목숨 수
- livesText : 화면에 표시할 목숨 텍스트
- lifeLostText : 공을 흘렸을 때 표시될 텍스트

### 새로운 텍스트 만들기

~~~
livesText = game.add.text(game.world.width - 5, 5, 'Lives: ' + lives, { font: '18px Arial', fill: '#0095DD' });
livesText.anchor.set(1, 0);

lifeLostText = game.add.text(game.world.width * 0.5, game.world.height * 0.5, 'Life lost, click to continue', { font: '18px Arial', fill: '#0095DD' });
lifeLostText.anchor.set(0.5);
lifeLostText.visible = false;
~~~

livesText의 anchor를 1, 0으로 세팅한 이유는 points와 대칭되게끔 하기 위해서다.  
lifeLostText는 특정 조건에서만 떠야하니 visible을 false로 설정해둔다.  

근데 여기서 fontStyle변수가 겹치는 것을 볼 수 있다.  

~~~
textStyle = { font: '18px Arial', fill: '#0095DD' };
~~~

새로운 변수로 지정해서 중복을 깔끔하게 정리하자.

### The lives handling code

기존의
~~~
ball.events.onOutOfBounds.add(function(){
    alert('Game over!');
    location.reload();
}, this);
~~~
를

~~~
ball.events.onOutOfBounds.add(ballLeaveScreen, this);
~~~
이렇게 바꾸고 새로운 함수를 정의해보자

~~~
function ballLeaveScreen() {
    lives--;
    if(lives) {
        livesText.setText('Lives: '+lives);
        lifeLostText.visible = true;
        ball.reset(game.world.width*0.5, game.world.height-25);
        paddle.reset(game.world.width*0.5, game.world.height-5);
        game.input.onDown.addOnce(function(){
            lifeLostText.visible = false;
            ball.body.velocity.set(150, -150);
        }, this);
    }
    else {
        alert('You lost, game over!');
        location.reload();
    }
}
~~~

공을 흘리면 lives가 깎이고 목숨을 잃었단 텍스트를 띄우고 공과 패들을 원점으로 세팅해준다.

여기서 알아야할 것은  
game.input.onDown.addOnce(function());  
이다.  

만약 인풋이 되었을 때(mouse or touch) 인자의 함수를 한번 실행해준다.

### Events

add와 addOnce의 차이점  
- add : 이벤트가 발생할 때마다 실행된다.
- addOnce : 한번만 실행하고 다시 발생되지 않는다.


이를 직관적으로 체감하기 위해선  
create()의 ball.events.onOutOfBounds.**add**(ballLeaveScreen, this);  
ballLeaveScreen()의 game.input.onDown.**addOnce**(function());  

bold를 addOnce로 바꿔도보고 add로도 바꿔보면 알 수 있다.  

- 둘 다 addOnce인 경우
-> ballLeaveScreen함수가 한번밖에 실행되지 않는다.
-> 한번 공을 흘렸을 땐 제대로 표시되는데 그다음부턴 공이 흘러도 쭉 지나간다.

- 둘 다 add인 경우
-> 한번 공을 흘린 뒤, 공이 벽에 튕길 때마다(공이 벽을 지나갈 때마다 ball  input을 주면  
lifeLostText.visible = false;  
ball.body.velocity.set(150, -150);  
가 계속해서 실행된다.  

근데 왜 벽에 튕길때마다 인진 모르겠다.  

