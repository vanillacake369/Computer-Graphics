# Snake Game 분석 & 변형

# 팀 👥

효율성을 위하여 분할된 작업에 따라 팀을 세분화해보았습니다.

## Team A

- 임지훈 [팀장]
- 권도영

> 순서도
기능별 수행역할 및 과정 나열
> 

## Team B

- 배우석
- 김성현
- 최혜령

> 변형
변형 내용 상세 나열
> 

---

# 순서도✍️

![flow chart of snake game.drawio.png](Snake%20Game%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20&%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%209c9f8fbb9fcc41a3b2b67cff0f3a6f5c/flow_chart_of_snake_game.drawio.png)

---

# 각 함수의 역할 📔

## setup() ⚙️

> 화면, 게임 초기값 셋팅
> 
1. 화면 구성을 한다.
2. s <- Snake객체
3. pickLocation()을 통해 초기 food가 나타날 곳을 지정한다.

## draw() 🖌️

> 화면, 실제 게임 실행
> 
1. 화면 구성을 한다.
2. Snake Game 점수 출력한다.
3. s가 food를 먹는다면 다음 food가 나타날 곳을 지정한다.
4. s.death() 호출
s.update() 호출
s.show() 호출
5. fill()을 통해 핑크색으로 지정한 뒤, rect()를 통해 food 화면에 출력한다.

## pickLocation() 📌

> 어느 랜덤한 위치에 음식을 지정할 것인지 고르기
> 
1. 게임 화면의 행과 열 생성
2. 행과 열 사이의 난수 생성하여 벡터 생성
3. mult()를 통해 벡터 각 원소에 곱셈
4. tail 각 원소에 대해 food에 닿으면(d<1)이면 자기 자신 호출( pickLocation() )

## scoreboard() 💯

> 점수판 구현
> 
1. (600,40) 짜리 검정사각형 (0,600)에 생성
2. textFont "Georgia" textSize 18로 지정
3. "Score" (10,625) 에 출력
"HighScore" (450,625)에 출력
뱀의 현재 점수 (70,625)에 출력
뱀의 최고 점수 (540,625)에 출력

## keyPressed() ⌨️

> 사용자 입력에 따른 뱀 동작
> 
1. if UP : s.dir(0,-1)
2. if DOWN : s.dir(0,1)
3. if RIGHT : s.dir(1,0)
4. if LEFT : s.dir(-1,0)

## Snake Object 🐍

> 상하좌우로 움직이며, // dir()
음식을 먹음에 따라 점수 최신화되고, // eat()
꼬리가 길어지고, // update()
화면에 보여지고, // show()
닿으면 점수 초기화된다. // death()
> 

### field var

*// position of Snake*

- x
- y

*// how fast(how much cell does this Snake goes)*

- xspeed
- yspeed

*// tail of Snake*

- tail

*// score*

- total
- highscore
- score

### function : declared as anonymous function

- **dir(x,y)**

> 이동속도 지정
> 
- **eat(pos)**

> 음식 먹은 후 꼬리수, 현 점수 증가
> 

> 현 점수 > 최고점수인 경우, 현 점수로 최고점수 최신화
> 
- **death()**

> 꼬리에 닿게 되면 꼬리수, 현 점수, 꼬리 자체 초기화
> 
- **update()**

> 꼬리 추가
> 

> 뱀의 꼬리에도 방향성 부여
> 
- **show()**

> 뱀 객체 화면에 출력
> 

---

# 각 함수의 동작과정 📄

## 각 함수 설명 양식

<aside>
💡 function_name()
- code
- parameter (이름:타입)
- role
- borrowed function

</aside>

---

## setup()

### code

```jsx
function setup() {
  createCanvas(playfield, 640);
  background(51);
  s = new Snake();
  frameRate (10);
  pickLocation();
}
```

### variable

- s : Snake
→ 스네이크 객체 참조
- playfield : 전역변수 
→ 캔버스 크기 변수

### role

