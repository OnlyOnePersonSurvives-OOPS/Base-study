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