- 화면 구성
- Snake() 호출 → Snake객체 생성
- pickLocation() 호출 → 초기 food가 나타날 곳 지정

### borrowed function

- createCanvas(playfield, 640);
: 캔버스를 생성하고 픽셀 단위로 크기를 설정
- background(51);
: charcoal grey background를 가진 캔버스 설정
- frameRate (10);
: 화면에 나타날 프레임 수를 10 초단위 새로 고침

---

## draw()

### code

```jsx
function draw() {
  background(51);
  scoreboard();
  if (s.eat(food)) {
    pickLocation();
  }
  s.death();
  s.update();
  s.show();

  fill (255,0,100);
  rect(food.x,food.y, scl, scl);
}
```

### variable

- s : Snake
→ 스네이크 객체 참조
- food : var
→ 음식 칸 위치 (랜덤한 cols + 랜덤한 rows)

### role

- Snake Game 점수 출력
- Snake Game 실행
- Snake 객체 관련 함수 호출
- food 화면에 출력

### borrowed function

- background(51);
→ charcoal grey background를 가진 캔버스 설정

---

## pickLocation()

### code

```jsx
function pickLocation() {
  var cols = floor(playfield/scl);
  var rows = floor(playfield/scl);
  food = createVector(floor(random(cols)), floor(random(rows)));
  food.mult(scl);

  // Check the food isn't appearing inside the tail

  for (var i = 0; i < s.tail.length; i++) {
    var pos = s.tail[i];
    var d = dist(food.x, food.y, pos.x, pos.y);
    if (d < 1) {
      pickLocation();
    }
  }
}
```

### variable

- cols
→ playfield(600)/ scl(20)=전체 너비/한 칸의 너비 => 너비 칸 개수(30개)
- rows
→ playfield(600)/ scl(20)=전체 너비/한 칸의 너비 => 길이 칸 개수(30개)
- food
→ 음식 칸 위치 (랜덤한 cols + 랜덤한 rows)
- pos
→ 꼬리의 position
- d
→ food.x, food.y(음식 칸 위치)와 pos.x , pos.y(꼬리들의 위치) 거리  d

### role

- 거리 d가 1보다 낮은 0이면 다시 실행, 
즉, 음식 칸 위치와 꼬리들의 위치가 같으면 겹침 >> pickLocation(); 다시 실행

### borrowed function

- floor()
→ 매개변수의 값보다 작거나 같은 수들 중, 가장 가까운 정수(int)값을 계산
- createVector()
→ 2차원 및 3차원 벡터, 특히 유클리드 (기하학) 벡터 생성
- mult() : Vector() 원소에 원하는 값만큼 곱함
- random(rows) 
→ 0~rows 사이 난수 반환

---

## scoreboard()

### code

```jsx
function scoreboard() {
  fill(0);
  rect(0, 600, 600, 40);
  fill(255);
  textFont("Georgia");
  textSize(18);
  text("Score: ", 10, 625);
  text("Highscore: ", 450, 625)
  text(s.score, 70, 625);
  text(s.highscore, 540, 625)
}
```

### variable

- s.score 현재 점수
- s.highscore 최고 점수

### role

- 화면 최하단에 현재 점수, 최고 점수 출력

### borrowed function

- fill()
- rect() 
→ rect을 이용하여 0, 600에 600*40크기의 사각형을 그려준다.
- textFont()
→ 폰트설정 "Georgia”
- textSize()
→ 텍스트 크기 18
- text()

---

## keyPressed()

### code

```jsx
function keyPressed() {
  if (keyCode === UP_ARROW){
      s.dir(0, -1);
  }else if (keyCode === DOWN_ARROW) {
      s.dir(0, 1);
  }else if (keyCode === RIGHT_ARROW) {
      s.dir (1, 0);
  }else if (keyCode === LEFT_ARROW) {
      s.dir (-1, 0);
  }
}
```

### variable

- UP_ARROW
- DOWN_ARROW
- RIGHT_ARROW
- LEFT_ARROW

### role

- 조건문 if else사용 Snake()의 dir 호출
    - UP_ARROW x좌표로 0칸씩 y좌표로 -1칸씩 이동 (위로 이동)
    DOWN_ARROW x좌표로 0칸씩 y좌표로 1칸씩 이동 (아래로 이동)
    RIGHT_ARROW x좌표로 1칸씩 y좌표로 0칸씩 이동 (우로 이동)
    LEFT_ARROW x좌표로 -1칸씩 y좌표로 -0칸씩 이동 (좌로 이동)

### borrowed function

- keyPressed
- keyCode
- UP_ARROW
- DOWN_ARROW
- RIGHT_ARROW
- LEFT_ARROW

---

## Snake Object

### code

```jsx
function Snake() {
  this.x =0;
  this.y =0;
  this.xspeed = 1;
  this.yspeed = 0;
  this.total = 0;
  this.tail = [];
  this.score = 1;
  this.highscore = 1;

  this.dir = function(x,y) {
    this.xspeed = x;
    this.yspeed = y;
  }

  this.eat = function(pos) {
    var d = dist(this.x, this.y, pos.x, pos.y);
    if (d < 1) {
      this.total++;
      this.score++;
      text(this.score, 70, 625);
      if (this.score > this.highscore) {
        this.highscore = this.score;
      }
      text(this.highscore, 540, 625);
      return true;
    } else {
      return false;
    }
  }

  this.death = function() {
    for (var i = 0; i < this.tail.length; i++) {
      var pos = this.tail[i];
      var d = dist(this.x, this.y, pos.x, pos.y);
      if (d < 1) {
        this.total = 0;
        this.score = 0;
        this.tail = [];
      }
    }
  }

  this.update = function(){
    if (this.total === this.tail.length) {
      for (var i = 0; i < this.tail.length-1; i++) {
        this.tail[i] = this.tail[i+1];
    }

    }
    this.tail[this.total-1] = createVector(this.x, this.y);

    this.x = this.x + this.xspeed*scl;
    this.y = this.y + this.yspeed*scl;

    this.x = constrain(this.x, 0, playfield-scl);
    this.y = constrain(this.y, 0, playfield-scl);

  }
  this.show = function(){
    fill(255);
    for (var i = 0; i < this.tail.length; i++) {
        rect(this.tail[i].x, this.tail[i].y, scl, scl);
    }

    rect(this.x, this.y, scl, scl);
  }
}
```

### instance variable

- this.x, this.y, this.xspeed, this.yspeed, this.dir //방향 조작
- this.tail[ ] //뱀의 꼬리
- this.score //현재 먹은 음식를 계산
- this.highscore //최고 점수를 계산
- this.total = 0; // 먹은 음식에 의한 늘어난 꼬리수

## dir() :: Snake

### code

```jsx
this.dir = function(x,y) {
    this.xspeed = x;
    this.yspeed = y;
  }
```

### parameter

- x,y

### variable

- this.x, this.y, this.xspeed, this.yspeed, this.dir 
→ 방향 조작을 위해 만들어 낸 변수

### role

- 입력받은 x,y를 통해 x축,y축 스피드 변수에 입력

### borrowed function

X

---

## eat() :: Snake

### code

```jsx
this.eat = function(pos) {
    var d = dist(this.x, this.y, pos.x, pos.y);
    if (d < 1) {
      this.total++;
      this.score++;
      text(this.score, 70, 625);
      if (this.score > this.highscore) {
        this.highscore = this.score;
      }
      text(this.highscore, 540, 625);
      return true;
    } else {
      return false;
    }
  }
```

### parameter

- pos

### variable

- d 
→ 뱀의 위치와 pos(←food) 위치 간의 거리

### role

function(pos)를 가지고 온다. 만약 dist를 사용하여 뱀의 머리와 food의 거리가 1보다 낮을때는 겹칠때를 의미한다. 이때 score의 점수와 tatal을 1씩 더하여 표시한다. 이때 Score이 Highscore보다 높으면 Highscore을 Score과 같게 만든다.

### borrowed function

---

## death() :: Snake

### code

```jsx
this.death = function() {
    for (var i = 0; i < this.tail.length; i++) {
      var pos = this.tail[i];
      var d = dist(this.x, this.y, pos.x, pos.y);
      if (d < 1) {
        this.total = 0;
        this.score = 0;
        this.tail = [];
      }
    }
  }
```

### variable

- pos
→ 각 tail의 원소 지정
- d
→ 뱀과 pos 간의 거리

### role

- 변수 pos에 꼬리배열를 저장하고 dist를 사용하여 현재의 위치가 꼬리 배열 중 하나와 거리가 0이하가 되면 종료하는 방식이다

### borrowed function

- dist()

---

## update() :: Snake

### code

```jsx
this.update = function(){
    if (this.total === this.tail.length) {
      for (var i = 0; i < this.tail.length-1; i++) {
        this.tail[i] = this.tail[i+1];
    }

    }
    this.tail[this.total-1] = createVector(this.x, this.y);

    this.x = this.x + this.xspeed*scl;
    this.y = this.y + this.yspeed*scl;

    this.x = constrain(this.x, 0, playfield-scl);
    this.y = constrain(this.y, 0, playfield-scl);

  }
```

### variable

- this.x
- this.y
- this.xspeed
- this.yspeed
- scl
- playfield
- scl

### role

현재 이전의 위치값(this.x, this.y)을 꼬리 배열에 total-1(뱀의 머리) 갯수만큼 저장한다. 저장할때 꼬리도 방향 전환을 해줘야 하기 때문에 this.xspeed**scl와 this.yspeed**scl을 더해준다

### borrowed function

- createVector
- constrain
// Constrains a value between a minimum and maximum value

---

## show() :: Snake

### code

```jsx
this.show = function(){
    fill(255);
    for (var i = 0; i < this.tail.length; i++) {
        rect(this.tail[i].x, this.tail[i].y, scl, scl);
    }

    rect(this.x, this.y, scl, scl);
  }
```

### variable

- this.x
- this.y
- scl

### role

반복문 for을 이용하여 꼬리를 사각형으로 표시한다. 가로세로길이는 scl이고 위치는 꼬리배열에 저장해 놓은 값을 사용한다. 이때 현재위치인 머리는 따로 표현한다.

### borrowed function

- fill(255);
- rect(this.x, this.y, scl, scl);

---

# Modification ⛏️

## 기존 게임에서의 변경점

> Snake() 객체를 이용하여 뱀을 하나 더 만들어 적 역할을 하게 했다.
적은 random()으로 이동 경로를 결정하여 무작위로 이동하게끔 하였으며
플레이어는 벽에 부딫히는 것, 꼬리 칸이 둘 이상일 때 이동 방향의 정 반대로 이동하는 것 이외에도 적과 부딫히는 것으로도 사망하여 게임이 끝나게 하였다.

한편 게임 중 적은 플레이어처럼 사망하지 않고, '움직이는 장애물' 역할을 해야 하기 때문에 적에 대해서는 death()는 불러오지 않는다.

또한 적의 움직임을 좀 더 자연스럽게 하기 위해 머리가 몸체와 겹치지 않게 하였다.
> 

## 게임 시작 과정

### 게임 시작화면

![menu.jpg](Snake%20Game%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20&%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%209c9f8fbb9fcc41a3b2b67cff0f3a6f5c/menu.jpg)

- r 이나 엔터누르면 시작

### 동작화면

![Untitled](Snake%20Game%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20&%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%209c9f8fbb9fcc41a3b2b67cff0f3a6f5c/Untitled.png)

- 노란색 뱀
→ 플레이어
- 빨간색 뱀
→ 적
- 핑크색 음식

### 게임 오버 화면

![Untitled](Snake%20Game%20%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8%20&%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A7%E1%86%BC%209c9f8fbb9fcc41a3b2b67cff0f3a6f5c/Untitled%201.png)

- m누르면 시작화면으로 리셋

## Code

```jsx
//Delcare Global Variables
var s;
var scl = 20;
var food;
var enemyMoveTempX;
var enemyMoveTempY;
var enemyMoveValueX;
var enemyMoveValueY;
var enemyLength = 6;
var playfield = 600;
let mode = 0;
let menu;
let gameover;
const MODE_MENU = 0;
const MODE_IN_GAME = 1;
const MODE_GAMEOVER = 2;

function preload() {				//이미지를 연결해준다
  menu = loadImage('menu.jpg');
  gameover = loadImage('gameover.jpg');
}

// p5js Setup function - required

function setup() {
  createCanvas(playfield, 640);
  background(51);
  s = new Snake();
  enemy = new Snake();
  enemyPreset();
  frameRate (10);
  pickLocation();
}

// p5js Draw function - required

function draw() {
  if (mode == MODE_MENU) {			//메뉴
    drawMenu();
  }
  if (mode == MODE_IN_GAME) {			//인게임
    drawGame();
  }
  if (mode == MODE_GAMEOVER) {			//게임오버
    drawGameover();
  }
}

function drawMenu() {
  image(menu, 0, 0, 600, 640);			//메뉴 이미지
}

function drawGameover() {
  image(gameover, 0, 0, 600, 640);			//게임오버 이미지
}

function drawGame() {
  background(51);
  scoreboard();
  if (s.eat(food)) {
    pickLocation();
  }
  if (frameCount%2==0) {				//2프레임 간격으로 실행
    enemyDir();					//적의 방향을 정해준다
  } else {
    if (enemyDirLimit()) {				//적의 방향이 제한된 곳으로 이동할 때
      enemyDir();
    }
  }
  if (enemy.total < enemyLength) {			//적의 꼬리길이 만큼 몸을 늘린다.
    enemy.total++;
  }

  s.death(enemy);
  s.update();
  enemy.update();
  s.show();
  enemy.show();

  fill (255, 0, 100);
  rect(food.x, food.y, scl, scl);
  noFill();
}

function enemyPreset() {				//적 세팅
  enemy.x = 14*scl;					//x 15번째 칸
  enemy.y = 14*scl;				//y 15번째 칸
  enemy.col = createVector(255, 0, 0);			//색깔
  enemy.lineCol = createVector(0, 127, 0);		//줄무늬색깔(원래 색깔과 차이)
}

function enemyDir() {
  do {						//적의 방향을 정해준다
    enemyMoveTempX = round(random(-1, 1));
    enemyMoveTempY = abs(abs(enemyMoveTempX)-1)*((floor(random(-1, 1))+0.5)*2);
    enemy.dir(enemyMoveTempX, enemyMoveTempY);
  } while (enemyDirLimit());				//적의 방향이 제한된 곳으로 이동하면 다시한다
  enemyMoveValueX = enemyMoveTempX;		//실제로 움직인 x값 저장
  enemyMoveValueY = enemyMoveTempY;		//실제로 움직인 y값 저장
}

function enemyDirLimit() {				//적의 방향 제한
  if ((enemyMoveTempX != 0 && enemyMoveValueX == -enemyMoveTempX) ||	//진행방향 x축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
    (enemyMoveTempX == 0 && enemyMoveValueY == -enemyMoveTempY)) {	//진행방향 y축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
    return true;
  }
  if (enemy.x+enemyMoveTempX*scl<0 || enemy.x+enemyMoveTempX*scl > playfield-scl  ||	//x값이 맵 밖으로 나갈 때
    enemy.y+enemyMoveTempY*scl<0 || enemy.y+enemyMoveTempY*scl > playfield-scl) {	//y값이 맵 밖으로 나갈 때
    return true;
  }
  for (var i = 0; i < enemy.tail.length; i++) {		//적의 모든 꼬리 비교
    var posEnemy = enemy.tail[i];
    var d = dist(enemy.x + enemyMoveTempX*scl, enemy.y + enemyMoveTempY*scl, posEnemy.x, posEnemy.y);
    if ( d < 1) {					//적의 머리와 각 꼬리 중 하나가 만날 때
      return true;
    }
  }
  return false;
}

// Pick a location for food to appear

function pickLocation() {
  var cols = floor(playfield/scl);
  var rows = floor(playfield/scl);
  food = createVector(floor(random(cols)), floor(random(rows)));
  food.mult(scl);

  // Check the food isn't appearing inside the tail

  for (var i = 0; i < s.tail.length; i++) {
    var pos = s.tail[i];
    var d = dist(food.x, food.y, pos.x, pos.y);
    if (d < 1) {
      pickLocation();
    }
  }
}

// scoreboard

function scoreboard() {
  fill(0);
  rect(0, 600, 600, 40);
  fill(255);
  textFont("Georgia");
  textSize(18);
  text("Score: ", 10, 625);
  text("Highscore: ", 450, 625);
  text(s.score, 70, 625);
  text(s.highscore, 540, 625);
  noFill();
}

// CONTROLS function

function keyPressed() {
  if (keyCode === UP_ARROW) {
    s.dir(0, -1);
  } else if (keyCode === DOWN_ARROW) {
    s.dir(0, 1);
  } else if (keyCode === RIGHT_ARROW) {
    s.dir (1, 0);
  } else if (keyCode === LEFT_ARROW) {
    s.dir (-1, 0);
  }
  if (keyCode === ENTER || key === 'r' || key === 'R') {		//엔터, r, R을 입력할 때
    s.reset();						//s 리셋
    enemy.reset();						//enemy 리셋
    enemyPreset();						//enemy 프리셋 적용
    mode = MODE_IN_GAME;					//모드는 인게임(const 1)
  }
  if ( key === 'm' || key === 'M') {				//m, M을 입력 할 때
    mode = MODE_MENU;					//모드는 메뉴(const 0)
  }
}

// SNAKE OBJECT

function Snake() {
  this.x =3*scl;				//4번째 칸
  this.y =3*scl;				//4번째 칸
  this.xspeed = 0;
  this.yspeed = 0;
  this.total = 0;
  this.tail = [];
  this.score = 0;
  this.highscore = 0;
  this.col = createVector(255, 255, 0);		//색깔
  this.lineCol = createVector(0, -127, 0);	//줄무늬 색깔(원래 색깔과 차이)

  this.dir = function(x, y) {
    this.xspeed = x;
    this.yspeed = y;
  }

  this.reset = function() {		//생성자 값으로 변경
    this.x = 3*scl;
    this.y = 3*scl;
    this.xspeed = 0;
    this.yspeed = 0;
    this.total = 0;
    this.tail = [];
    this.score = 0;
  }

  this.eat = function(pos) {
    var d = dist(this.x, this.y, pos.x, pos.y);
    if (d < 1) {
      this.total++;
      this.score++;
      text(this.score, 70, 625);
      if (this.score > this.highscore) {
        this.highscore = this.score;
      }
      text(this.highscore, 540, 625);
      return true;
    } else {
      return false;
    }
  }

  this.death = function(enemy) {
    var d;
    var d2;
    if (this.x + this.xspeed*scl < 0 || this.x + this.xspeed*scl>playfield-scl ||	//x값이 맵밖으로 나갈때
      this.y + this.yspeed*scl < 0 || this.y + this.yspeed*scl>playfield-scl) {	//y값이 맵밖으로 나갈때
      this.reset();
      enemy.reset();
      enemyPreset();
      mode = MODE_GAMEOVER;
    }
    for (var i = 0; i < this.tail.length; i++) {		//모든 꼬리 비교
      var pos = this.tail[i];
      d = dist(this.x, this.y, pos.x, pos.y);
      d2 = dist(enemy.x, enemy.y, pos.x, pos.y);
      if (d < 1 || d2 < 1) {	//자신 머리와 자신의 꼬리가 만날 때, 적의 머리와 자신의 꼬리가 만날 때
        this.reset();
        enemy.reset();
        enemyPreset();
        mode = MODE_GAMEOVER;
      }
    }
    d = dist(this.x, this.y, enemy.x, enemy.y);
    if ( d < 1) {		//자신의 머리와 적의 머리가 만날 때
      this.reset();
      enemy.reset();
      enemyPreset();
      mode = MODE_GAMEOVER;
    }
    for (var j=0; j<enemy.tail.length; j++) {
      var enemyPos = enemy.tail[j];
      d = dist(this.x, this.y, enemyPos.x, enemyPos.y);
      if ( d < 1) {		//자신의 머리와 적의 꼬리가 만날 때
        this.reset();
        enemy.reset();
        enemyPreset();
        mode = MODE_GAMEOVER;
      }
    }
  }

  this.update = function() {
    if (this.total === this.tail.length) {
      for (var i = 0; i < this.tail.length-1; i++) {
        this.tail[i] = this.tail[i+1];
      }
    }
    this.tail[this.total-1] = createVector(this.x, this.y);

    this.x = this.x + this.xspeed*scl;
    this.y = this.y + this.yspeed*scl;

    this.x = constrain(this.x, 0, playfield-scl);
    this.y = constrain(this.y, 0, playfield-scl);
  }

  this.show = function() {
    for (var i = 0; i < this.tail.length; i++) {
      fill(this.col.x, this.col.y+i%3/2*this.lineCol.y, this.col.z);
      rect(this.tail[i].x, this.tail[i].y, scl, scl);
    }
    fill(this.col.x, this.col.y, this.col.z);
    rect(this.x, this.y, scl, scl);
  }
}
```

---

# Work Process of Modified Snake Game 📝

## 1. 메뉴(m or M), 인게임(r or R or ENTER), 게임오버 분류

```jsx
function draw() {
  if (mode == MODE_MENU) {			//메뉴
    drawMenu();
  }
  if (mode == MODE_IN_GAME) {			//인게임
    drawGame();
  }
  if (mode == MODE_GAMEOVER) {			//게임오버
    drawGameover();
  }
}

function drawMenu() {
  image(menu, 0, 0, 600, 640);			//메뉴 이미지
}

function drawGameover() {
  image(gameover, 0, 0, 600, 640);			//게임오버 이미지
}

function drawGame() {
  background(51);
  scoreboard();
  if (s.eat(food)) {
    pickLocation();
  }
  if (frameCount%2==0) {				//2프레임 간격으로 실행
    enemyDir();					//적의 방향을 정해준다
  } else {
    if (enemyDirLimit()) {				//적의 방향이 제한된 곳으로 이동할 때
      enemyDir();
    }
  }
  if (enemy.total < enemyLength) {			//적의 꼬리길이 만큼 몸을 늘린다.
    enemy.total++;
  }

  s.death(enemy);
  s.update();
  enemy.update();
  s.show();
  enemy.show();

  fill (255, 0, 100);
  rect(food.x, food.y, scl, scl);
  noFill();
}
```

## 2. 적 프리셋

1.  x 15번째 
2. y 15번째 칸
3. 빨간색
4. 주황 줄무늬

```jsx
function enemyPreset() {				//적 세팅
  enemy.x = 14*scl;					//x 15번째 칸
  enemy.y = 14*scl;				//y 15번째 칸
  enemy.col = createVector(255, 0, 0);			//색깔
  enemy.lineCol = createVector(0, 127, 0);		//줄무늬색깔(원래 색깔과 차이)
}
```

## 3. 적의 방향 지정 (1,0) or (-1,0) or (0,1) or (0,-1)

```jsx
function enemyDir() {
  do {						//적의 방향을 정해준다
    enemyMoveTempX = round(random(-1, 1));
    enemyMoveTempY = abs(abs(enemyMoveTempX)-1)*((floor(random(-1, 1))+0.5)*2);
    enemy.dir(enemyMoveTempX, enemyMoveTempY);
  } while (enemyDirLimit());				//적의 방향이 제한된 곳으로 이동하면 다시한다
  enemyMoveValueX = enemyMoveTempX;		//실제로 움직인 x값 저장
  enemyMoveValueY = enemyMoveTempY;		//실제로 움직인 y값 저장
}
```

## 4. 적 방향 제한

1. x값이 맵 밖으로 나갈 때
2. y값이 맵 밖으로 나갈 때
3. 진행방향 x축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
4. 진행방향 y축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
5. 적의 머리와 적의 꼬리 중 하나가 만날 때

```jsx
function enemyDirLimit() {				//적의 방향 제한
  if ((enemyMoveTempX != 0 && enemyMoveValueX == -enemyMoveTempX) ||	//진행방향 x축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
    (enemyMoveTempX == 0 && enemyMoveValueY == -enemyMoveTempY)) {	//진행방향 y축에서 정반대로 움직일 때 (머리가 꼬리에 겹침)
    return true;
  }
  if (enemy.x+enemyMoveTempX*scl<0 || enemy.x+enemyMoveTempX*scl > playfield-scl  ||	//x값이 맵 밖으로 나갈 때
    enemy.y+enemyMoveTempY*scl<0 || enemy.y+enemyMoveTempY*scl > playfield-scl) {	//y값이 맵 밖으로 나갈 때
    return true;
  }
  for (var i = 0; i < enemy.tail.length; i++) {		//적의 모든 꼬리 비교
    var posEnemy = enemy.tail[i];
    var d = dist(enemy.x + enemyMoveTempX*scl, enemy.y + enemyMoveTempY*scl, posEnemy.x, posEnemy.y);
    if ( d < 1) {					//적의 머리와 각 꼬리 중 하나가 만날 때
      return true;
    }
  }
  return false;
}
```

## 5. 자신이 죽는 경우

1. x값이 맵밖으로 나갈때
2. y값이 맵밖으로 나갈때
3. 자신 머리와 자신의 꼬리가 만날 때, 적의 머리와 자신의 꼬리가 만날 때
4. 자신의 머리와 적의 머리가 만날 때
5. 자신의 머리와 적의 꼬리가 만날 때

```jsx
 this.death = function(enemy) {
    var d;
    var d2;
    if (this.x + this.xspeed*scl < 0 || this.x + this.xspeed*scl>playfield-scl ||	//x값이 맵밖으로 나갈때
      this.y + this.yspeed*scl < 0 || this.y + this.yspeed*scl>playfield-scl) {	//y값이 맵밖으로 나갈때
      this.reset();
      enemy.reset();
      enemyPreset();
      mode = MODE_GAMEOVER;
    }
    for (var i = 0; i < this.tail.length; i++) {		//모든 꼬리 비교
      var pos = this.tail[i];
      d = dist(this.x, this.y, pos.x, pos.y);
      d2 = dist(enemy.x, enemy.y, pos.x, pos.y);
      if (d < 1 || d2 < 1) {	//자신 머리와 자신의 꼬리가 만날 때, 적의 머리와 자신의 꼬리가 만날 때
        this.reset();
        enemy.reset();
        enemyPreset();
        mode = MODE_GAMEOVER;
      }
    }
    d = dist(this.x, this.y, enemy.x, enemy.y);
    if ( d < 1) {		//자신의 머리와 적의 머리가 만날 때
      this.reset();
      enemy.reset();
      enemyPreset();
      mode = MODE_GAMEOVER;
    }
    for (var j=0; j<enemy.tail.length; j++) {
      var enemyPos = enemy.tail[j];
      d = dist(this.x, this.y, enemyPos.x, enemyPos.y);
      if ( d < 1) {		//자신의 머리와 적의 꼬리가 만날 때
        this.reset();
        enemy.reset();
        enemyPreset();
        mode = MODE_GAMEOVER;
      }
    }
  }
```

---

# Reference 📖

JS 변수 선언방법

[https://medium.com/@wainy254/변수-선언-c696c3b9e787](https://medium.com/@wainy254/%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8-c696c3b9e787)